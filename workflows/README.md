# workflows/

This directory contains **multi-step prompt chains** and workflow definitions. A workflow orchestrates a sequence of prompts (and optionally skill calls) to accomplish a higher-level task.

## Structure

```
workflows/
├── <workflow-name>/
│   ├── README.md       # Goal, steps, expected inputs/outputs
│   └── workflow.yaml   # Step-by-step workflow definition
```

## When to Use

- Use a workflow when a task requires **more than one prompt** or **conditional branching**.
- Workflows may reference prompts from `prompts/production/` and skills from `skills/`.

## Naming Convention

Folder name: `<verb>-and-<verb>` or `<domain>-<pipeline>` — e.g., `summarize-and-respond`, `research-and-draft`, `ingest-classify-route`.

## Workflow Schema

See [`../templates/workflow-template.yaml`](../templates/workflow-template.yaml) for the canonical template.

## Available Workflows

| Workflow | Description |
|---|---|
| [`summarize-and-respond/`](summarize-and-respond/) | Summarise an input document then generate a user-facing reply |
| [`research-and-draft/`](research-and-draft/) | Search the web, gather context, then draft a document |
