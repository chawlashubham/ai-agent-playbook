# agents/

This directory contains agent configurations and their associated system prompts. Each agent lives in its own sub-folder.

## Structure

```
agents/
├── <agent-name>/
│   ├── README.md          # What the agent does, how to deploy it
│   ├── agent-config.yaml  # Model, tools, memory, and runtime settings
│   └── system-prompt.md   # The agent's persona / system-level instructions
```

## When to Use

- Create a new sub-folder here when you are defining a **reusable, named agent** (e.g., a customer support bot, a research assistant).
- One-off or exploratory agent ideas belong in [`experiments/`](../experiments/) until they stabilise.

## Naming Convention

Folder name: `<domain>-<role>` — e.g., `customer-support-agent`, `research-assistant`, `code-reviewer`.

## Example Agents

| Agent | Description |
|---|---|
| [`customer-support-agent/`](customer-support-agent/) | Handles customer queries and escalations |
| [`research-agent/`](research-agent/) | Searches the web, reads documents, and drafts reports |
