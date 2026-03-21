# Results: Chain-of-Thought Summarization Experiment

**Date completed:** 2024-01-28
**Evaluated by:** AI Team
**Model tested:** gpt-4o

## Quantitative Results

| Metric | Direct Summary | CoT Summary | Change |
|---|---|---|---|
| ROUGE-L (avg) | 0.41 | 0.46 | **+12.2%** ✅ |
| Human preference (CoT preferred) | — | 73% | **+23pp above 50%** ✅ |
| Hallucination rate | 18% | 11% | **-39% reduction** ✅ |

All three success criteria were met.

## Qualitative Observations

- CoT summaries were consistently better at covering the document's **conclusion and recommendations** section.
- The reasoning step helped the model correctly identify audience level, leading to more appropriately scoped summaries.
- Minor downside: CoT responses were ~40% longer in token count, increasing cost and latency.

## Domain Breakdown

| Domain | Direct ROUGE-L | CoT ROUGE-L | Winner |
|---|---|---|---|
| Technical reports | 0.38 | 0.47 | CoT |
| News articles | 0.45 | 0.48 | CoT |
| Customer emails | 0.40 | 0.43 | CoT |

## Conclusion

The CoT approach significantly outperforms direct summarization on all metrics. **Recommend promoting to experimental and beginning production review.**

## Next Steps

1. ✅ Promote `chain-of-thought-summary-v1.md` to `prompts/experimental/summarization/`.
2. [ ] Add test cases to `eval/test-cases/`.
3. [ ] Evaluate on gpt-3.5-turbo and claude-3-haiku for cost-sensitive use cases.
4. [ ] Investigate whether a lighter reasoning step (fewer instructions) preserves gains at lower cost.
