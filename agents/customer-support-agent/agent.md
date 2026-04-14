---
id: customer-support-agent
version: "1.0"
status: production
description: Handles customer queries including refunds, order status, and FAQs with empathetic, policy-grounded responses.
tags:
  - customer-support
  - refund
  - order-lookup
  - escalation
config: config.yaml
system_prompt: system-prompt.md
skills:
  - skills/web-search
tools_compatible:
  - claude
  - copilot
  - chatgpt
author: ai-team
created: 2024-01-15
updated: 2024-01-15
---

# Customer Support Agent

> A professional, empathetic AI support specialist that resolves customer queries using available tools and defined policies, escalating to humans when appropriate.

---

## Purpose

Use this agent to handle inbound customer support interactions — refund requests, order status checks, FAQ responses, and general enquiries. The agent follows defined company policies strictly and escalates to a human agent when the issue exceeds its authority or complexity.

---

## Capabilities

- **Refund handling**: Processes refund requests within defined policy limits
- **Order lookup**: Retrieves order details and status via the `order-lookup` skill
- **FAQ resolution**: Answers product and policy questions via `knowledge-base-search`
- **Empathetic responses**: Acknowledges customer feelings before jumping to solutions
- **Escalation**: Hands off to human agents for high-value, complex, or distressed cases
- **Policy enforcement**: Never makes promises or commitments outside stated policy

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `customer_message` | string | yes | The customer's support query |
| `company_name` | string | yes | Company name used in the agent's persona |
| `refund_policy` | string | yes | Full refund policy text or reference |
| `escalation_threshold` | string | yes | Order value above which to escalate (e.g., `$500`) |

---

## Outputs

| Output | Type | Description |
|---|---|---|
| `response` | string | Agent's reply to the customer |
| `action_taken` | string | What the agent did: `resolved`, `escalated`, `information_provided` |
| `escalation_reason` | string | Reason for escalation, if applicable |

---

## Tools / Skills Used

| Skill | Purpose |
|---|---|
| `order-lookup` | Retrieves order details by order ID |
| `knowledge-base-search` | Searches product and policy knowledge base |
| [`web-search`](../../skills/web-search/SKILL.md) | Fallback for general product or policy queries |

> Note: `order-lookup` and `knowledge-base-search` are customer-specific skills not included in this playbook's shared skills library. Define them in your own `skills/` folder following the [SKILL.md template](../../templates/SKILL.md).

---

## Prompt References

| File | Purpose |
|---|---|
| [`system-prompt.md`](system-prompt.md) | Agent persona, responsibilities, policies, tone |
| [`../../prompts/production/customer-support/handle-refund-v2.md`](../../prompts/production/customer-support/handle-refund-v2.md) | Refund-specific prompt |

---

## Configuration

See [`config.yaml`](config.yaml) for model settings, memory, and guardrails.

Key settings:
- **Model**: `gpt-4o` (temperature: `0.3`)
- **Memory**: Buffer, max 20 messages
- **Max turns**: 15
- **Escalation keywords**: `speak to a human`, `manager`, `escalate`

---

## Examples

### Refund request within policy

**Customer:** "I ordered the wrong size and want a refund."

**Agent response:**
> "I'm sorry to hear that! I'd be happy to help with a refund. Could you please share your order number so I can look into this for you?"

See [`examples/`](examples/) for more sample conversations.

---

## Limitations

- Will not make commitments outside the stated refund policy
- Cannot access payment systems directly — refund processing requires integration with your order management system
- Escalates automatically for orders above the configured threshold
- Keeps responses concise (target: under 150 words)

---

## Related

- **Prompts**: [`prompts/production/customer-support/`](../../prompts/production/customer-support/)
- **Eval**: [`eval/test-cases/customer-support-handle-refund-v1-tests.yaml`](../../eval/test-cases/customer-support-handle-refund-v1-tests.yaml)
- **Dataset**: [`eval/datasets/customer-support-refund-dataset.json`](../../eval/datasets/customer-support-refund-dataset.json)
