---
id: file-reader
version: "1.0"
status: production
description: Reads and extracts text content from a local file path or remote URL (PDF, TXT, Markdown, HTML).
tags:
  - file
  - document
  - parsing
  - pdf
  - text-extraction
config: config.yaml
used_by:
  - agents/research-agent
author: ai-team
created: 2024-01-15
updated: 2024-01-15
---

# file-reader

> Reads and extracts text content from a local file path or a remote URL. Supports PDF, plain text, Markdown, and HTML.

---

## Overview

This skill provides agents with the ability to read documents — both from the local filesystem and from remote URLs. It is typically used after `web-search` to extract full-text content from result URLs, or to process user-supplied documents.

Content is truncated to `max_chars` to prevent context overflow.

---

## Input Schema

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `source` | string | yes | — | Local file path (e.g., `/data/report.pdf`) or remote URL |
| `max_chars` | integer | no | `10000` | Maximum characters to return |

---

## Output Schema

| Field | Type | Description |
|---|---|---|
| `content` | string | Extracted text content |
| `source_type` | string | Detected type: `pdf`, `txt`, `markdown`, `html`, `unknown` |
| `truncated` | boolean | `true` if content was cut at `max_chars` |

---

## Usage

### In an agent config

```yaml
skills:
  - ref: skills/file-reader/SKILL.md
```

### Calling the skill

```json
{
  "source": "https://example.com/paper.pdf",
  "max_chars": 5000
}
```

### Example response

```json
{
  "content": "Abstract: This paper investigates...",
  "source_type": "pdf",
  "truncated": true
}
```

---

## Implementation

See [`config.yaml`](config.yaml) for implementation details.

- **Type**: Python function
- **Module**: `skills.file_reader`
- **On error**: Raises immediately

---

## Error Handling

| Error | Behaviour |
|---|---|
| File not found | Raises with path in message |
| Unsupported format | Returns `source_type: unknown`, best-effort extraction |
| Network error on URL | Raises after single attempt |
| Permission denied | Raises immediately |

---

## Constraints

- Remote URLs must be publicly accessible (no auth support)
- PDF extraction quality depends on whether the PDF is text-based (not scanned images)
- `max_chars` applies to extracted text, not raw file size

---

## Notes

- Always set `max_chars` when using inside a workflow to avoid exhausting model context
- For scanned PDFs, consider an OCR skill instead
