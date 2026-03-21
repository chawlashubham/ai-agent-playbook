# Results: Few-Shot vs Zero-Shot Intent Classification

**Date completed:** 2024-02-28
**Evaluated by:** AI Team
**Model tested:** gpt-4o

## Quantitative Results

| Metric | Zero-Shot | Few-Shot | Change |
|---|---|---|---|
| Accuracy | 78% | 91% | **+13pp** ✅ |
| Avg. latency (ms) | 820 | 950 | **+130ms** ✅ (within 500ms threshold) |
| Estimated cost / call | $0.0018 | $0.0031 | **+72%** ✅ (within 3× threshold) |

All three success criteria were met.

## Per-Class Accuracy Breakdown

| Intent Class | Zero-Shot | Few-Shot | Δ |
|---|---|---|---|
| `refund_request` | 90% | 100% | +10pp |
| `order_status` | 80% | 90% | +10pp |
| `general_inquiry` | 70% | 90% | +20pp |
| `escalation` | 80% | 90% | +10pp |
| `product_question` | 70% | 80% | +10pp |
| `cancellation` | 60% | 95% | **+35pp** |

## Qualitative Observations

- The largest accuracy gain was on `cancellation` — the zero-shot model frequently
  confused it with `refund_request`. Adding labelled examples eliminated most of
  these confusions.
- `general_inquiry` was consistently the hardest class in zero-shot mode due to
  ambiguous phrasing; few-shot examples provided a clear boundary.
- Few-shot prompts were approximately 40% longer in token count, contributing to
  the latency and cost increases.

## Conclusion

Few-shot prompting significantly outperforms zero-shot for this intent set,
particularly for semantically similar classes. **Recommend promoting the few-shot
variant to experimental and beginning production review.**

The zero-shot prompt remains useful for low-cost, low-stakes routing where a
confidence threshold can be used to catch ambiguous cases.

## Next Steps

1. ✅ Promote `classify-intent-few-shot-v1.md` to `prompts/experimental/classification/`.
2. [ ] Add test cases to `eval/test-cases/`.
3. [ ] Evaluate zero-shot + confidence threshold as a cost-saving hybrid approach.
4. [ ] Test on a harder, 12-class intent set to assess scalability of few-shot approach.
