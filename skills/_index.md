# Skills Registry

All reusable skills in this playbook. Each skill lives in its own folder with a `SKILL.md` (human + AI readable definition) and `config.yaml` (implementation details).

## Available Skills

| Skill | Status | Description | Used By |
|---|---|---|---|
| [`web-search`](web-search/SKILL.md) | production | Queries a search engine, returns ranked results | research-agent, coding-assistant-agent, customer-support-agent |
| [`file-reader`](file-reader/SKILL.md) | production | Reads text from local files or remote URLs | research-agent |
| [`code-execution`](code-execution/SKILL.md) | production | Executes sandboxed Python/JS snippets | coding-assistant-agent |

## Adding a New Skill

1. Copy [`../templates/SKILL.md`](../templates/SKILL.md) → `skills/<domain>-<action>/SKILL.md`
2. Copy [`../templates/skill-config.yaml`](../templates/skill-config.yaml) → `skills/<domain>-<action>/config.yaml`
3. Fill in all fields in both files
4. Add a row to the table above
5. Reference the skill from any agent that uses it via `ref: skills/<name>/SKILL.md`

## Naming Convention

Skill folders follow `<domain>-<action>` in lowercase kebab-case:
- `web-search`, `file-reader`, `code-execution`
- `database-query`, `email-send`, `image-generate`
