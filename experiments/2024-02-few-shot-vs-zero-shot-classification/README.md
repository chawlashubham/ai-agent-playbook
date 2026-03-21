# Experiment: Few-Shot vs Zero-Shot Intent Classification

**Period:** February 2024
**Status:** Completed
**Outcome:** ✅ Promoted — Few-shot variant achieved 91% accuracy vs 78% for zero-shot; see `results.md`.

## Goal

Evaluate whether providing 2–3 labelled examples per intent class (few-shot)
meaningfully improves classification accuracy compared to a pure zero-shot
approach, and whether the accuracy gain justifies the additional prompt length
and latency cost.

## Artifacts

| File | Description |
|---|---|
| [`hypothesis.md`](hypothesis.md) | The testable hypothesis and experimental design |
| [`results.md`](results.md) | Findings, metrics, and conclusions |

## Outcome

Few-shot classification improved accuracy by 13 percentage points (78% → 91%)
with only a ~130ms latency increase. The few-shot prompt was promoted to
`prompts/experimental/classification/classify-intent-few-shot-v1.md` and is
scheduled for production review.
