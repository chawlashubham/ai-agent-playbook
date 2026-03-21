# eval/

This directory contains resources for **evaluating prompt and agent quality**: datasets, test cases, and benchmarks.

## Sub-directories

| Folder | Purpose |
|---|---|
| [`datasets/`](datasets/) | Curated input/output pairs for offline evaluation |
| [`test-cases/`](test-cases/) | Structured pass/fail tests for individual prompts |
| [`benchmarks/`](benchmarks/) | Comparative benchmarks across model versions or prompt variants |

## When to Use

- Before promoting a prompt from `experimental/` to `production/`, run its test cases.
- Use datasets to regression-test prompts after changes.
- Use benchmarks to compare prompt variants or model upgrades.

## Evaluation Workflow

```
1. Write test cases in eval/test-cases/<prompt-id>-tests.yaml
2. Run against the prompt using your evaluation harness
3. Record results in eval/benchmarks/ if comparing variants
4. Only promote to production/ when all test cases pass
```
