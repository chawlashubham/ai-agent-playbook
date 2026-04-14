---
id: <agent-name>
version: "1.0"
status: experimental           # experimental | staging | production
description: <One-line description of what this agent does.>
tags:
  - <domain-tag>
  - <technique-tag>
config: config.yaml
system_prompt: system-prompt.md
skills:
  - skills/<skill-name>
tools_compatible:
  - claude                     # claude | copilot | chatgpt | cursor
author: <team-or-username>
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# <Agent Name>

> One-sentence summary of the agent's role and primary value.

---

## Purpose

Describe the problem this agent solves and when to use it. 2–4 sentences.
Be specific about what it does and what it does NOT do.

---

## Capabilities

- **Capability 1**: Brief description
- **Capability 2**: Brief description
- **Capability 3**: Brief description

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `input_1` | string | yes | Description |
| `input_2` | string | no | Description (default: `value`) |

---

## Outputs

| Output | Type | Description |
|---|---|---|
| `output_1` | string | Description |
| `output_2` | array | Description |

---

## Tools / Skills Used

| Skill | Purpose |
|---|---|
| [`skill-name`](../../skills/skill-name/SKILL.md) | What it does for this agent |

---

## Prompt References

| File | Purpose |
|---|---|
| [`system-prompt.md`](system-prompt.md) | Agent identity, process, output format |

---

## Configuration

See [`config.yaml`](config.yaml) for model settings, memory, and guardrails.

Key settings:
- **Model**: `<model-name>` (temperature: `<value>`)
- **Memory**: `<type>`, max `<N>` messages
- **Max turns**: `<N>`

---

## Examples

### Example 1: <short description>

**Input:**
```
input_1: example value
```

**Output summary:**
> What the agent produces for this input.

See [`examples/`](examples/) for full sample outputs.

---

## Limitations

- Limitation 1
- Limitation 2

---

## Related

- **Workflows**: [`workflows/<name>/README.md`](../../workflows/<name>/README.md)
- **Prompts**: [`prompts/production/<domain>/`](../../prompts/production/<domain>/)
- **Eval**: [`eval/test-cases/`](../../eval/test-cases/)
