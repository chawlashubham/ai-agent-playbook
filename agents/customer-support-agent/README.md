# Customer Support Agent

An AI agent that handles inbound customer queries, including refund requests, order status, and product questions.

## Capabilities

- Answer common customer support questions using a knowledge base
- Process refund requests following the company refund policy
- Look up order status via the `order-lookup` skill
- Escalate complex issues to human agents

## Skills Used

| Skill | Purpose |
|---|---|
| `order-lookup` | Retrieve order details by order ID |
| `knowledge-base-search` | Search the product/policy knowledge base |

## Prompts Used

| Prompt | Purpose |
|---|---|
| [`prompts/production/customer-support/handle-refund-v1.md`](../../prompts/production/customer-support/handle-refund-v1.md) | Refund handling |

## Configuration

See [`agent-config.yaml`](agent-config.yaml) for the full configuration.

## How to Deploy

1. Fill in the required variables in `agent-config.yaml` (model, API keys, knowledge base ID).
2. Load the system prompt from `system-prompt.md`.
3. Register the skills listed in the config with your orchestration framework.
4. Run using your LangChain / custom agent runtime.
