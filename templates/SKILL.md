---
id: <skill-name>
version: "1.0"
status: experimental           # experimental | production | deprecated
description: <One-line description of what this skill does.>
tags:
  - <tag>
config: config.yaml
used_by:
  - agents/<agent-name>
author: <team-or-username>
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# <skill-name>

> One-sentence summary of what this skill does and what it returns.

---

## Overview

2–3 sentences describing the skill's purpose, when to use it,
and what system or API it wraps (if applicable).

---

## Input Schema

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `param_1` | string | yes | — | Description |
| `param_2` | integer | no | `5` | Description (max: N) |

---

## Output Schema

| Field | Type | Description |
|---|---|---|
| `field_1` | string | Description |
| `field_2` | array | Description |

---

## Usage

### In an agent config

```yaml
skills:
  - ref: skills/<skill-name>/SKILL.md
```

### Calling the skill

```json
{
  "param_1": "example value",
  "param_2": 5
}
```

### Example response

```json
{
  "field_1": "example output",
  "field_2": []
}
```

---

## Implementation

See [`config.yaml`](config.yaml) for endpoint, auth, and error handling details.

- **Type**: `api` | `python_function` | `shell`
- **Auth**: Description of auth method and environment variable
- **On error**: Description of error behaviour

---

## Error Handling

| Error | Behaviour |
|---|---|
| Auth missing | Raises immediately |
| Rate limit | Retries N times |
| No results | Returns empty |

---

## Constraints

- Constraint 1 (e.g., max N items per call)
- Constraint 2

---

## Notes

- Usage tip 1
- Usage tip 2
