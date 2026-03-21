# prompts/experimental/

Work-in-progress and exploratory prompts. These are **not** approved for production use.

## Rules

- Experimental prompts may have incomplete metadata — at minimum include `id`, `status: experimental`, and `description`.
- When a prompt is ready for production, copy it to `prompts/production/<domain>/`, increment the version, update the status, and log the change.
- Stale experimental prompts (> 90 days without update) should be archived or deleted.

## Domain Sub-folders

Mirror the same domain structure as `production/`:

```
experimental/
├── summarization/
├── coding/
└── ...
```

## Example Files

| File | Description |
|---|---|
| [`summarization/chain-of-thought-summary-v1.md`](summarization/chain-of-thought-summary-v1.md) | CoT summarization experiment |
