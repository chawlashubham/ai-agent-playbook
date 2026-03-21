# eval/test-cases/

Structured pass/fail tests for individual prompts. Each file maps to one prompt by ID.

## File Naming Convention

```
<prompt-id>-tests.yaml
```

Example: `customer-support-handle-refund-v1-tests.yaml`

## Test Case Format

```yaml
prompt_id: <id>
tests:
  - id: tc-001
    description: <short description>
    input:
      variable_1: value
    assertions:
      - type: contains          # contains | not_contains | json_valid | regex
        value: "expected text"
      - type: not_contains
        value: "text that should NOT appear"
    expected_status: pass       # pass | fail (for negative test cases)
```

## Available Test Files

| File | Prompt |
|---|---|
| [`customer-support-handle-refund-v1-tests.yaml`](customer-support-handle-refund-v1-tests.yaml) | `customer-support-handle-refund-v1` |
