# eval/benchmarks/

Comparative benchmarks for evaluating prompt variants, model upgrades, or technique changes.

## File Naming Convention

```
<domain>-<comparison-description>.yaml
```

Example: `summarization-cot-vs-direct.yaml`

## Benchmark Format

```yaml
benchmark_id: <id>
description: <what is being compared>
metric: rouge-l              # rouge-l | human-preference | latency | cost
variants:
  - id: control
    prompt_ref: prompts/experimental/...
  - id: treatment
    prompt_ref: prompts/experimental/...
dataset: eval/datasets/<dataset-file>.json
results:
  - variant_id: control
    score: 0.41
  - variant_id: treatment
    score: 0.46
winner: treatment
notes: >
  Notes on statistical significance or caveats.
```

## Available Benchmarks

| File | Description |
|---|---|
| [`summarization-cot-vs-direct.yaml`](summarization-cot-vs-direct.yaml) | CoT vs direct summarization on ROUGE-L |
