# templates/

Starter templates for every artifact type in this repository. Copy the relevant template when creating a new file and fill in the placeholders.

## Available Templates

| Template | Use For |
|---|---|
| [`prompt-template.md`](prompt-template.md) | Any new prompt file (production, experimental, or component) |
| [`agent-config-template.yaml`](agent-config-template.yaml) | A new agent configuration |
| [`skill-template.yaml`](skill-template.yaml) | A new skill / tool definition |
| [`workflow-template.yaml`](workflow-template.yaml) | A new multi-step workflow |

## How to Use

```bash
# Example: create a new production prompt
cp templates/prompt-template.md prompts/production/customer-support/handle-refund-v1.md
# Then edit the new file, filling in all <PLACEHOLDER> fields.
```

All templates include inline comments explaining each field.
