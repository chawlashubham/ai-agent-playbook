---
id: web-search
version: "1.0"
status: production
description: Queries a search engine and returns ranked results with titles, URLs, and snippets.
tags:
  - search
  - web
  - information-retrieval
config: config.yaml
used_by:
  - agents/research-agent
  - agents/coding-assistant-agent
  - agents/customer-support-agent
author: ai-team
created: 2024-01-15
updated: 2024-01-15
---

# web-search

> Searches the web for up-to-date information on a given query and returns a ranked list of results with titles, URLs, and snippets.

---

## Overview

This skill wraps a REST search API to provide agents with real-time web access. It is the primary information-gathering tool for research, fact-checking, and answering questions that require current data beyond the model's training cutoff.

Pair with [`file-reader`](../file-reader/SKILL.md) to extract full content from the URLs returned.

---

## Input Schema

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `query` | string | yes | — | The search query string |
| `num_results` | integer | no | `5` | Number of results to return (max 20) |

---

## Output Schema

| Field | Type | Description |
|---|---|---|
| `results` | array | Ranked list of search results |
| `results[].title` | string | Page title |
| `results[].url` | string | Source URL |
| `results[].snippet` | string | Short excerpt from the page |

---

## Usage

### In an agent config

```yaml
skills:
  - ref: skills/web-search/SKILL.md
```

### Calling the skill

```json
{
  "query": "few-shot prompting classification accuracy 2024",
  "num_results": 10
}
```

### Example response

```json
{
  "results": [
    {
      "title": "Few-Shot Prompting Guide — Prompt Engineering",
      "url": "https://example.com/few-shot-guide",
      "snippet": "Few-shot prompting significantly improves classification accuracy by providing the model with representative examples..."
    }
  ]
}
```

---

## Implementation

See [`config.yaml`](config.yaml) for endpoint, auth, and error handling details.

- **Type**: REST API
- **Auth**: API key via `SEARCH_API_KEY` environment variable
- **On error**: Returns empty results after 2 retries

---

## Error Handling

| Error | Behaviour |
|---|---|
| API key missing | Raises immediately with clear message |
| Rate limit (429) | Retries twice with exponential backoff |
| No results found | Returns `{ "results": [] }` |
| Network timeout | Returns empty after 2 retries |

---

## Constraints

- Maximum 20 results per call
- Query must be a non-empty string
- Does not support image or video search

---

## Notes

- Pair with `file-reader` to extract full-text content from result URLs
- Results are not cached — repeated identical queries consume API quota
