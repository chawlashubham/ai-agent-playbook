---
id: constraint-no-pii-v1
version: "1.0"
type: component
component_type: constraint
status: production
tags:
  - constraint
  - pii
  - privacy
  - gdpr
  - data-safety
author: ai-team
created: 2024-02-15
updated: 2024-02-15
description: >
  A reusable constraint fragment that instructs the model to detect and redact
  personally identifiable information (PII) from its outputs. Include in any
  prompt that processes user-submitted documents or messages where PII must
  not appear in model output or logs.
---

# Component: No-PII Output Constraint

## Fragment

```
Before returning any output, scan it for personally identifiable information (PII).
Redact or replace the following categories if found:
- Full names → [NAME]
- Email addresses → [EMAIL]
- Phone numbers → [PHONE]
- Physical addresses → [ADDRESS]
- National ID numbers, passport numbers, or tax IDs → [ID]
- Payment card numbers → [CARD]
- Dates of birth → [DOB]
- IP addresses → [IP]

If a PII value is required for the structured output schema (e.g., a named field),
replace it with the appropriate placeholder token above rather than omitting the field.
Retain all non-PII values (e.g., invoice totals, quantities, dates that are not DOB).
```

## Usage

Append this fragment to any prompt that processes user documents and must not
leak PII in its output or downstream logs:

```markdown
<!-- Prompt body: extract invoice details -->
Extract the following from the invoice: vendor, date, total, line items.

<!-- Include: components/constraint/no-pii-v1.md -->
Before returning any output, scan it for personally identifiable information...
```

## Referenced By

- `prompts/experimental/data-extraction/extract-invoice-json-v1.md`

## Notes

- This constraint is **best-effort at the model level**. Do not rely on it as
  the sole PII protection mechanism in production systems.
- Always pair with server-side PII detection and masking for production data pipelines.
- For stricter compliance requirements (e.g., HIPAA, GDPR), apply deterministic
  regex-based redaction before passing data to the model.
