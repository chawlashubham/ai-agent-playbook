# ai-agent-playbook

![Prompts](https://img.shields.io/badge/prompts-7-blue?style=flat-square)
![Agents](https://img.shields.io/badge/agents-3-blueviolet?style=flat-square)
![Skills](https://img.shields.io/badge/skills-3-green?style=flat-square)
![Experiments](https://img.shields.io/badge/experiments-2-orange?style=flat-square)
![Last Commit](https://img.shields.io/github/last-commit/chawlashubham/ai-agent-playbook?style=flat-square)
![License](https://img.shields.io/github/license/chawlashubham/ai-agent-playbook?style=flat-square)

A clean, scalable, and well-documented repository for managing AI prompts, agent instructions, reusable skills, workflows, templates, experiments, and documentation for LLM-powered systems.

Works with **Claude**, **GitHub Copilot**, **ChatGPT**, and **Cursor**.

---

## Quick Reference

| I want to... | Start here |
|---|---|
| Use a ready-made prompt | [`prompts/production/`](prompts/production/) |
| Browse available agents | [`agents/_index.md`](agents/_index.md) |
| Browse available skills | [`skills/_index.md`](skills/_index.md) |
| Build a new agent | [`templates/agent.md`](templates/agent.md) |
| Add a new skill | [`templates/SKILL.md`](templates/SKILL.md) |
| Chain agents together | [`workflows/`](workflows/) |
| Test a prompt | [`eval/test-cases/`](eval/test-cases/) |
| Run an experiment | [`experiments/`](experiments/) |
| Understand conventions | [`docs/`](docs/) |

---

## Directory Structure

```
ai-agent-playbook/
│
├── agents/                  # One folder per agent
│   ├── _index.md            # Agent registry
│   └── <name>/
│       ├── agent.md         # Primary definition (purpose, capabilities, I/O)
│       ├── config.yaml      # Model, memory, guardrails, skill refs
│       ├── system-prompt.md # The actual system prompt with {{variables}}
│       └── examples/        # Sample inputs and outputs
│
├── prompts/                 # All prompt files
│   ├── production/          # Battle-tested, reviewed prompts
│   ├── experimental/        # Work-in-progress prompts
│   └── components/          # Reusable fragments (persona, format, constraint)
│
├── skills/                  # One folder per skill
│   ├── _index.md            # Skill registry
│   └── <name>/
│       ├── SKILL.md         # Human + AI readable definition (uppercase)
│       └── config.yaml      # Implementation: endpoint, auth, error handling
│
├── workflows/               # Multi-step agent pipelines
│   ├── _index.md
│   └── <name>/
│       ├── README.md        # Human explanation
│       └── workflow.yaml    # Machine-readable step graph
│
├── tools/                   # AI platform adapter layer
│   ├── claude/              # Claude Code agent files
│   ├── copilot/             # GitHub Copilot .agent.md files (synced to .github/agents/)
│   ├── cursor/              # Cursor .cursorrules
│   └── chatgpt/             # ChatGPT custom GPT exports
│
├── templates/               # Starter files — copy, never edit originals
│   ├── agent.md
│   ├── config.yaml
│   ├── SKILL.md
│   ├── skill-config.yaml
│   ├── prompt-template.md
│   └── workflow-template.yaml
│
├── experiments/             # Time-boxed experiments with hypothesis and results
├── eval/                    # Datasets, test cases, benchmarks
├── docs/                    # Naming conventions, versioning, best practices
└── changelog/               # Change history
```

---

## Key Conventions

| Convention | Rule |
|---|---|
| Agent folder | `agents/<domain>-<role>/` |
| Agent definition | `agent.md` (human narrative) + `config.yaml` (machine config) |
| Skill folder | `skills/<domain>-<action>/` |
| Skill definition | `SKILL.md` (uppercase) + `config.yaml` |
| Prompt naming | `<domain>-<intent>[-<variant>]-v<N>.md` |
| All names | Lowercase kebab-case only |
| Variables | `{{double_braces}}` — never hardcode values |
| Metadata | Every `agent.md`, `SKILL.md`, and prompt starts with YAML frontmatter |

Full naming rules → [`docs/naming-conventions.md`](docs/naming-conventions.md)

---

## Prompt Lifecycle

```
prompts/experimental/ → (review + 3 test cases) → prompts/production/ → deprecated
```

- Never edit an existing production prompt — create a new version file (`v2`, `v1.1`)
- Never delete deprecated prompts — set `status: deprecated` + `superseded_by: <id>`

---

## Documentation

| Document | Description |
|---|---|
| [`docs/naming-conventions.md`](docs/naming-conventions.md) | File and folder naming rules |
| [`docs/versioning.md`](docs/versioning.md) | How to version prompts and configs |
| [`docs/metadata-format.md`](docs/metadata-format.md) | Metadata frontmatter specification |
| [`docs/best-practices.md`](docs/best-practices.md) | Guidelines for writing effective prompts |
| [`agents/_index.md`](agents/_index.md) | All agents with status and links |
| [`skills/_index.md`](skills/_index.md) | All skills with status and links |
| [`workflows/_index.md`](workflows/_index.md) | All workflows with status and links |

---

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the full guide.

Quick checklist:
- Follow naming conventions in [`docs/naming-conventions.md`](docs/naming-conventions.md)
- Add frontmatter to every new `agent.md`, `SKILL.md`, and prompt file
- Add new artifacts to their `_index.md` registry
- Log changes in [`changelog/CHANGELOG.md`](changelog/CHANGELOG.md)
- Experimental prompts go to `prompts/experimental/` first; graduate after review + 3 test cases
