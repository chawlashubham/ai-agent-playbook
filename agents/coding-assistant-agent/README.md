# Coding Assistant Agent

An AI agent that reviews code for quality, correctness, and security vulnerabilities.
Designed for use in developer workflows including PR reviews, security audits, and code Q&A.

## Capabilities

- **Security-focused code review** — detects OWASP Top 10 vulnerabilities and common CWEs
- **Pull request review** — analyses diffs and provides line-level feedback
- **Refactoring guidance** — suggests minimal, safe improvements
- **Code explanation** — translates complex code into plain language
- **General coding Q&A** — answers development questions with authoritative guidance

## Skills Used

| Skill | Purpose |
|---|---|
| `code-execution` | Run sandboxed snippets to verify fixes |
| `web-search` | Look up CVE details, library versions, and documentation |

## Configuration

| File | Description |
|---|---|
| [`agent-config.yaml`](agent-config.yaml) | Model, skills, memory, and guardrails |
| [`system-prompt.md`](system-prompt.md) | Agent persona and instructions |

## Configuration Variables

| Variable | Description |
|---|---|
| `{{languages_supported}}` | Comma-separated list of languages to review (e.g., `Python, JavaScript, Go`) |
| `{{severity_threshold}}` | Minimum severity level to surface (e.g., `high`) |

## Guardrails

- Blocks destructive operations (`DROP TABLE`, `rm -rf`, `DELETE FROM`) without explicit confirmation
- No network requests without user approval
- Never echoes back secrets or PII found in reviewed code
- Code execution requires confirmation before running

## Related Prompts

- [`prompts/production/coding/review-code-security-v1.md`](../../prompts/production/coding/review-code-security-v1.md) — standalone security review prompt used by this agent
