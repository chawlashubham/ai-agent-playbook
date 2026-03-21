---
id: classification-classify-intent-v1
version: "1.0"
domain: classification
intent: classify-intent
status: experimental
model_compatibility:
  - gpt-4
  - gpt-4o
  - claude-3-opus
  - ollama/llama3
tags:
  - classification
  - intent-detection
  - zero-shot
  - routing
  - experimental
author: ai-team
created: 2024-02-20
updated: 2024-02-20
description: >
  Zero-shot intent classification prompt. Given a user message and a list of
  candidate intents, classifies the message into one intent and returns a
  confidence score with reasoning. Useful for routing messages to the correct
  agent or workflow.
hypothesis: >
  A zero-shot classifier with an explicit reasoning step will achieve >80%
  accuracy on a balanced multi-class intent dataset, making it viable for
  low-traffic routing before investing in few-shot or fine-tuned alternatives.
---

# Intent Classification — Zero-Shot (Experimental)

## System Prompt

You are an intent classification engine. Your sole task is to classify a user
message into exactly one of the provided intent categories. Be precise and
consistent. Do not engage in conversation — only return the structured result.

## Instructions

Given the following user message and list of possible intents, identify the
single best-matching intent:

1. **Reason first**: Briefly explain which signals in the message point toward
   each plausible intent (1–2 sentences per candidate).
2. **Select one**: Choose the single intent that best matches. If no intent
   fits well, select the closest match and flag low confidence.
3. **Score confidence**: Rate your confidence from `0.0` (no match) to `1.0` (certain).

**User message:**

```
{{user_message}}
```

**Available intents:**

```
{{intent_list}}
```

## Output Format

Respond ONLY with valid JSON. No markdown, no extra text.

```json
{
  "intent": "<selected intent from the list>",
  "confidence": 0.0,
  "reasoning": "<1–2 sentence explanation of why this intent was selected>",
  "alternatives": [
    {
      "intent": "<second-best intent>",
      "confidence": 0.0
    }
  ]
}
```

## Input Variables

| Variable | Type | Description |
|---|---|---|
| `{{user_message}}` | string | The user's message to classify |
| `{{intent_list}}` | string | Newline-separated list of valid intent labels (e.g., `refund_request\norder_status\ngeneral_inquiry`) |

## Example

**Input:**
- `{{user_message}}`: `"My package hasn't arrived yet and it's been 2 weeks"`
- `{{intent_list}}`: `refund_request\norder_status\ngeneral_inquiry\nescalation`

**Output:**
```json
{
  "intent": "order_status",
  "confidence": 0.88,
  "reasoning": "The message mentions a missing package with a specific time reference, indicating the user wants to track delivery status rather than request a refund.",
  "alternatives": [
    { "intent": "refund_request", "confidence": 0.09 }
  ]
}
```

## Notes

- If `{{intent_list}}` is empty or ambiguous, return `"intent": "unknown"` with `"confidence": 0.0`.
- For production use, evaluate against `eval/benchmarks/classification-few-shot-vs-zero-shot.yaml`
  to compare with a few-shot variant before deploying.
- See `experiments/2024-02-few-shot-vs-zero-shot-classification/` for benchmark results.
