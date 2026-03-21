# Hypothesis: Chain-of-Thought Improves Summarization Quality

## Background

Direct summarization prompts often produce summaries that miss key details or introduce subtle inaccuracies. Chain-of-thought (CoT) prompting forces the model to reason through a document's structure before writing the summary.

## Hypothesis

> Adding an explicit reasoning step (identify document type → extract key points → identify audience) before writing the summary will reduce hallucinations and improve coverage of key facts compared to a direct summarization prompt.

## Metrics

| Metric | Measurement Method |
|---|---|
| ROUGE-L score | Automated comparison against human-written reference summaries |
| Human preference | Blind A/B evaluation by 3 raters on 30 documents |
| Hallucination rate | Manual review of 30 summaries for unsupported claims |

## Test Set

- 30 documents across 3 domains: technical reports, news articles, customer emails
- 10 documents per domain

## Control

`prompts/experimental/summarization/direct-summary-v1.md` — a standard zero-shot summarization prompt with no reasoning step.

## Treatment

`prompts/experimental/summarization/chain-of-thought-summary-v1.md` — the CoT prompt with explicit reasoning steps.

## Success Criteria

- ROUGE-L improvement ≥ 5%
- Human preference ≥ 60% for CoT
- Hallucination rate reduction ≥ 20%
