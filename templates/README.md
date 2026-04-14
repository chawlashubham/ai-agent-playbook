# Templates

Starter files for every artifact type. Copy the relevant template, never edit the originals.

## Available Templates

| Template | Use For | Copy To |
|---|---|---|
| [`agent.md`](agent.md) | New agent definition | `agents/<name>/agent.md` |
| [`config.yaml`](config.yaml) | New agent runtime config | `agents/<name>/config.yaml` |
| [`SKILL.md`](SKILL.md) | New skill definition | `skills/<name>/SKILL.md` |
| [`skill-config.yaml`](skill-config.yaml) | New skill implementation config | `skills/<name>/config.yaml` |
| [`prompt-template.md`](prompt-template.md) | New prompt | `prompts/experimental/<domain>/<name>-v1.md` |
| [`workflow-template.yaml`](workflow-template.yaml) | New workflow | `workflows/<name>/workflow.yaml` |

## How to Use

```bash
# New agent
cp templates/agent.md agents/my-agent/agent.md
cp templates/config.yaml agents/my-agent/config.yaml

# New skill
cp templates/SKILL.md skills/my-skill/SKILL.md
cp templates/skill-config.yaml skills/my-skill/config.yaml

# New prompt
cp templates/prompt-template.md prompts/experimental/my-domain/my-prompt-v1.md

# New workflow
cp templates/workflow-template.yaml workflows/my-workflow/workflow.yaml
```

Fill in all `<PLACEHOLDER>` values after copying. Follow naming conventions in [`docs/naming-conventions.md`](../docs/naming-conventions.md).

## After Creating a New File

1. Add it to the relevant `_index.md` registry (`agents/`, `skills/`, or `workflows/`)
2. Log the addition in [`changelog/CHANGELOG.md`](../changelog/CHANGELOG.md)
