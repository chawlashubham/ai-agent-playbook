# Metadata Format

Every prompt, component, agent system prompt, and experiment file **must** begin with a YAML front-matter block enclosed in `---` delimiters. This enables automated indexing, search, and validation.

---

## 1. Full Specification

```yaml
---
# ── Required fields ──────────────────────────────────────────────────────────
id: <domain>-<intent>-v<version>
  # Unique identifier matching the file name (without .md).
  # Example: customer-support-handle-refund-v1

version: "1.0"
  # Semantic version string: "1.0", "1.1", "2.0"

domain: <domain>
  # Functional area. Examples: customer-support, summarization, coding,
  # data-extraction, classification, research

intent: <intent>
  # What the prompt achieves. Examples: handle-refund-request,
  # summarize-document, review-code

status: experimental
  # Lifecycle stage: experimental | production | deprecated

description: >
  # One to three sentences. What does this prompt do, when should you use it,
  # and are there any important constraints?

author: <name-or-team>
  # GitHub username, team name, or email

created: YYYY-MM-DD
  # ISO 8601 date the file was first created

updated: YYYY-MM-DD
  # ISO 8601 date of the last meaningful change

# ── Conditional fields ───────────────────────────────────────────────────────
model_compatibility:
  # List all models this prompt has been tested with.
  # Required for production prompts; optional for experimental.
  - gpt-4
  - gpt-4o
  - claude-3-opus
  - ollama/llama3

tags:
  # Lowercase, hyphenated. Used for search and filtering.
  # Include: domain tags, technique tags, model tags.
  - customer-support
  - refund
  - empathy

# ── Optional fields ──────────────────────────────────────────────────────────
components:
  # List of component fragments included in this prompt.
  - persona/helpful-assistant-v1
  - format/json-output-v1

type: prompt
  # Artifact type: prompt | component | system-prompt
  # Default: prompt. Set to component for files in components/.

component_type: persona
  # Only for type: component. Values: persona | format | constraint | context

test_cases: eval/test-cases/<id>-tests.yaml
  # Path to the test cases file for this prompt.

superseded_by: <id-of-newer-version>
  # Only when status: deprecated. Points to the replacement.

hypothesis: >
  # Only for experimental prompts. The testable claim being evaluated.
---
```

---

## 2. Minimal Header (Experimental Prompts)

At a minimum, experimental prompts must include:

```yaml
---
id: summarization-chain-of-thought-v1
version: "1.0"
status: experimental
description: >
  Experimental CoT prompt for document summarization.
author: ai-team
created: 2024-01-10
updated: 2024-01-10
---
```

---

## 3. Full Header (Production Prompts)

Production prompts must include all required and conditional fields:

```yaml
---
id: customer-support-handle-refund-v1
version: "1.0"
domain: customer-support
intent: handle-refund-request
status: production
model_compatibility:
  - gpt-4
  - gpt-4o
  - claude-3-opus
tags:
  - customer-support
  - refund
  - empathy
author: ai-team
created: 2024-01-15
updated: 2024-01-20
description: >
  Guides the agent through a structured refund-handling conversation,
  ensuring empathy and policy compliance.
test_cases: eval/test-cases/customer-support-handle-refund-v1-tests.yaml
---
```

---

## 4. Searching by Metadata

You can search the repository using standard shell tools:

```bash
# Find all production prompts
grep -rl "status: production" prompts/

# Find all prompts tagged with 'refund'
grep -rl "refund" prompts/ --include="*.md"

# Find all prompts compatible with gpt-4o
grep -rl "gpt-4o" prompts/ --include="*.md"
```
