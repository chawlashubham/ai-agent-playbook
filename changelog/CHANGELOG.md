# Changelog

## 2026-04-14

### Restructure: modular agent + skill architecture

**agents/**
- Added `agent.md` (primary definition) to all 3 agents: `research-agent`, `coding-assistant-agent`, `customer-support-agent`
- Renamed `agent-config.yaml` → `config.yaml` in all agent folders (pure machine config, no prose)
- Added `examples/` folder to each agent with sample outputs
- Added `_index.md` registry listing all agents

**skills/**
- Restructured flat YAML skills into `skills/<name>/` folders with `SKILL.md` + `config.yaml`
  - `skills/web-search.yaml` → `skills/web-search/SKILL.md` + `skills/web-search/config.yaml`
  - `skills/file-reader.yaml` → `skills/file-reader/SKILL.md` + `skills/file-reader/config.yaml`
  - `skills/code-execution.yaml` → `skills/code-execution/SKILL.md` + `skills/code-execution/config.yaml`
- Added `skills/_index.md` registry

**workflows/**
- Added `workflows/_index.md` registry

**tools/** (new directory)
- Added `tools/` adapter layer for AI platform-specific formats
- `tools/copilot/agents/research-agent.agent.md` — source of truth for Copilot agent (previously only in `.github/agents/`)
- `tools/cursor/.cursorrules` — Cursor-specific repo conventions

**templates/**
- Added `templates/agent.md` — new agent definition template
- Added `templates/config.yaml` — new agent runtime config template
- Added `templates/SKILL.md` — new skill definition template
- Added `templates/skill-config.yaml` — new skill implementation config template
- Updated `templates/README.md` with new file inventory and usage instructions

**CI**
- Updated `validate-frontmatter.yml` to validate `agent.md` and `SKILL.md` files in addition to prompts

**Docs**
- Updated `README.md` — new directory structure diagram, Quick Reference table
- Updated `.github/copilot-instructions.md` — new agent/skill structure, folder roles, critical don'ts

All notable changes to prompts, agents, skills, and workflows are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## [2026-03-22]

### Added

- `prompts/production/coding/review-code-security-v1.md` — Security-focused code review prompt; returns OWASP-aligned JSON findings with severity ratings and remediation guidance.
- `prompts/production/customer-support/handle-refund-v2.md` — Improved refund handler with customer tier-awareness (`standard`/`premium`/`enterprise`), structured JSON output, and tiered escalation thresholds. Supersedes v1.
- `prompts/experimental/data-extraction/extract-invoice-json-v1.md` — Invoice data extraction prompt; demonstrates three-component composition (persona + format + constraint/no-pii-v1).
- `prompts/experimental/classification/classify-intent-v1.md` — Zero-shot intent classification with confidence scoring and reasoning output.
- `prompts/components/constraint/no-pii-v1.md` — Reusable PII redaction constraint component.
- `agents/coding-assistant-agent/` — New coding assistant agent with code-execution + web-search skills, security-first persona, and destructive-operation guardrails.
- `eval/datasets/coding-review-security-dataset.json` — Evaluation dataset for security code review prompt (safe code, SQL injection, XSS cases).
- `eval/test-cases/coding-review-security-v1-tests.yaml` — Test cases for the security review prompt including an adversarial prompt-injection test.
- `eval/benchmarks/classification-few-shot-vs-zero-shot.yaml` — Benchmark comparing zero-shot vs few-shot intent classification (few-shot wins: 91% vs 78% accuracy).
- `experiments/2024-02-few-shot-vs-zero-shot-classification/` — Completed experiment with hypothesis and results; few-shot variant promoted.
- `.github/workflows/validate-frontmatter.yml` — CI workflow that validates required YAML frontmatter fields on every PR touching `prompts/` or `agents/`.
- `CONTRIBUTING.md` — Contributor guide covering naming conventions, metadata rules, prompt lifecycle, and PR process.

### Deprecated

- `prompts/production/customer-support/handle-refund-v1.md` — Superseded by `handle-refund-v2`. File retained for reference.

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
