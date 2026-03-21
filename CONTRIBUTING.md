# Contributing to ai-agent-playbook

Thank you for contributing! Follow this guide to keep the repo consistent and
discoverable.

---

## Quick Checklist

Before opening a PR, confirm:

- [ ] File is in the correct folder (`production/`, `experimental/`, or `components/`)
- [ ] File name follows the naming convention (see below)
- [ ] YAML frontmatter block is present and complete (see required fields below)
- [ ] All required frontmatter fields are filled in — no `<PLACEHOLDER>` values left
- [ ] Entry added to [`changelog/CHANGELOG.md`](changelog/CHANGELOG.md)
- [ ] For production prompts: test cases exist in `eval/test-cases/`
- [ ] For deprecated prompts: `superseded_by` field points to the replacement

---

## Naming Conventions

### Prompts

```
<domain>-<intent>[-<variant>]-v<major>[.<minor>].md
```

| Component | Example |
|---|---|
| `domain` | `customer-support`, `coding`, `summarization` |
| `intent` | `handle-refund`, `review-code`, `classify-intent` |
| `variant` (optional) | `cot`, `few-shot`, `json` |
| `version` | `v1`, `v1.1`, `v2` |

**Examples:** `coding-review-security-v1.md`, `classification-classify-intent-few-shot-v1.md`

### Components

```
<type>-<description>-v<version>.md
```

Types: `persona`, `format`, `constraint`, `context`

**Examples:** `persona-helpful-assistant-v1.md`, `constraint-no-pii-v1.md`

### Agents

Folder name: `<domain>-<role>/` (e.g., `coding-assistant-agent/`)

Each agent folder must contain:
- `agent-config.yaml`
- `system-prompt.md`
- `README.md`

---

## Required Frontmatter Fields

Every `.md` file in `prompts/` and `agents/` must begin with a YAML frontmatter block:

```yaml
---
id: <matches filename without extension>
version: "1.0"
status: experimental | production | deprecated
description: >
  One to three sentences describing what this prompt does.
author: <your-github-username or team name>
---
```

Additional fields (domain, intent, tags, model_compatibility, components, test_cases)
are documented in [`docs/metadata-format.md`](docs/metadata-format.md).

---

## Prompt Lifecycle

```
experimental/ → (review + test cases) → production/ → (superseded) → deprecated
```

- **experimental**: Work-in-progress. Minimal metadata OK. Remove if stale > 90 days.
- **production**: Must have complete metadata, test cases, and changelog entry.
- **deprecated**: Set `status: deprecated` and add `superseded_by: <new-id>`.
  Keep the file — do not delete it.

---

## PR Process

1. Branch off `main`: `git checkout -b feat/prompts/my-new-prompt`
2. Create your file(s) following the conventions above.
3. Run the frontmatter validator locally (optional but recommended):
   ```bash
   python .github/scripts/validate_frontmatter.py   # or push and let CI run
   ```
4. Add a `changelog/CHANGELOG.md` entry under today's date.
5. Open a PR with a clear title: `feat(prompts): add coding-review-security-v1`
6. A maintainer will review for naming, metadata completeness, and prompt quality.

---

## Where to Ask for Help

- Open a GitHub Discussion for questions about conventions or design decisions.
- Open a GitHub Issue to report a bug in an existing prompt or config.
