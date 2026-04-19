# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A content-only repository — no application code, no build system, no package manager. All artifacts are Markdown and YAML files. There is nothing to compile or install.

## Validating frontmatter (the only "test" command)

The CI check can be run locally:

```bash
pip install pyyaml
python -c "
import os, sys, yaml

RULES = {
    'prompt': ['id', 'version', 'status', 'description', 'author'],
    'agent':  ['id', 'version', 'status', 'description', 'author'],
    'skill':  ['id', 'version', 'status', 'description', 'author'],
}

def classify(fp):
    parts = fp.replace('\\\\', '/').split('/')
    fn = os.path.basename(fp)
    if 'prompts' in parts: return 'prompt'
    if 'agents' in parts and fn == 'agent.md': return 'agent'
    if 'skills' in parts and fn == 'SKILL.md': return 'skill'
    return None

def frontmatter(fp):
    c = open(fp, encoding='utf-8').read()
    if not c.startswith('---'): return None
    parts = c.split('---', 2)
    return yaml.safe_load(parts[1]) if len(parts) >= 3 else None

errors = []
for d in ['prompts', 'agents', 'skills']:
    for root, _, files in os.walk(d):
        for f in files:
            if not f.endswith('.md'): continue
            fp = os.path.join(root, f)
            rk = classify(fp)
            if not rk: continue
            fm = frontmatter(fp)
            if not fm: errors.append(f'MISSING FRONTMATTER: {fp}'); continue
            for field in RULES[rk]:
                if field not in fm: errors.append(f'MISSING {field}: {fp}')

[print(e) for e in errors]
sys.exit(1 if errors else 0)
"
```

## Architecture

### Artifact types and their locations

| Artifact | Folder | Key files |
|---|---|---|
| Agent | `agents/<domain>-<role>/` | `agent.md` (narrative definition), `config.yaml` (model/memory/guardrails), `system-prompt.md` |
| Skill | `skills/<domain>-<action>/` | `SKILL.md` (uppercase — definition), `config.yaml` (implementation/auth/errors), `tool-schema.json` (portable tool definition) |
| Prompt | `prompts/production/<domain>/` or `prompts/experimental/<domain>/` | Single versioned `.md` file |
| Reusable fragment | `prompts/components/<type>/` | Persona, format, constraint snippets included by prompts |
| Workflow | `workflows/<name>/` | `workflow.yaml` (step graph), `README.md` |
| Platform adapter | `tools/<platform>/` | Thin wrappers: `tools/copilot/agents/` syncs to `.github/agents/`; `tools/cursor/.cursorrules` |

### How the pieces connect

- `agent.md` declares which skills it uses (`skills:` list) and references `config.yaml` and `system-prompt.md`
- `config.yaml` references skills as `ref: skills/<name>/SKILL.md`
- `system-prompt.md` uses `{{variable_name}}` placeholders; values are declared in `config.yaml` under `prompt_variables:`
- Prompts reference components with `<!-- Include: components/<type>/<file>.md -->` comments
- Workflows reference agents and skills via `agent_ref:` / `skill_ref:` fields in `workflow.yaml`
- Each directory has a `_index.md` registry (`agents/_index.md`, `skills/_index.md`, `workflows/_index.md`) — update these when adding new artifacts

### Prompt lifecycle

```
prompts/experimental/ → (review + 3 test cases in eval/test-cases/) → prompts/production/
```

Never edit a production prompt in place — create a new version file (e.g. `v2`) and set the old one to `status: deprecated` with `superseded_by: <new-id>`.

## Naming rules

- All file and folder names: **lowercase kebab-case** — no spaces, underscores, or camelCase
- Exception: `SKILL.md` is always uppercase
- Prompt files: `<domain>-<intent>[-<variant>]-v<N>.md` (e.g. `coding-review-security-v1.md`)
- Agent folders: `<domain>-<role>` (e.g. `research-agent`)
- Skill folders: `<domain>-<action>` (e.g. `web-search`)
- The `id` field in frontmatter must match the file or folder name exactly

## Required frontmatter

Every `agent.md`, `SKILL.md`, and file under `prompts/` must start with a YAML frontmatter block containing at minimum:

```yaml
---
id: <matches filename or folder name>
version: "1.0"
status: experimental   # experimental | staging | production | deprecated
description: <one-line description>
author: <team-or-username>
---
```

## When adding any new artifact

1. Use the matching template from `templates/` — copy it, never edit the original
2. Add a row to the relevant `_index.md` registry
3. Add an entry to `changelog/CHANGELOG.md` under today's date
4. For new prompts: start in `prompts/experimental/`, add test cases to `eval/test-cases/` before promoting to production

## Critical don'ts

- Never hardcode secrets or real values — use `{{variable_name}}` placeholders
- Never add prose or comments to `config.yaml` files — pure machine config only
- Never put a `README.md` inside an agent or skill folder — `agent.md` and `SKILL.md` serve that purpose
- Never write a prompt that does two things — split and chain via a workflow instead
- Never duplicate persona or format logic across prompts — extract to `prompts/components/`
