---
id: research-agent
version: "1.0"
status: production
description: Searches the web, reads documents, and drafts structured research reports with citations.
tags:
  - research
  - web-search
  - report-generation
  - information-retrieval
config: config.yaml
system_prompt: system-prompt.md
skills:
  - skills/web-search
  - skills/file-reader
tools_compatible:
  - claude
  - copilot
  - chatgpt
author: ai-team
created: 2024-01-15
updated: 2024-01-15
---

# Research Agent

> An expert research assistant that searches the web, reads sources, synthesises findings, and produces structured reports with numbered citations.

---

## Purpose

Use this agent when you need to investigate a topic, gather information from multiple sources, or produce a research report. It searches up to 10 sources, reads their content, and synthesises findings into a clear, citation-backed report.

This agent is **read-only** — it does not write files, run code, or take actions beyond searching and reading.

---

## Capabilities

- **Web search**: Finds up to 10 authoritative sources for a given topic
- **Document reading**: Extracts full-text content from URLs and local files
- **Synthesis**: Identifies key themes, consensus views, and contradictions across sources
- **Report drafting**: Produces structured markdown reports with inline citations
- **Conflict flagging**: Explicitly surfaces contradictory information rather than silently resolving it

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `topic` | string | yes | The research question or subject to investigate |
| `output_format` | string | no | Desired report format (default: `markdown report with sections`) |
| `max_sources` | integer | no | Number of sources to search (default: `10`) |

---

## Outputs

| Output | Type | Description |
|---|---|---|
| `report` | string (markdown) | Structured research report with summary, findings, analysis, and citations |
| `sources` | array | List of sources consulted with titles and URLs |

---

## Tools / Skills Used

| Skill | Purpose |
|---|---|
| [`web-search`](../../skills/web-search/SKILL.md) | Finds authoritative sources for the research topic |
| [`file-reader`](../../skills/file-reader/SKILL.md) | Extracts full-text content from result URLs |

---

## Prompt References

| File | Purpose |
|---|---|
| [`system-prompt.md`](system-prompt.md) | Agent identity, research process, output format |

---

## Configuration

See [`config.yaml`](config.yaml) for model settings, memory, and guardrails.

Key settings:
- **Model**: `gpt-4o` (temperature: `0.5`)
- **Memory**: Summary memory, max 50 messages
- **Max turns**: 30 | **Max search calls**: 20

---

## Examples

### Research report on a technical topic

**Input:**
```
topic: "impact of few-shot prompting on classification accuracy"
max_sources: 8
```

**Output summary:**
> The agent searches 8 sources, synthesises findings across papers and posts,
> and returns a markdown report with a 3-sentence summary, numbered key findings,
> detailed section analysis, and a sources list.

See [`examples/research-report-example.md`](examples/research-report-example.md) for a full sample output.

---

## Limitations

- Does not write or edit files in the workspace
- Cannot execute code or take actions beyond searching and reading
- Source quality depends on the search API's index
- Maximum 10 sources per run (configurable via `max_sources`)
- Will explicitly state when fewer than 3 sources are found

---

## Related

- **Workflows**: [`research-and-draft`](../../workflows/research-and-draft/README.md)
- **Eval**: [`eval/test-cases/`](../../eval/test-cases/)
- **Copilot version**: [`../../.github/agents/research-agent.agent.md`](../../.github/agents/research-agent.agent.md)
