# Changelog

All notable changes to prompts, agents, skills, and workflows are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## [2024-01-28]

### Added

- `prompts/production/customer-support/handle-refund-v1.md` — Production refund-handling prompt with empathy and policy compliance instructions.
- `prompts/experimental/summarization/chain-of-thought-summary-v1.md` — Experimental CoT summarization prompt promoted from `experiments/2024-01-chain-of-thought-summarization/`.
- `prompts/components/persona/helpful-assistant-v1.md` — Reusable helpful-assistant persona component.
- `prompts/components/format/json-output-v1.md` — Reusable JSON output format component.
- `agents/customer-support-agent/` — Customer support agent with config and system prompt.
- `agents/research-agent/` — Research agent with web search and document reading capabilities.
- `skills/web-search.yaml` — Web search skill definition.
- `skills/code-execution.yaml` — Sandboxed code execution skill definition.
- `skills/file-reader.yaml` — File/document reader skill definition.
- `workflows/summarize-and-respond/` — Two-step summarize → respond workflow.
- `workflows/research-and-draft/` — Three-step search → read → draft workflow.
- `eval/test-cases/customer-support-handle-refund-v1-tests.yaml` — Test cases for refund prompt.
- `eval/datasets/customer-support-refund-dataset.json` — Evaluation dataset for refund prompt.
- `eval/benchmarks/summarization-cot-vs-direct.yaml` — CoT vs direct summarization benchmark.
- `experiments/2024-01-chain-of-thought-summarization/` — Completed CoT experiment with hypothesis and results.
- `docs/naming-conventions.md`, `docs/versioning.md`, `docs/metadata-format.md`, `docs/best-practices.md` — Repository documentation.
- `templates/` — Starter templates for prompts, agent configs, skills, and workflows.

---

<!-- Add new entries above this line in the format:

## [YYYY-MM-DD]

### Added
- ...

### Changed
- ...

### Deprecated
- ...

### Removed
- ...

### Fixed
- ...
-->
