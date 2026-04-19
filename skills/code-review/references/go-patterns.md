# Go Patterns — Reference for Code Review

Load this when reviewing Go code involving concurrency, HTTP handlers, database access, or context usage. Each section lists patterns to flag, *why* they matter, and the idiomatic fix.

---

## 1. Context Propagation

### Flag
- Function accepts no `context.Context` but calls the DB, an HTTP client, or a gRPC stub.
- `context.Background()` / `context.TODO()` created deep in the call stack instead of receiving one from the caller.
- `ctx` stored on a struct field (should be passed as an argument).
- Goroutine launched from a request handler without passing (or deriving from) the request's `ctx`.

### Why
Without ctx propagation, cancellation and deadlines don't reach downstream calls — one slow dependency can pin goroutines and file descriptors until OOM.

### Fix
```go
// Bad
func (s *Service) Fetch(id string) (*User, error) {
    return s.db.QueryRow("SELECT ... WHERE id = $1", id).Scan(...)
}

// Good
func (s *Service) Fetch(ctx context.Context, id string) (*User, error) {
    return s.db.QueryRowContext(ctx, "SELECT ... WHERE id = $1", id).Scan(...)
}
```

---

## 2. Goroutine Lifecycle

### Flag
- `go f()` with no way to signal cancellation or wait for completion.
- `for { go worker() }` — unbounded fan-out.
- Goroutine that writes to a channel nobody reads (or reads from a channel nobody closes).
- Missing `sync.WaitGroup` / `errgroup.Group` when the caller needs to know when work is done.
- Worker pools sized by user input instead of a fixed constant or config value.

### Fix — bounded concurrency
```go
g, ctx := errgroup.WithContext(ctx)
g.SetLimit(runtime.GOMAXPROCS(0))
for _, item := range items {
    item := item
    g.Go(func() error { return process(ctx, item) })
}
return g.Wait()
```

---

## 3. `defer` in Loops

### Flag
```go
for _, path := range paths {
    f, _ := os.Open(path)
    defer f.Close() // leaks until the enclosing function returns
    ...
}
```

### Fix
Wrap each iteration in a function so `defer` fires per-iteration:
```go
for _, path := range paths {
    if err := func() error {
        f, err := os.Open(path)
        if err != nil { return err }
        defer f.Close()
        ...
        return nil
    }(); err != nil { return err }
}
```

---

## 4. Error Wrapping

### Flag
- `return err` with no context when crossing a layer boundary.
- `fmt.Errorf("db failed: %s", err)` — uses `%s`, breaks `errors.Is` / `errors.As`.
- `if err != nil { return errors.New("db failed") }` — drops the original.
- Sentinel errors defined inline instead of exported `Err...` vars.

### Fix
```go
if err != nil {
    return fmt.Errorf("load user %q: %w", id, err)
}
```
Check with `errors.Is(err, sql.ErrNoRows)` or `errors.As(err, &pqErr)`.

---

## 5. Mutex Hygiene

### Flag
- Mutex passed or received by value (`func(m sync.Mutex)`) — makes a copy, no mutual exclusion.
- Struct with `sync.Mutex` field copied via value receiver method.
- `sync.Map` used where a regular `map[K]V` + `sync.RWMutex` would be simpler and faster (small, known key set).
- `Lock()` without `defer Unlock()` on every return path.

### Fix
Embed mutex by *value* on the struct, but pass the struct by *pointer*; always defer unlock.

---

## 6. HTTP Server

### Flag
- `http.ListenAndServe(":8080", nil)` — no timeouts. Vulnerable to slowloris.
- Handler reads `req.Body` without `io.LimitReader` — unbounded memory.
- Missing `req.Body.Close()` or response body close on outbound client calls.
- Global `http.DefaultClient` used for outbound calls — no timeout by default.
- Panics in handler without recovery middleware crash the server.
- No graceful shutdown path (`srv.Shutdown(ctx)` on SIGTERM).

