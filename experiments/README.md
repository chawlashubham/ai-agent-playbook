# experiments/

This directory holds **time-boxed experiments** — exploratory work that is not yet ready for production. Each experiment lives in its own sub-folder.

## Structure

```
experiments/
└── YYYY-MM-<short-description>/
    ├── README.md        # Experiment summary (goal, approach, outcome)
    ├── hypothesis.md    # What you are testing and why
    └── results.md       # Findings, metrics, conclusions
```

## When to Use

- Trying a new prompting technique (chain-of-thought, few-shot, tree-of-thought).
- Evaluating a new model or framework.
- Prototyping a multi-agent workflow before formalising it.

## Lifecycle

```
experiments/ → (review) → prompts/production/ or agents/ or workflows/
                         → (if unsuccessful) archived in experiments/ with status: failed
```

## Naming Convention

```
YYYY-MM-<short-kebab-description>
```

Examples:
- `2024-01-chain-of-thought-summarization`
- `2024-03-ollama-llama3-code-review`
- `2024-06-multi-agent-research-pipeline`

## Available Experiments

| Experiment | Status | Description |
|---|---|---|
| [`2024-01-chain-of-thought-summarization/`](2024-01-chain-of-thought-summarization/) | completed | Evaluated CoT prompting for document summarization |
