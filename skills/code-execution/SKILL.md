---
id: code-execution
version: "1.0"
status: production
description: Executes sandboxed Python or JavaScript code snippets and returns stdout, stderr, and exit code.
tags:
  - code
  - execution
  - sandbox
  - python
  - javascript
config: config.yaml
used_by:
  - agents/coding-assistant-agent
author: ai-team
created: 2024-02-10
updated: 2024-02-10
---

# code-execution

> Executes a code snippet in a sandboxed environment and returns standard output, standard error, and exit code.

---

## Overview

This skill runs user-provided code in an isolated sandbox with no filesystem or network access. It is used by the coding assistant agent to validate, test, and demonstrate code snippets safely.

**Security note**: Always run in a sandboxed environment. Never grant filesystem or network access to untrusted code.

---

## Input Schema

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `code` | string | yes | — | The code snippet to execute |
| `language` | string | yes | — | Programming language: `python` or `javascript` |
| `timeout_seconds` | integer | no | `10` | Maximum execution time in seconds |

---

## Output Schema

| Field | Type | Description |
|---|---|---|
| `stdout` | string | Standard output from the execution |
| `stderr` | string | Standard error output (if any) |
| `exit_code` | integer | Process exit code (`0` = success) |

---

## Usage

### In an agent config

```yaml
skills:
  - ref: skills/code-execution/SKILL.md
```

### Calling the skill

```json
{
  "code": "print(sum(range(1, 101)))",
  "language": "python",
  "timeout_seconds": 5
}
```

### Example response

```json
{
  "stdout": "5050\n",
  "stderr": "",
  "exit_code": 0
}
```

---

## Implementation

See [`config.yaml`](config.yaml) for endpoint, auth, and error handling details.

- **Type**: REST API (sandboxed execution service)
- **Auth**: Bearer token via `SANDBOX_API_TOKEN` environment variable
- **On error**: Raises immediately

---

## Error Handling

| Error | Behaviour |
|---|---|
| Timeout exceeded | Returns `exit_code: 124`, partial stdout if any |
| Syntax error | Returns `exit_code: 1`, error in `stderr` |
| Auth token missing | Raises immediately |
| Unsupported language | Raises with supported language list |

---

## Constraints

- Supported languages: `python`, `javascript` only
- No filesystem access inside the sandbox
- No outbound network access inside the sandbox
- Maximum execution time: 30 seconds (hard cap, regardless of `timeout_seconds`)
- Code output is capped at 10,000 characters

---

## Notes

- Always confirm with the user before executing code that modifies state
- The coding assistant agent has a guardrail requiring `require_confirmation_before_execution: true`
- Treat any `stderr` output as a signal to review the code before retrying
