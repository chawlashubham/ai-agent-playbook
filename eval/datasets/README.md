# eval/datasets/

Curated input/output pairs for offline evaluation of prompts and agents.

## File Naming Convention

```
<prompt-id>-dataset.json
```

Example: `customer-support-handle-refund-v1-dataset.json`

## Dataset Format

Each dataset is a JSON array of `{input, expected_output}` objects:

```json
[
  {
    "id": "ds-001",
    "description": "Happy path: eligible refund request",
    "input": {
      "company_name": "Acme Corp",
      "refund_policy": "Items may be returned within 30 days.",
      "escalation_threshold": "$500",
      "customer_message": "Order #12345 arrived damaged. I want a refund."
    },
    "expected_output": {
      "decision": "approved",
      "contains": ["refund", "3-5 business days"]
    }
  }
]
```

## Available Datasets

| Dataset | Prompt | Records |
|---|---|---|
| [`customer-support-refund-dataset.json`](customer-support-refund-dataset.json) | `customer-support-handle-refund-v1` | 20 |
