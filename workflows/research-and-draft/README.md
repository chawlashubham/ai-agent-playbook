# Workflow: Research and Draft

Searches the web for information on a topic, gathers and reads relevant sources, then produces a structured draft document.

## Goal

Automate the research–writing pipeline: from a topic or question to a ready-to-review draft.

## Steps

1. **search** — Use the `web-search` skill to find relevant sources.
2. **read** — Use the `file-reader` skill to extract content from top results.
3. **draft** — Use the research agent system prompt to synthesise sources into a report.

## Inputs

| Variable | Type | Description |
|---|---|---|
| `topic` | string | The research topic or question |
| `num_sources` | integer | How many sources to read (default: 5) |
| `output_format` | string | Output format description (default: "markdown report") |

## Output

A structured draft report with cited sources.

## Configuration

See [`workflow.yaml`](workflow.yaml).
