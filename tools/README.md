# Tools — AI Platform Adapter Layer

This directory contains platform-specific adapter files for each AI tool that consumes agents, prompts, or skills from this playbook. The goal is a single source of truth in `agents/` and `prompts/`, with thin adapter layers here for each tool's specific format requirements.

## Supported Tools

| Tool | Folder | Format | Notes |
|---|---|---|---|
| Claude Code | [`claude/`](claude/) | `.md` agent files | Claude Code reads agent files directly |
| GitHub Copilot | [`copilot/`](copilot/) | `.agent.md` files | Synced to `.github/agents/` by CI |
| Cursor | [`cursor/`](cursor/) | `.cursorrules` | Cursor-specific instruction format |
| ChatGPT | [`chatgpt/`](chatgpt/) | GPT instruction exports | Custom GPT configuration |

## How It Works

1. **Canonical definitions** live in `agents/<name>/agent.md` and `prompts/`
2. **Tool adapters** in this directory reference or adapt those definitions for each platform's format
3. **CI keeps adapters in sync** — when an agent changes, CI reminds you to update the relevant tool adapter

## Adding a New Tool

1. Create a subfolder: `tools/<tool-name>/`
2. Add a `README.md` explaining the format and how to use it
3. Add the tool to the table above
4. If the tool has a specific consumption directory (e.g., `.github/agents/`), document the sync process

## Copilot Sync

The `.github/agents/` directory is the actual consumption point for GitHub Copilot. Files in `tools/copilot/agents/` are the source; CI copies them to `.github/agents/` on push to `main`.
