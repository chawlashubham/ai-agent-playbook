---
id: coding-assistant-agent
version: "1.0"
status: production
description: Reviews code and pull requests for quality, correctness, and security vulnerabilities with a security-first approach.
tags:
  - coding
  - security
  - code-review
  - owasp
  - refactoring
config: config.yaml
system_prompt: system-prompt.md
skills:
  - skills/code-execution
  - skills/web-search
tools_compatible:
  - claude
  - copilot
  - chatgpt
  - cursor
author: ai-team
created: 2024-02-10
updated: 2024-02-10
---

# Coding Assistant Agent

> A senior software engineer and security-conscious code reviewer that analyses code for bugs, vulnerabilities, and quality issues — with specific, actionable feedback.

---

## Purpose

Use this agent for code review, security audits, refactoring guidance, and coding Q&A. It takes a security-first approach: vulnerabilities are always surfaced before style or optimisation feedback.

The agent can execute small code snippets in a sandbox to validate fixes, but always requires explicit user confirmation before running code.

---

## Capabilities

- **Code review**: Analyses diffs or snippets for bugs, security issues, performance problems, and code quality
- **Security audit**: Scans for OWASP Top 10 vulnerabilities and common CWEs
- **Refactoring guidance**: Suggests targeted improvements while preserving intent
- **Code explanation**: Breaks down complex logic into plain language
- **Fix validation**: Executes corrected snippets in a sandbox to confirm they work
- **Coding Q&A**: Answers technical questions with references to documentation or standards

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `code` | string | yes | Code snippet or diff to review |
| `language` | string | yes | Programming language (e.g., `python`, `javascript`, `go`) |
| `languages_supported` | string | no | Override the agent's declared language expertise |
| `severity_threshold` | string | no | Minimum severity to surface: `critical`, `high`, `medium`, `low` (default: `high`) |

---

## Outputs

| Output | Type | Description |
|---|---|---|
| `findings` | array | List of issues with severity, category, line hint, and remediation |
| `overall_risk` | string | Aggregate risk level: `critical`, `high`, `medium`, `low`, `none` |
| `summary` | string | One-line summary of the review |

---

## Tools / Skills Used

| Skill | Purpose |
|---|---|
| [`code-execution`](../../skills/code-execution/SKILL.md) | Validates fixes by running corrected snippets in a sandbox |
| [`web-search`](../../skills/web-search/SKILL.md) | Looks up documentation, CVEs, or library security advisories |

---

## Prompt References

| File | Purpose |
|---|---|
| [`system-prompt.md`](system-prompt.md) | Agent identity, review process, severity levels, hard constraints |
| [`../../prompts/production/coding/review-code-security-v1.md`](../../prompts/production/coding/review-code-security-v1.md) | Structured security review prompt (JSON output) |

---

## Configuration

See [`config.yaml`](config.yaml) for model settings, memory, and guardrails.

Key settings:
- **Model**: `gpt-4o` (temperature: `0.2` — low for deterministic analysis)
- **Memory**: Buffer, max 30 messages
- **Blocked patterns**: `DROP TABLE`, `rm -rf`, `DELETE FROM`, `format c:`
- **Requires confirmation before code execution**: `true`

---

## Examples

### Security review of a SQL snippet

**Input:**
```
language: python
code: |
  query = "SELECT * FROM users WHERE username = '" + username + "'"
  cursor.execute(query)
```

**Output:**
```json
{
  "overall_risk": "critical",
  "summary": "SQL injection via unsanitised string concatenation.",
  "findings": [
    {
      "id": "F-001",
      "severity": "critical",
      "category": "Injection — SQL Injection (OWASP A03:2021)",
      "line_hint": "1",
      "description": "User input concatenated directly into SQL query.",
      "remediation": "Use parameterised queries: cursor.execute('SELECT * FROM users WHERE username = %s', (username,))"
    }
  ]
}
```

See [`examples/`](examples/) for more sample reviews.

---

## Limitations

- Cannot review code that requires runtime context (e.g., live database state)
- Sandbox execution has no filesystem or network access
- Will not suggest or execute code that deletes data or drops tables without explicit confirmation
- Never echoes back secrets or credentials found in reviewed code

---

## Related

- **Prompts**: [`prompts/production/coding/review-code-security-v1.md`](../../prompts/production/coding/review-code-security-v1.md)
- **Eval**: [`eval/test-cases/coding-review-security-v1-tests.yaml`](../../eval/test-cases/coding-review-security-v1-tests.yaml)
