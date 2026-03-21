# ai-agent-playbook

A clean, scalable, and well-documented repository for managing AI prompts, agent instructions, reusable skills, workflows, templates, experiments, and documentation for LLM-powered systems.

## Purpose

This playbook is used by engineers building AI agents with tools such as:
- **OpenAI / Anthropic** style hosted models
- **Local models** (e.g. Ollama)
- **Orchestration frameworks** (LangChain, custom agents)
- **YAML / Markdown** based configuration

## Directory Structure

```
ai-agent-playbook/
├── prompts/            # All prompt files (production, experimental, components)
│   ├── production/     # Battle-tested, reviewed prompts ready for use in systems
│   ├── experimental/   # Work-in-progress or exploratory prompts
│   └── components/     # Reusable prompt fragments (personas, format instructions, etc.)
├── agents/             # Agent configurations and system prompts
├── skills/             # Reusable tool/skill definitions (YAML)
├── workflows/          # Multi-step workflow prompt chains
├── templates/          # Starter templates for each artifact type
├── experiments/        # Time-boxed experiments with hypothesis and results
├── docs/               # Documentation: naming conventions, versioning, best practices
├── eval/               # Evaluation datasets, test cases, and benchmarks
└── changelog/          # Prompt and agent change history
```

## Quick Start

1. **Browse prompts**: Start in [`prompts/production/`](prompts/production/) for ready-to-use prompts.
2. **Configure an agent**: Copy a starter from [`templates/agent-config-template.yaml`](templates/agent-config-template.yaml).
3. **Add a skill**: Define it in [`skills/`](skills/) following the template in [`templates/skill-template.yaml`](templates/skill-template.yaml).
4. **Chain a workflow**: See examples in [`workflows/`](workflows/) and use [`templates/workflow-template.yaml`](templates/workflow-template.yaml).
5. **Run an experiment**: Create a folder under [`experiments/`](experiments/) following the naming convention.

## Key Conventions

- **File naming**: `<domain>-<intent>-<version>.md` (e.g., `customer-support-handle-refund-v1.md`)
- **Metadata header**: Every prompt file begins with a YAML front-matter block — see [`docs/metadata-format.md`](docs/metadata-format.md)
- **Versioning**: Semantic versioning (`v1`, `v1.1`, `v2`) embedded in filenames and metadata
- **Tagging**: Tags in metadata for discoverability (e.g., `tags: [customer-support, refund, gpt-4]`)

## Documentation

| Document | Description |
|---|---|
| [`docs/naming-conventions.md`](docs/naming-conventions.md) | File and folder naming rules |
| [`docs/versioning.md`](docs/versioning.md) | How to version prompts and configs |
| [`docs/metadata-format.md`](docs/metadata-format.md) | Metadata header specification |
| [`docs/best-practices.md`](docs/best-practices.md) | Guidelines for writing effective prompts |

## Contributing

- Follow the naming conventions in [`docs/naming-conventions.md`](docs/naming-conventions.md).
- Add metadata headers to every new prompt file.
- Log changes in [`changelog/CHANGELOG.md`](changelog/CHANGELOG.md).
- Experimental prompts go into `prompts/experimental/` first; graduate them to `prompts/production/` after review.

