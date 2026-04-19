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
| [`reporting/management-weekly-report-v1.md`](reporting/management-weekly-report-v1.md) | Manager-facing weekly status from GitHub PR/issue data |
| [`planning/rfc-critique-v1.md`](planning/rfc-critique-v1.md) | Principal-engineer critique of a design doc / RFC |
| [`planning/scope-splitter-v1.md`](planning/scope-splitter-v1.md) | Split an epic into vertically-sliced, shippable tickets |
| [`management/1on1-prep-v1.md`](management/1on1-prep-v1.md) | Private 1:1 prep for a direct report from GitHub activity + prior notes |
