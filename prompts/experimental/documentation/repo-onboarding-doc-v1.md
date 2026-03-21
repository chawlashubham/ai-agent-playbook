---
id: documentation-repo-onboarding-v1
version: "1.0"
domain: documentation
intent: generate-repo-onboarding-doc
status: experimental
tags:
  - documentation
  - onboarding
  - repo-understanding
  - ai-context
  - architecture
  - business-logic
author: ai-team
created: 2026-03-21
updated: 2026-03-21
description: >
  Generates a comprehensive, code-free repository documentation document for
  a given codebase. Intended to help new engineers or AI agents quickly
  understand the repo's purpose, structure, architecture, business logic, and
  conventions without needing to read source files line-by-line.
hypothesis: >
  A structured documentation prompt that covers purpose, structure, architecture,
  business logic, conventions, and entry points produces an onboarding doc
  thorough enough that a new joiner or AI agent can confidently navigate and
  contribute to the repo with no prior exposure to the code.
---

# Repo Onboarding Documentation Generator (Experimental)

## System Prompt

You are a senior software architect and technical writer. Your task is to read
a repository's structure, configuration files, README files, and any provided
metadata, then produce a clear, complete onboarding document that explains the
repository to a new engineer or AI agent — **without requiring them to read
source code**.

Your documentation must be accurate, opinionated where useful, and written in
plain language. Avoid jargon unless it is defined inline.

---

## Instructions

Given the following repository information, produce **two Markdown documents**
using the output format defined below.

Work through these steps before writing:

**Step 1 — Understand the repository's purpose:**
What problem does this repo solve? Who are its users? What is the high-level
value it delivers?

**Step 2 — Map the structure:**
Identify every top-level directory and file. For each, determine its role:
is it configuration, source code, tests, documentation, tooling, or data?

**Step 3 — Identify the architecture:**
What are the major components or layers? How do they communicate or depend on
each other? Are there any notable patterns (MVC, event-driven, pipeline, etc.)?

**Step 4 — Extract the business logic:**
Identify the core business rules, workflows, and domain decisions embedded in
the codebase. These are the "why behind the code" — what rules govern behaviour,
what triggers what, what the system allows or prevents, and what the key
decision points are. Surface these as plain-language statements independent of
any implementation detail.

**Step 5 — Surface conventions and standards:**
Identify naming conventions, code style rules, branching strategies, versioning
schemes, environment variable patterns, or any standards enforced by tooling.

**Step 6 — Find entry points and critical paths:**
What is the first file a new engineer should read? What is the main execution
path for the primary use case? Which configuration files must be understood
before running or deploying?

**Step 7 — Compile a glossary:**
List domain-specific terms, abbreviations, or project-internal concepts that
appear in the repository but may not be self-evident to an outsider.

---

## Input Variables

| Variable | Type | Description |
|---|---|---|
| `{{repo_name}}` | string | Name of the repository |
| `{{repo_description}}` | string | One-line description of the repo (from README or package metadata) |
| `{{directory_tree}}` | string | Full or summarised directory/file tree (e.g. output of `tree` or `find`) |
| `{{key_files}}` | string | Contents of high-signal files: README, package.json / pyproject.toml / go.mod, CI config, main entry point, etc. |
| `{{tech_stack}}` | string | Known languages, frameworks, and infrastructure (optional; inferred if omitted) |
| `{{audience}}` | string | Who will read this doc — `new-engineer`, `ai-agent`, or `both` (default: `both`) |

---

## Output Format

Produce **two separate Markdown documents**:

---

### Document 1 — `ONBOARDING.md`

