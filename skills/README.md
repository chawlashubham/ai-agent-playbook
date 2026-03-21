# skills/

This directory contains YAML definitions for reusable **tools and skills** that agents can call. A skill corresponds to a callable function or API that the agent can invoke during a conversation.

## When to Use

- Define a skill here whenever you want multiple agents to share the same tool.
- Skills are referenced by name in `agent-config.yaml` files.

## File Naming Convention

```
<domain>-<action>.yaml
```

Examples:
- `web-search.yaml`
- `code-execution.yaml`
- `database-query.yaml`
- `file-reader.yaml`

## Skill Schema

See [`../templates/skill-template.yaml`](../templates/skill-template.yaml) for the canonical template.

```yaml
skill:
  name: web-search
  version: "1.0"
  description: Search the web for up-to-date information.
  input_schema:
    query:
      type: string
      required: true
  output_schema:
    results:
      type: array
  implementation:
    type: api          # api | python_function | shell | langchain_tool
    endpoint: https://api.search.example.com/v1/search
```

## Available Skills

| Skill | Description |
|---|---|
| [`web-search.yaml`](web-search.yaml) | Query a search engine and return results |
| [`code-execution.yaml`](code-execution.yaml) | Run sandboxed code snippets |
| [`file-reader.yaml`](file-reader.yaml) | Read and parse local or remote files |
