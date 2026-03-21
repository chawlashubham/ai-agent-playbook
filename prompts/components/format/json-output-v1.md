---
id: format-json-output-v1
version: "1.0"
type: component
component_type: format
status: production
tags:
  - format
  - json
  - structured-output
author: ai-team
created: 2024-01-05
updated: 2024-01-05
description: >
  A reusable instruction fragment that constrains the model to respond
  strictly in valid JSON. Use when downstream code will parse the output.
---

# Component: JSON Output Instruction

## Fragment

```
Respond ONLY with valid JSON. Do not include any explanatory text, markdown
code fences, or additional commentary outside the JSON object. Your entire
response must be parseable by `JSON.parse()`.
```

## Usage

Append this fragment to a prompt when structured output is required:

```markdown
<!-- Domain-specific prompt body -->
Extract the following information from the text: name, date, amount.

<!-- Include: components/format/json-output-v1.md -->
Respond ONLY with valid JSON...
```

## Expected Output Shape

Define the JSON schema in the prompt body. Example:

```json
{
  "name": "string",
  "date": "ISO 8601 date string",
  "amount": "number"
}
```

## Notes

- If the model cannot determine a value, use `null` rather than omitting the key.
- Pair with an output validation step in your application layer.