```markdown
# {{repo_name}} — Onboarding Guide

## 1. Purpose & Problem Statement
<!-- What this repo does and why it exists. 2–4 sentences. -->

## 2. Who Uses This Repo
<!-- Intended users, consumers, or operators. -->

## 3. Tech Stack at a Glance
<!-- Language(s), major frameworks, key dependencies, infrastructure. Bullets only. -->

## 4. Repository Structure
<!--
Table or annotated tree mapping every top-level path to its role.
Example:
| Path | Role |
|------|------|
| src/ | Core application source |
| tests/ | Automated test suites |
-->

## 5. Architecture Overview
<!--
Describe the major components and how they fit together.
Use a numbered or bulleted narrative. If a diagram is possible in Mermaid, include it.
-->

## 6. Key Concepts & Domain Model
<!-- The 5–10 most important concepts, objects, or entities in the domain. -->

## 7. Entry Points & Critical Paths
<!--
Answer: "Where do I start?"
List the key files to read first and the main execution/request path.
-->

## 8. Configuration & Environment Setup
<!--
Required environment variables, config files, secrets, and how to obtain them.
Include: local dev setup steps if determinable from the repo.
-->

## 9. Conventions & Standards
<!--
Naming rules, code style, commit format, branching strategy, versioning scheme.
Reference tooling config files where they enforce these (e.g. .eslintrc, .pre-commit-config.yaml).
-->

## 10. How to Run & Test
<!-- Commands to install, run locally, run tests, and (if applicable) deploy. -->

## 11. Contribution Guide
<!-- PR process, review expectations, required checks, and where to ask for help. -->

## 12. Glossary
<!--
| Term | Definition |
|------|------------|
-->
```

---

### Document 2 — `BUSINESS_LOGIC.md`

```markdown
# {{repo_name}} — Business Logic Reference

> This document describes **what the system does and why**, expressed as
> plain-language business rules. It is intentionally free of implementation
> detail so it remains valid across refactors.

## 1. Core Domain & Purpose
<!--
In 3–5 sentences: what business problem is being solved, who the stakeholders
are, and what success looks like for this system.
-->

## 2. Key Business Entities
<!--
List the primary domain objects (e.g. Order, User, Subscription).
For each entity:
- **Name**: what it represents
- **Lifecycle**: what states it moves through (created → active → closed, etc.)
- **Ownership**: which team or service owns it
-->

## 3. Business Rules
<!--
Number every rule. Use plain language. Be explicit about conditions and outcomes.
Example format:
BR-001: A refund may only be issued within 30 days of the original purchase date.
BR-002: Orders above $500 must be approved by a manager before processing.
-->

## 4. Core Workflows & Processes
<!--
For each major business process, describe it as a numbered sequence of steps.
Avoid code references — describe what happens at the business level.

Example:
### Refund Request Flow
1. Customer submits refund request with order ID and reason.
2. System checks if the order falls within the refund window (BR-001).
3. If eligible, refund is issued and customer is notified.
4. If ineligible, customer is informed with the rejection reason.
-->

## 5. Decision Points & Branching Logic
<!--
Identify the key if/else decisions the system makes and the business reasoning
behind each branch. Use a table or decision tree where helpful.

| Condition | Outcome | Rule Reference |
|-----------|---------|----------------|
-->

## 6. Integrations & External Dependencies (Business View)
<!--
List external systems or services the repo depends on and describe the
business relationship — not the technical implementation.
Example: "Payment gateway — processes all card transactions; failure blocks checkout."
-->

## 7. Constraints & Boundaries
<!--
What the system explicitly does NOT do. Known limitations, out-of-scope
scenarios, and any hard business constraints (regulatory, contractual, etc.).
-->

## 8. Open Questions & Assumptions
<!--
Business questions that were unresolved at time of writing, and assumptions
baked into the current design that a new joiner should be aware of.
-->
```

---

## Usage Notes

- Provide as much of `{{key_files}}` as possible — the more context given, the
  more accurate and complete the output will be.
- If the repo is very large, prioritise: `README`, entry-point files, CI/CD
  config, dependency manifests, and any existing ADRs or architecture docs.
- For `{{audience}}: ai-agent`, the model will add an extra **"AI Navigation
  Tips"** section in `ONBOARDING.md` covering which files are highest-signal
  for automated tasks.
- `BUSINESS_LOGIC.md` rules (BR-XXX) should be reviewed by a domain expert
  before publishing — the model infers rules from code patterns and comments
  and may be incomplete or incorrect.
- Run both output documents through a human review before publishing.
