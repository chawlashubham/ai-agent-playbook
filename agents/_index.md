# Agents Registry

All agents in this playbook. Each agent lives in its own folder with an `agent.md` (primary definition), `config.yaml` (runtime config), and `system-prompt.md`.

## Available Agents

| Agent | Status | Description | Skills Used |
|---|---|---|---|
| [`research-agent`](research-agent/agent.md) | production | Web research and report drafting | web-search, file-reader |
| [`coding-assistant-agent`](coding-assistant-agent/agent.md) | production | Code review, security audit, refactoring | code-execution, web-search |
| [`customer-support-agent`](customer-support-agent/agent.md) | production | Customer query handling, refunds, escalation | web-search |

## Adding a New Agent

1. Create folder: `agents/<domain>-<role>/`
2. Copy [`../templates/agent.md`](../templates/agent.md) → `agents/<name>/agent.md`
3. Copy [`../templates/config.yaml`](../templates/config.yaml) → `agents/<name>/config.yaml`
4. Write `agents/<name>/system-prompt.md`
5. Add a row to the table above
6. Add an entry in `changelog/CHANGELOG.md`

## File Structure per Agent

```
agents/<name>/
├── agent.md          # Primary definition — purpose, capabilities, inputs, outputs
├── config.yaml       # Model, memory, guardrails, skill refs
├── system-prompt.md  # The actual system prompt with {{variables}}
└── examples/         # Sample inputs and outputs
```

## Naming Convention

Agent folders follow `<domain>-<role>` in lowercase kebab-case:
- `research-agent`, `coding-assistant-agent`, `customer-support-agent`
