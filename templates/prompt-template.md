---
# ============================================================
# PROMPT METADATA — fill in all fields before saving
# ============================================================
id: <domain>-<intent>-v<version>          # e.g., customer-support-handle-refund-v1
version: "1.0"                            # Semantic: "1.0", "1.1", "2.0"
domain: <domain>                          # e.g., customer-support, summarization, coding
intent: <intent>                          # e.g., handle-refund-request, summarize-document
status: experimental                      # experimental | production | deprecated
model_compatibility:                      # List all tested models
  - gpt-4
  - gpt-4o
  # - claude-3-opus
  # - ollama/llama3
tags:
  - <tag-1>                               # e.g., refund, customer-support, empathy
  - <tag-2>
components:                               # Optional: reusable fragments used
  # - persona/helpful-assistant-v1
  # - format/json-output-v1
author: <your-name-or-team>
created: YYYY-MM-DD
updated: YYYY-MM-DD
description: >
  One to three sentences describing what this prompt does,
  when to use it, and any important constraints.
test_cases: eval/test-cases/<id>-tests.yaml   # Optional: link to test cases
---

# <Prompt Title>

## System Prompt

<!-- Write the system-level instructions here. Use {{variable_name}} for template variables. -->

You are a <role> for <context>. Your goal is to <primary_objective>.

## Instructions

<!-- Step-by-step instructions for the model. Be specific. -->

1. **Step 1**: ...
2. **Step 2**: ...
3. **Step 3**: ...

## Input Variables

| Variable | Type | Required | Description |
|---|---|---|---|
| `{{variable_1}}` | string | yes | Description of variable_1 |
| `{{variable_2}}` | string | no | Description of variable_2 |

## Example

<!-- Provide at least one concrete input/output example. -->

**Input:**
- `{{variable_1}}`: example value

**Output:**
> Expected model response here.

## Notes

<!-- Edge cases, known limitations, or tips for using this prompt. -->

- Note 1
- Note 2
