# prompts/

This directory contains all prompt files organized by maturity and purpose.

## Sub-directories

| Folder | Purpose | When to Use |
|---|---|---|
| [`production/`](production/) | Battle-tested prompts approved for use in live systems | When you need a reliable, reviewed prompt |
| [`experimental/`](experimental/) | Work-in-progress or exploratory prompts | When iterating on new ideas or techniques |
| [`components/`](components/) | Reusable prompt fragments (personas, format blocks, etc.) | When composing prompts from smaller, tested parts |

## Naming Convention

```
<domain>-<intent>[-<variant>]-v<major>[.<minor>].md
```

Examples:
- `customer-support-handle-refund-v1.md`
- `summarization-chain-of-thought-v2.md`
- `code-review-security-focus-v1.1.md`

## Metadata Header

Every prompt file **must** begin with a YAML front-matter block:

```yaml
---
id: customer-support-handle-refund-v1
version: "1.0"
domain: customer-support
intent: handle-refund-request
status: production          # production | experimental | deprecated
model_compatibility: [gpt-4, claude-3, ollama/llama3]
tags: [customer-support, refund, empathy]
author: your-name
created: 2024-01-15
updated: 2024-01-20
description: >
  Guides the agent through a structured refund-handling conversation,
  ensuring empathy and policy compliance.
---
```

See [`docs/metadata-format.md`](../docs/metadata-format.md) for the full specification.

## Scaling to 1000+ Prompts

- Group files into **domain sub-folders** inside `production/` and `experimental/` (e.g., `customer-support/`, `coding/`, `summarization/`).
- Use metadata `tags` and `domain` fields for search/filtering.
- Run a simple `grep` or script against the front-matter to build a prompt index.
