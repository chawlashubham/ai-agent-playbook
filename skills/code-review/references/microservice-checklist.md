# Microservice Review Checklist

Load this when reviewing code that crosses service boundaries: HTTP/gRPC handlers, event producers/consumers, clients for other services, or API/schema changes.

---

## 1. API Contract

- [ ] **Backward compatible?** New required fields on an existing endpoint break old clients. Prefer optional + default.
- [ ] **Versioning strategy explicit?** URL (`/v2/...`), header, or content-type — one of them, applied consistently.
- [ ] **Error shape stable?** Same error envelope (code, message, details) across endpoints. No leaking internals (stack traces, SQL, hostnames).
- [ ] **Pagination on list endpoints?** Cursor > offset for large or mutable sets. Max page size enforced.
- [ ] **Idempotency keys** accepted on all mutation endpoints clients may retry.
- [ ] **Timeouts documented** in the API contract, not just in code.
- [ ] **Deprecation path** for fields being removed: `Deprecated` header, sunset date, migration note.

---

## 2. Service Boundaries

- [ ] Does this logic belong in *this* service? (Common smell: a service reaching into another's domain model.)
- [ ] Two services writing to the same table? → shared-DB anti-pattern; one owner per table.
- [ ] Is the call synchronous because it needs to be, or just because that was easier? Events often scale better.
- [ ] Circular dependency between services? → usually a missing third service or a boundary drawn in the wrong place.

---

## 3. Resilience

- [ ] **Timeout per outbound call** — and shorter than the caller's timeout (timeout hierarchy).
- [ ] **Retries only on idempotent operations**, with exponential backoff + jitter, and a max attempts cap.
- [ ] **Circuit breaker** on external dependencies to avoid cascading failure.
- [ ] **Bulkhead**: separate connection pools / goroutine pools per downstream, so a slow one doesn't starve the rest.
- [ ] **Graceful degradation**: what does this endpoint return when dependency X is down? 503 is fine; an uncaught panic is not.
- [ ] **Graceful shutdown**: SIGTERM → stop accepting new requests → drain in-flight → close pools → exit. No hard kill of in-flight work.

---

## 4. Idempotency & Consistency

- [ ] Mutation endpoints accept and honor an idempotency key (dedupe window documented).
- [ ] At-least-once event delivery → consumers are idempotent (dedupe by event ID, or use a processed-event table).
- [ ] Distributed transaction? → saga with explicit compensating actions, not 2PC.
- [ ] Read-after-write consistency expectations are explicit (caller knows whether to expect immediate or eventual).
- [ ] Eventual consistency windows quantified somewhere (docs, runbook, or metrics).

---

## 5. Event Schemas (Kafka / NATS / PubSub)

- [ ] Schema registered (Avro/Protobuf/JSON Schema) — not just "whatever the producer sends today".
- [ ] Backward- *and* forward-compatible changes only: add optional fields, never remove or repurpose.
- [ ] Event carries an ID and a producer-assigned timestamp for ordering/dedup.
- [ ] DLQ (dead letter queue) for poison messages, with an alert and a replay story.
- [ ] Consumer lag is monitored and alerted.
- [ ] Partition key chosen for even distribution *and* ordering guarantees the consumer needs.

---

## 6. Observability

- [ ] **Structured logs** (JSON) with correlation/request ID propagated from ingress → all downstream calls.
- [ ] **RED metrics** (Rate, Errors, Duration) on every service boundary; histograms, not just averages.
- [ ] **USE metrics** (Utilization, Saturation, Errors) on resources (DB pool, goroutines, queue depth).
- [ ] **Traces** span across services; context carries trace headers.
- [ ] **Error logs include** enough context to debug without reproducing: IDs, input summary, dependency state.
- [ ] **Health checks**: `/livez` (am I alive?) and `/readyz` (can I serve traffic?) are *different*.
- [ ] At least one alert points to a human-actionable runbook.

---

## 7. Security

- [ ] AuthN at the edge; AuthZ on every internal endpoint that handles user-scoped data (don't assume the edge is enough).
- [ ] Service-to-service auth: mTLS, signed JWTs, or similar — not just network boundary trust.
- [ ] Secrets loaded from a secret manager or env — never committed, never logged.
- [ ] PII fields tagged or redacted in logs.
- [ ] Rate limiting per caller / per API key on public endpoints.
- [ ] Input validation at the boundary (size, type, enum, range) — don't rely on the callee.

---

## 8. Data

- [ ] Migrations are backward compatible with the previously-deployed version (expand → migrate → contract).
- [ ] Long-running migrations chunked; no multi-hour table locks.
- [ ] Soft delete vs hard delete decision explicit; retention policy exists for PII.
- [ ] Indexes reviewed for new query patterns; explain plans checked at realistic data size.

---

## 9. Deployment & Rollback

- [ ] Feature flag guards risky behavior changes.
- [ ] Rollback path: can the previous version safely read data written by this version?
- [ ] Config changes ship separately from code changes where possible.
- [ ] Canary / staged rollout for high-blast-radius changes.

---

## 10. Smell-Test Questions

Ask these aloud during review:

1. "If the downstream service is 10x slower today, what happens here?"
2. "What happens if this message is delivered twice?"
3. "Two users hit this endpoint at the same millisecond — same row. Who wins? Does anyone lose data?"
4. "Deploy-in-progress: old and new versions running side by side for 5 minutes. Any corruption?"
5. "Dependency is down for 30 minutes. Do we back-pressure, queue, or drop? Does the caller know?"
6. "An engineer gets paged at 3am on this. What does the alert link to? Can they fix it?"
