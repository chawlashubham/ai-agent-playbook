# ai-agent-playbook — Copilot Instructions

This repo is a framework for managing AI prompts, agents, skills, workflows,
and experiments for LLM-powered systems. All artifacts are Markdown or YAML
files — there is no application code.

## Core Conventions

**Every `.md` file under `prompts/` and `agents/` must start with a YAML frontmatter block.**
Required fields: `id`, `version`, `status`, `description`, `author`.
Full spec → [docs/metadata-format.md](../docs/metadata-format.md)

**File naming is strict — follow these patterns exactly:**
- Prompts: `<domain>-<intent>[-<variant>]-v<version>.md` (e.g., `coding-review-security-v1.md`)
- Components: `<type>-<description>-v<version>.md` (e.g., `constraint-no-pii-v1.md`)
- Agent folders: `<domain>-<role>/` with `agent-config.yaml`, `system-prompt.md`, `README.md`
- Skills: `<domain>-<action>.yaml`
- All names: lowercase kebab-case only — no spaces, underscores, or camelCase

Full naming rules → [docs/naming-conventions.md](../docs/naming-conventions.md)

## Prompt Lifecycle

```
prompts/experimental/ → (review + test cases) → prompts/production/ → deprecated
```

- **Never edit an existing production prompt** — create a new version file (`v2`, `v1.1`)
- **Never delete deprecated prompts** — set `status: deprecated` + `superseded_by: <id>`
- **Promote to production only when** 3+ test cases exist in `eval/test-cases/`

Details → [docs/versioning.md](../docs/versioning.md)

## Folder Roles

| Folder | What goes here |
|---|---|
| `prompts/production/` | Battle-tested prompts — complete metadata + test cases required |
| `prompts/experimental/` | WIP prompts — minimal metadata OK, remove if stale >90 days |
| `prompts/components/` | Reusable fragments (persona, format, constraint, context) |
| `agents/` | One folder per agent: `agent-config.yaml`, `system-prompt.md`, `README.md` |
| `eval/` | Datasets, test cases, benchmarks — link from prompt `test_cases` field |
| `experiments/` | One folder per experiment: `hypothesis.md`, `results.md`, `README.md` |
| `templates/` | Starter files — copy, don't edit the originals |

## Critical Don'ts

- **No hardcoded secrets or PII** — always use `{{variable_name}}` placeholders
- **No ambiguous output** — every prompt must define its output format explicitly
- **No duplicate persona/format logic** — extract to `prompts/components/` instead
- **No multi-intent prompts** — if a prompt does two things, split it and chain in a workflow

Best practices → [docs/best-practices.md](../docs/best-practices.md)

## CI

`validate-frontmatter.yml` blocks merges if any `.md` in `prompts/` or `agents/` is
missing required frontmatter fields. Always run locally before pushing:

```bash
pip install pyyaml
python -c "$(grep -A999 'python - <<' .github/workflows/validate-frontmatter.yml | tail -n+2 | head -n-2)"
```

## Changelog

Every PR that adds or changes a prompt, agent, skill, or workflow must include
an entry in `changelog/CHANGELOG.md` under today's date.

See [CONTRIBUTING.md](../CONTRIBUTING.md) for the full PR checklist.
