# Example: Research Report on Few-Shot Prompting

**Input topic:** `impact of few-shot prompting on classification accuracy`

---

# Impact of Few-Shot Prompting on Classification Accuracy

## Summary

Few-shot prompting consistently improves classification accuracy over zero-shot baselines across a range of NLP tasks, with gains of 10–30% reported in benchmark studies. The improvement is most pronounced for complex or ambiguous categories where label semantics are non-obvious. However, example selection quality is a significant variable — poorly chosen examples can degrade performance below zero-shot.

## Key Findings

1. Few-shot prompting outperforms zero-shot on most classification tasks (Brown et al., 2020).
2. Example diversity matters more than quantity — 3–5 well-chosen examples often match performance of 10+.
3. Label-balanced example sets reduce model bias toward majority classes.
4. Chain-of-thought few-shot prompting further improves accuracy on multi-label tasks.
5. Performance gains are smaller on models with strong instruction-following (e.g., GPT-4o, Claude Sonnet).

## Detailed Analysis

### Effect on Accuracy
Studies consistently show few-shot prompting improves classification accuracy by 10–30% over zero-shot on standard benchmarks such as SST-2, AGNews, and TREC. The effect is larger for smaller models and tasks with subtle label distinctions.

### Example Selection
The quality of examples is the dominant variable. Random selection from a labelled pool performs significantly worse than similarity-based selection (e.g., using embeddings to find examples close to the test input).

### Conflicting Views
Some recent work argues that for large instruction-tuned models (GPT-4 class), the marginal benefit of few-shot prompting over zero-shot is diminishing — with zero-shot + detailed instructions matching few-shot in many benchmarks.

## Sources

1. [Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165) — Original GPT-3 paper introducing few-shot prompting
2. [What Makes Good In-Context Examples for GPT-3?](https://arxiv.org/abs/2101.06804) — Analysis of example selection strategies
3. [Chain-of-Thought Prompting Elicits Reasoning](https://arxiv.org/abs/2201.11903) — CoT combined with few-shot for complex tasks