### Fix — server
```go
srv := &http.Server{
    Addr:              ":8080",
    Handler:           router,
    ReadHeaderTimeout: 5 * time.Second,
    ReadTimeout:       10 * time.Second,
    WriteTimeout:      15 * time.Second,
    IdleTimeout:       60 * time.Second,
    MaxHeaderBytes:    1 << 20,
}
```

### Fix — client
```go
client := &http.Client{Timeout: 10 * time.Second}
```
Always close response bodies: `defer resp.Body.Close()`.

---

## 7. Database Access

### Flag
- `db.Query` / `db.Exec` instead of `db.QueryContext` / `db.ExecContext`.
- `rows.Close()` not deferred — connection leak.
- Missing `rows.Err()` check after the loop — errors silently dropped.
- Query strings concatenated with user input — SQL injection.
- No `SetMaxOpenConns` / `SetMaxIdleConns` / `SetConnMaxLifetime` — connection storms under load.
- Transactions without a deferred rollback guard.

### Fix — transaction
```go
tx, err := db.BeginTx(ctx, nil)
if err != nil { return err }
defer tx.Rollback() // no-op if already committed
// ... work ...
return tx.Commit()
```

---

## 8. Channels & Timers

### Flag
- `time.After(d)` in a `select` inside a loop — each call allocates a timer that isn't GC'd until it fires.
- Sending on a closed channel (panic) — usually a race where cancellation and send aren't synchronized.
- `nil` channels used accidentally (blocks forever) vs deliberately (in `select` to disable a case).
- Buffered channels used "for performance" without justification — often hides a design issue.

### Fix — per-iteration timer
```go
t := time.NewTimer(d)
defer t.Stop()
for {
    select {
    case <-ctx.Done():  return ctx.Err()
    case <-t.C:         // timeout
    case v := <-ch:     process(v); t.Reset(d)
    }
}
```

---

## 9. Slice Aliasing

### Flag
- Returning a sub-slice of an internal buffer — caller can mutate internal state.
- `append` in a library where the caller's backing array might be shared.
- Taking a pointer into a slice element inside a `for _, v := range` loop (loop variable reused prior to Go 1.22).

### Fix
Copy explicitly when handing data across an API boundary:
```go
out := make([]byte, len(internal))
copy(out, internal)
return out
```

---

## 10. Interface Design

### Flag
- Interface defined in the package that *implements* it, not the package that *uses* it (creates unnecessary coupling).
- Large "kitchen sink" interfaces with 10+ methods — hard to mock, violates ISP.
- Returning an interface where a concrete type would do (hides useful methods from the caller).
- Missing compile-time assertion that a type satisfies an interface.

### Fix
```go
// in the consuming package
type UserStore interface {
    Get(ctx context.Context, id string) (*User, error)
}

// in the implementing package
var _ consumer.UserStore = (*PostgresStore)(nil)
```

---

## 11. Testing

### Flag
- Tests that sleep to wait for goroutines — flaky. Use `sync.WaitGroup` or channels.
- Shared mutable state between `t.Parallel()` tests.
- `t.Fatal` inside a goroutine — doesn't fail the test; use `t.Errorf` and signal via channel.
- Missing table-driven structure for tests with >3 similar cases.
- Assertions on implementation detail (private fields via reflection) instead of observable behavior.

---

## Quick Triage Heuristics

| Symptom | Likely cause |
|---|---|
| "Our goroutine count climbs over hours" | Missing ctx/cancel on long-lived goroutine |
| "P99 latency cliff under load" | Missing outbound timeout or unbounded concurrency |
| "Intermittent 500s after deploy" | Missing graceful shutdown; in-flight requests dropped |
| "Works in staging, fails in prod" | `http.DefaultClient` or missing conn pool tuning |
| "Connection pool exhausted" | Leaked `rows`/`tx` from missing defer |
