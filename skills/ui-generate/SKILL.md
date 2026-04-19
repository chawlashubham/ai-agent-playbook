---
id: ui-generate
version: "1.0"
status: experimental
description: Generates UI component code (HTML/CSS/JSX) from a natural language description or design spec.
tags:
  - ui
  - frontend
  - code-generation
  - design
config: config.yaml
used_by: []
author: shubhamchawla13
created: 2026-04-19
updated: 2026-04-19
---

# ui-generate

> Generates UI component code in HTML/CSS, Tailwind, or JSX/TSX from a plain-language description or design spec, and returns the source code with a usage example.

---

## Overview

This skill takes a natural language description of a UI component and returns production-ready markup or component code. It targets three output formats — plain HTML/CSS, Tailwind-styled HTML, and React JSX/TSX — selectable per call. Use it to scaffold buttons, forms, cards, modals, navbars, and other common UI patterns without leaving the agent workflow.

---

## Input Schema

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `description` | string | yes | — | Plain-language description of the UI component to generate |
| `framework` | string | no | `"html"` | Target output format: `html` \| `tailwind` \| `jsx` \| `tsx` |
| `theme` | string | no | `"light"` | Color theme hint: `light` \| `dark` \| `system` |
| `responsive` | boolean | no | `true` | Whether to include responsive breakpoint styles |

---

## Output Schema

| Field | Type | Description |
|---|---|---|
| `component_name` | string | Inferred name for the generated component |
| `framework` | string | Output format that was used |
| `code` | string | The generated source code |
| `usage_example` | string | Minimal snippet showing how to use the component |
| `dependencies` | array | Any external packages required (e.g. `react`, `tailwindcss`) |

---

## Usage

### In an agent config

```yaml
skills:
  - ref: skills/ui-generate/SKILL.md
```

### Calling the skill

```json
{
  "description": "A primary call-to-action button with a loading spinner state",
  "framework": "tsx",
  "theme": "light",
  "responsive": false
}
```

### Example response

```json
{
  "component_name": "PrimaryButton",
  "framework": "tsx",
  "code": "import { useState } from 'react';\n\ninterface PrimaryButtonProps {\n  label: string;\n  onClick: () => Promise<void>;\n}\n\nexport function PrimaryButton({ label, onClick }: PrimaryButtonProps) {\n  const [loading, setLoading] = useState(false);\n  return (\n    <button\n      disabled={loading}\n      onClick={async () => { setLoading(true); await onClick(); setLoading(false); }}\n      className=\"px-4 py-2 bg-blue-600 text-white rounded disabled:opacity-50\"\n    >\n      {loading ? <span className=\"animate-spin\">⏳</span> : label}\n    </button>\n  );\n}",
  "usage_example": "<PrimaryButton label=\"Submit\" onClick={handleSubmit} />",
  "dependencies": ["react"]
}
```

---

## Implementation

See [`config.yaml`](config.yaml) for endpoint, auth, and error handling details.

- **Type**: `python_function`
- **Auth**: `ANTHROPIC_API_KEY` — calls Claude to generate component code
- **On error**: Raises immediately with a descriptive message

---

## Error Handling

| Error | Behaviour |
|---|---|
| `ANTHROPIC_API_KEY` missing | Raises immediately with clear message |
| Invalid `framework` value | Raises with list of accepted values |
| Generation timeout (>30 s) | Raises with timeout message |
| Empty `description` | Raises with validation message |

---

## Constraints

- `description` must be a non-empty string (max 1 000 characters)
- `framework` must be one of: `html`, `tailwind`, `jsx`, `tsx`
- Does not generate multi-file projects — single component output only
- Generated code is not executed or validated; review before use in production

---

## Notes

- For Tailwind output, ensure `tailwindcss` is installed and configured in the consuming project
- Pair with `code-execution` skill to run generated JS snippets in a sandbox for smoke-testing
- Start components in `experimental` status and promote after design review
