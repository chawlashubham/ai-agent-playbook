# Naming Conventions

## 0. Fixed Filenames (Special Cases)

Some files have fixed names — do not rename them:

| Fixed Filename | Location | Purpose |
|---|---|---|
| `agent.md` | `agents/<name>/agent.md` | Primary agent definition (replaces README.md) |
| `config.yaml` | `agents/<name>/config.yaml` | Agent runtime config |
| `system-prompt.md` | `agents/<name>/system-prompt.md` | Agent system prompt |
| `SKILL.md` | `skills/<name>/SKILL.md` | Skill definition (uppercase) |
| `config.yaml` | `skills/<name>/config.yaml` | Skill implementation config |
| `workflow.yaml` | `workflows/<name>/workflow.yaml` | Workflow step graph |
| `_index.md` | `agents/`, `skills/`, `workflows/` | Registry file for each directory |

`SKILL.md` is always uppercase — this visually distinguishes it from other markdown files and aligns with the SkillsMP convention.

Consistent naming makes prompts easy to discover, version, and compose. Follow these rules across the entire repository.

---

## 1. General Rules

- Use **lowercase kebab-case** for all file and folder names.
- No spaces, underscores, or camelCase.
- Use hyphens (`-`) as separators.
- Keep names descriptive but concise (3–6 words maximum).

---

## 2. Prompt Files

### Pattern

```
<domain>-<intent>[-<variant>]-v<major>[.<minor>].md
```

### Components

| Component | Description | Example |
|---|---|---|
| `<domain>` | The functional area or subject | `customer-support`, `summarization`, `coding` |
| `<intent>` | What the prompt achieves | `handle-refund`, `summarize-document`, `review-code` |
| `<variant>` | Optional differentiator | `cot` (chain-of-thought), `few-shot`, `json` |
| `v<version>` | Semantic version, no dots for major | `v1`, `v1.1`, `v2` |

### Examples

```
customer-support-handle-refund-v1.md
summarization-abstractive-cot-v2.md
coding-review-security-focus-v1.1.md
data-extraction-invoice-json-v1.md
```

---

## 3. Component Files

```
<component-type>-<description>-v<version>.md
```

| Component Type | Example File |
|---|---|
| `persona` | `persona-helpful-assistant-v1.md` |
| `format` | `format-json-output-v1.md` |
| `constraint` | `constraint-no-pii-v1.md` |
| `context` | `context-product-catalog-v1.md` |

---

## 4. Agent Folders

```
agents/<domain>-<role>/
```

Examples:
- `customer-support-agent/`
- `research-assistant/`
- `code-reviewer/`
- `data-pipeline-orchestrator/`

Files inside each agent folder:
- `README.md` — always required
- `agent-config.yaml` — always required
- `system-prompt.md` — always required

---

## 5. Skill Files

```
skills/<domain>-<action>.yaml
```

Examples:
- `web-search.yaml`
- `code-execution.yaml`
- `database-query.yaml`
- `email-send.yaml`

---

## 6. Workflow Folders

```
workflows/<verb>-and-<verb>/
workflows/<domain>-<pipeline-description>/
```

Examples:
- `summarize-and-respond/`
- `research-and-draft/`
- `ingest-classify-route/`

Files inside each workflow folder:
- `README.md` — always required
- `workflow.yaml` — always required

---

## 7. Experiment Folders

```
experiments/YYYY-MM-<short-description>/
```

Examples:
- `2024-01-chain-of-thought-summarization/`
- `2024-03-ollama-llama3-benchmark/`

---

## 8. IDs

The `id` field in metadata should match the file name (without the `.md` extension):

```yaml
id: customer-support-handle-refund-v1
```

---

## 9. Tags

Tags should be lowercase, hyphenated, and drawn from a shared vocabulary where possible:

**Domain tags:** `customer-support`, `summarization`, `coding`, `data-extraction`, `classification`

**Technique tags:** `chain-of-thought`, `few-shot`, `zero-shot`, `rag`, `function-calling`

**Model tags:** `gpt-4`, `gpt-4o`, `claude-3`, `ollama`, `llama3`

**Status tags:** `production`, `experimental`, `deprecated`
