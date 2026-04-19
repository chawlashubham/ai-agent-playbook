---
id: git-diff-analyze
version: "1.0"
status: experimental
description: >
  Analyzes a unified git diff and returns structured metadata: intent, surface
  area, risk signals, and suggested reviewers. Designed as a shared dependency
  for code-review, weekly-report, and release-notes workflows so they don't
  each re-parse diffs.
author: ai-agent-playbook
---

# git-diff-analyze

> Turn a unified diff into structured, review-ready metadata.

---

## Overview

Given a unified diff (as produced by `git diff`, `gh pr diff`, or a patch
file), this skill extracts:

- **Intent**: one-line inferred purpose (feature / fix / refactor / infra / docs / test).
- **Surface area**: files and packages touched, grouped by layer.
- **Risk signals**: changes that warrant extra scrutiny (migrations, auth,
  concurrency primitives, error handling, public API, deleted tests).
- **Stats**: added/removed lines, files changed, largest hunks.

Use as a pre-step before a human or LLM review. It does **not** render
opinions on quality — that's the job of the `code-review` skill.

---

## Input Schema

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `diff` | string | yes | — | Unified diff text. Accepts multi-file diffs. |
| `repo_language_hint` | string | no | `auto` | `go`, `python`, `typescript`, `auto`. Tunes risk detection. |
| `max_files` | integer | no | `200` | Safety cap; diffs above this truncate with a warning. |

---

## Output Schema

| Field | Type | Description |
|---|---|---|
| `intent` | string | One-line inferred purpose |
| `category` | string | One of: `feature`, `fix`, `refactor`, `infra`, `docs`, `test`, `mixed` |
| `files_changed` | integer | Total files in the diff |
| `lines_added` | integer | Total additions |
| `lines_removed` | integer | Total removals |
| `surface_area` | array<object> | `{path, layer, added, removed}` per file, `layer ∈ handler\|service\|repo\|infra\|test\|migration\|config\|docs` |
| `risk_signals` | array<object> | `{kind, path, line, detail}` — see Risk Signals below |
| `suggested_reviewers` | array<string> | Inferred from CODEOWNERS patterns if supplied, else file-path heuristics |
| `truncated` | boolean | True if `max_files` was exceeded |

### Risk Signal kinds

- `migration` — SQL or migration tool files touched
- `auth` — authN/authZ code paths
- `concurrency` — goroutines, channels, locks, atomics (language-aware)
- `error_handling` — added `err != nil` swallowing, removed error checks
- `public_api` — change to exported types/handlers/OpenAPI spec
- `deleted_tests` — test files or test functions removed
- `secrets` — strings matching secret patterns introduced
- `config_drift` — production configs changed without code counterpart

---

## Usage

### In an agent config

```yaml
skills:
  - ref: skills/git-diff-analyze/SKILL.md
```

### Calling the skill

```json
{
  "diff": "diff --git a/cmd/server/main.go ...",
  "repo_language_hint": "go"
}
```

### Example response

```json
{
  "intent": "Add idempotency-key enforcement to POST /charge",
  "category": "feature",
  "files_changed": 4,
  "lines_added": 128,
  "lines_removed": 12,
  "surface_area": [
    {"path": "internal/charge/handler.go", "layer": "handler", "added": 42, "removed": 4},
    {"path": "internal/charge/service.go", "layer": "service", "added": 61, "removed": 6},
    {"path": "internal/charge/service_test.go", "layer": "test", "added": 20, "removed": 2},
    {"path": "migrations/0042_idempotency_keys.sql", "layer": "migration", "added": 5, "removed": 0}
  ],
  "risk_signals": [
    {"kind": "migration", "path": "migrations/0042_idempotency_keys.sql", "line": 1, "detail": "New table idempotency_keys"},
    {"kind": "public_api", "path": "internal/charge/handler.go", "line": 34, "detail": "New required header Idempotency-Key on POST /charge"}
  ],
  "suggested_reviewers": ["@payments-team"],
  "truncated": false
}
```

---

## Implementation

See [`config.yaml`](config.yaml) for runtime details.

- **Type**: `python_function` — deterministic parsing + regex-based signal detection, then one LLM pass for `intent` inference
- **Auth**: none for parsing; uses `ANTHROPIC_API_KEY` for the intent pass
- **On error**: malformed diff → returns `category: "unknown"` with a parse-error signal rather than failing

---

## Error Handling

| Error | Behaviour |
|---|---|
| Empty diff | Returns zeros with `intent: "no changes"` |
| Malformed diff | Parses what it can, sets `truncated: true` and emits a parse-error risk signal |
| `max_files` exceeded | Truncates, sets `truncated: true`, processes the largest-hunk files first |
| LLM call fails | Falls back to a rule-based intent (first file's layer + category) |

---

## Constraints

- Not a semantic diff — `foo.Bar()` → `foo.Baz()` is a text change, not a
  behavior analysis. Use `code-review` for that.
- Risk detection is regex/heuristic: false positives possible on
  `auth_test.go`, false negatives possible on creative naming.
- Diffs larger than ~5k lines should be split or scoped via `max_files`.

---

## Notes

- Intended to feed *into* other skills and prompts, not to be shown to
  users directly. Its output is structured, not narrative.
- Pairs with `code-review` (quality critique) and
  `management-weekly-report-v1` (multi-PR summarization).
- When used in a workflow, cache results per commit SHA — a diff's analysis
  doesn't change once the commit is frozen.
