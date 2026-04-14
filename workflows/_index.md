# Workflows Registry

All multi-step workflows in this playbook. Each workflow lives in its own folder with a `README.md` (human explanation) and `workflow.yaml` (machine-readable step graph).

## Available Workflows

| Workflow | Status | Description | Steps |
|---|---|---|---|
| [`research-and-draft`](research-and-draft/README.md) | production | Web search → read sources → draft report | 3 |
| [`summarize-and-respond`](summarize-and-respond/README.md) | production | Summarise input → generate contextual response | 2 |

## Adding a New Workflow

1. Create folder: `workflows/<verb>-and-<verb>/` or `workflows/<domain>-<pipeline>/`
2. Copy [`../templates/workflow.yaml`](../templates/workflow.yaml) → `workflows/<name>/workflow.yaml`
3. Write `workflows/<name>/README.md` explaining the workflow's purpose and steps
4. Add a row to the table above

## File Structure per Workflow

```
workflows/<name>/
├── README.md        # Human explanation: purpose, steps, when to use
└── workflow.yaml    # Machine-readable step graph with inputs, outputs, refs
```
