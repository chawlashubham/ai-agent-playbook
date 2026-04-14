# ai-agent-playbook — Copilot Instructions

This repo is a framework for managing AI prompts, agents, skills, workflows,
and experiments for LLM-powered systems. All artifacts are Markdown or YAML
files — there is no application code.

---

## Core Conventions

**Every `agent.md`, `SKILL.md`, and `.md` under `prompts/` must start with a YAML frontmatter block.**
Required fields: `id`, `version`, `status`, `description`, `author`.
Full spec → [docs/metadata-format.md](../docs/metadata-format.md)

**File naming is strict — follow these patterns exactly:**
- Prompts: `<domain>-<intent>[-<variant>]-v<version>.md` (e.g., `coding-review-security-v1.md`)
- Components: `<type>-<description>-v<version>.md` (e.g., `constraint-no-pii-v1.md`)
- Agent folders: `<domain>-<role>/` with `agent.md`, `config.yaml`, `system-prompt.md`
- Skill folders: `<domain>-<action>/` with `SKILL.md` (uppercase) and `config.yaml`
- All names: lowercase kebab-case only — no spaces, underscores, or camelCase

Full naming rules → [docs/naming-conventions.md](../docs/naming-conventions.md)

---

## Folder Roles

| Folder | What goes here |
|---|---|
| `agents/<name>/agent.md` | Primary agent definition — purpose, capabilities, I/O, skill refs |
| `agents/<name>/config.yaml` | Machine config — model, memory, guardrails, skill refs |
| `agents/<name>/system-prompt.md` | The actual system prompt with `{{variables}}` |
| `prompts/production/` | Battle-tested prompts — complete metadata + test cases required |
| `prompts/experimental/` | WIP prompts — minimal metadata OK, remove if stale >90 days |
| `prompts/components/` | Reusable fragments (persona, format, constraint, context) |
| `skills/<name>/SKILL.md` | Skill definition — human + AI readable |
| `skills/<name>/config.yaml` | Skill implementation — endpoint, auth, error handling |
| `tools/copilot/agents/` | Source of truth for Copilot `.agent.md` files (synced to `.github/agents/`) |
| `tools/cursor/` | Cursor `.cursorrules` |
| `eval/` | Datasets, test cases, benchmarks — link from prompt `test_cases` field |
| `experiments/` | One folder per experiment: `hypothesis.md`, `results.md`, `README.md` |
| `templates/` | Starter files — copy, don't edit the originals |

---

## Prompt Lifecycle

```
prompts/experimental/ → (review + test cases) → prompts/production/ → deprecated
```

- **Never edit an existing production prompt** — create a new version file (`v2`, `v1.1`)
- **Never delete deprecated prompts** — set `status: deprecated` + `superseded_by: <id>`
- **Promote to production only when** 3+ test cases exist in `eval/test-cases/`

Details → [docs/versioning.md](../docs/versioning.md)

---

## Agent Structure

Each agent folder must contain exactly these files:

```
agents/<name>/
├── agent.md         ← primary definition (replaces README.md)
├── config.yaml      ← pure machine config (no prose)
├── system-prompt.md ← system prompt with {{variables}}
└── examples/        ← sample inputs and outputs
```

Skills are referenced in `config.yaml` as:
```yaml
skills:
  - ref: skills/<name>/SKILL.md
```

---

## Skill Structure

Each skill folder must contain exactly these files:

```
skills/<name>/
├── SKILL.md    ← definition (uppercase filename)
└── config.yaml ← implementation details
```

---

## Critical Don'ts

- **No hardcoded secrets or PII** — always use `{{variable_name}}` placeholders
- **No ambiguous output** — every prompt must define its output format explicitly
- **No duplicate persona/format logic** — extract to `prompts/components/` instead
- **No multi-intent prompts** — if a prompt does two things, split it and chain in a workflow
- **No README.md in agent or skill folders** — `agent.md` and `SKILL.md` serve that purpose

Best practices → [docs/best-practices.md](../docs/best-practices.md)

---

## After Adding Any New Artifact

1. Add it to the relevant `_index.md` registry (`agents/`, `skills/`, or `workflows/`)
2. Log the change in `changelog/CHANGELOG.md`

---

## CI

`validate-frontmatter.yml` blocks merges if any `agent.md`, `SKILL.md`, or `.md` in `prompts/`
is missing required frontmatter fields. Always run locally before pushing:

```bash
pip install pyyaml
python -c "$(grep -A999 'python - <<' .github/workflows/validate-frontmatter.yml | tail -n+2 | head -n-2)"
```

---

## Changelog

Every PR that adds or changes a prompt, agent, skill, or workflow must include
an entry in `changelog/CHANGELOG.md` under today's date.

See [CONTRIBUTING.md](../CONTRIBUTING.md) for the full PR checklist.
