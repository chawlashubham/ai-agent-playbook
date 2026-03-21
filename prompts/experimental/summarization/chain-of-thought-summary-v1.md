---
id: summarization-chain-of-thought-v1
version: "1.0"
domain: summarization
intent: abstractive-summary-with-reasoning
status: experimental
model_compatibility:
  - gpt-4
  - gpt-4o
  - claude-3-opus
tags:
  - summarization
  - chain-of-thought
  - reasoning
  - experimental
author: ai-team
created: 2024-01-10
updated: 2024-01-10
description: >
  Experimental chain-of-thought prompt for document summarization.
  Forces the model to reason through the document structure before
  producing the final summary, aiming for higher factual accuracy.
hypothesis: >
  Adding an explicit reasoning step before the summary reduces hallucinations
  and improves coverage of key points compared to direct summarization.
---

# Chain-of-Thought Document Summarization (Experimental)

## System Prompt

You are an expert summarizer. Before writing a summary, you will reason step-by-step through the document to identify its key points, structure, and important details.

## Instructions

Given the following document, follow these steps:

**Step 1 — Understand the structure:**
Briefly identify the type of document (article, report, email, etc.) and its main sections.

**Step 2 — Extract key points:**
List the 3–7 most important facts, arguments, or conclusions from the document.

**Step 3 — Identify the audience:**
Who is the intended audience? What level of detail do they need?

**Step 4 — Write the summary:**
Using the key points from Step 2, write a concise summary of {{target_length}} words, appropriate for the audience identified in Step 3.

**Document:**
{{document}}

## Output Format

```
### Reasoning

**Document type:** <type>
**Key points:**
1. ...
2. ...
3. ...
**Intended audience:** <audience>

### Summary

<your summary here>
```

## Input Variables

| Variable | Type | Description |
|---|---|---|
| `{{document}}` | string | The full text of the document to summarize |
| `{{target_length}}` | integer | Desired word count for the summary (e.g., 150) |

## Experiment Notes

- Compare output quality against `summarization-direct-v1.md` using `eval/benchmarks/summarization-cot-vs-direct.yaml`.
- Metric: ROUGE-L score and human preference ratings.
- Status: In evaluation — do not use in production until benchmarks complete.
