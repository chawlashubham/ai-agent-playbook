# Hypothesis: Few-Shot Examples Improve Intent Classification Accuracy

## Background

Zero-shot intent classification relies entirely on the model's pre-trained
understanding of intent labels. When label names are ambiguous or domain-specific
(e.g., `cancellation` vs `refund_request`), the model may misclassify edge cases.
Providing labelled examples (few-shot) gives the model concrete anchors for each class.

## Hypothesis

> Adding 2–3 representative labelled examples per intent class to the classification
> prompt will improve accuracy by ≥10 percentage points on a balanced test set,
> compared to a zero-shot prompt using the same underlying model.

## Metrics

| Metric | Measurement Method |
|---|---|
| Accuracy | Exact-match against human-labelled ground truth |
| Avg. latency | Time-to-first-token (ms), averaged across 60 test messages |
| Cost delta | Estimated token cost increase per classification call |

## Test Set

- 60 messages balanced across 6 intent classes (10 per class):
  - `refund_request`, `order_status`, `general_inquiry`,
    `escalation`, `product_question`, `cancellation`
- Messages sourced from synthetic customer support conversations.

## Control

`prompts/experimental/classification/classify-intent-v1.md` — zero-shot prompt
with explicit reasoning step, no labelled examples.

## Treatment

`prompts/experimental/classification/classify-intent-few-shot-v1.md` — same
reasoning structure, with 2 labelled examples per intent class added to the
prompt context.

## Success Criteria

- Accuracy improvement ≥ 10 percentage points
- Latency increase < 500ms (acceptable overhead for routing use case)
- Cost increase < 3× per call
