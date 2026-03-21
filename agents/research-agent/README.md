# Research Agent

An AI agent that autonomously searches the web, reads documents, and produces well-structured research reports on a given topic.

## Capabilities

- Search the web for up-to-date information on a topic
- Read and extract key information from web pages and documents
- Synthesise findings into a structured draft report

## Skills Used

| Skill | Purpose |
|---|---|
| `web-search` | Find relevant sources |
| `file-reader` | Read and parse document content |

## Configuration

See [`agent-config.yaml`](agent-config.yaml) for the full configuration.

## How to Deploy

1. Fill in model and API key settings in `agent-config.yaml`.
2. Load `system-prompt.md` as the system message.
3. Register the skills with your orchestration framework.
4. Invoke with a `research_topic` and optional `output_format`.
