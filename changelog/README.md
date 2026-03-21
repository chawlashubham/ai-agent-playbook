# changelog/

This directory tracks the history of significant changes to prompts, agents, skills, and workflows.

## Files

| File | Description |
|---|---|
| [`CHANGELOG.md`](CHANGELOG.md) | Human-readable log of all notable changes, newest first |

## Format

Changes are logged using the [Keep a Changelog](https://keepachangelog.com/) format, grouped by release date.

```markdown
## [YYYY-MM-DD]

### Added
- prompts/production/customer-support/handle-refund-v1.md

### Changed
- prompts/production/summarization/abstractive-summary-v1.md → v1.1 (improved output format)

### Deprecated
- prompts/experimental/old-cot-prompt.md (superseded by chain-of-thought-summary-v2.md)

### Removed
- agents/legacy-bot/ (decommissioned)
```

## When to Update

Update `CHANGELOG.md` whenever you:
- Add a new production prompt, agent, skill, or workflow
- Change the behaviour of an existing prompt
- Deprecate or remove an artifact
