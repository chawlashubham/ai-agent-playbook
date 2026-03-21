# prompts/production/

Production-ready prompts that have been reviewed, tested, and approved for use in live systems.

## Rules

- Every file in this directory **must** have a complete metadata header (see [`docs/metadata-format.md`](../../docs/metadata-format.md)).
- Every file should have passing test cases in [`eval/test-cases/`](../../eval/test-cases/).
- Changes to existing files must increment the version number and be logged in [`changelog/CHANGELOG.md`](../../changelog/CHANGELOG.md).

## Domain Sub-folders

Group prompts into domain folders as the collection grows:

```
production/
├── customer-support/
├── summarization/
├── coding/
├── data-extraction/
├── classification/
└── ...
```

## Example Files

| File | Description |
|---|---|
| [`customer-support/handle-refund-v1.md`](customer-support/handle-refund-v1.md) | Refund request handling prompt |
