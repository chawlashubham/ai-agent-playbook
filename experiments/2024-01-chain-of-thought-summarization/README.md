# Experiment: Chain-of-Thought Summarization

**Period:** January 2024
**Status:** Completed
**Outcome:** ✅ Promoted — CoT prompt produced measurably better summaries; see `results.md`.

## Goal

Evaluate whether adding an explicit chain-of-thought reasoning step before summarization improves output quality compared to direct summarization.

## Artifacts

| File | Description |
|---|---|
| [`hypothesis.md`](hypothesis.md) | The testable hypothesis and experimental design |
| [`results.md`](results.md) | Findings, metrics, and conclusions |

## Outcome

The CoT approach improved ROUGE-L scores by ~12% and human preference ratings by ~18%. The prompt was promoted to `prompts/experimental/summarization/chain-of-thought-summary-v1.md` and is scheduled for production review.
