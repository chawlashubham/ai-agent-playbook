# Best Practices

Guidelines for writing effective, maintainable, and reusable prompts and agent configurations.

---

## 1. Writing Effective Prompts

### Be specific about the role
Give the model a clear persona and context before stating the task.

```
✅ You are a customer support specialist for Acme Corp. Your goal is to resolve refund requests in accordance with the policy below.

❌ You are a helpful assistant. Help with refund requests.
```

### Use structured instructions
Number your instructions. The model follows ordered lists more reliably than prose paragraphs.

```
✅ 1. Acknowledge the customer's concern.
   2. Gather the order number if not provided.
   3. Check eligibility against the policy.

❌ First acknowledge the customer, then ask for their order number if you need it, and check the policy.
```

### Define the output format explicitly
Never leave output format to chance — define it with an example or schema.

```
✅ Respond in this exact JSON format:
   {"decision": "approved|denied", "reason": "...", "next_steps": "..."}

❌ Provide a decision and explain your reasoning.
```

### Use template variables for dynamic content
Replace hardcoded values with `{{variable_name}}` placeholders.

```
✅ The refund policy is: {{refund_policy}}
❌ The refund policy is: Items must be returned within 30 days...
```

---

## 2. Prompt Composition

### Prefer components over duplication
If the same persona or format instruction appears in multiple prompts, extract it into `prompts/components/` and reference it in metadata.

### Keep prompts focused
One prompt = one intent. If a prompt tries to do two things (e.g., "classify AND respond"), split it into two prompts and chain them in a workflow.

### Limit prompt length
- System prompts: aim for < 800 tokens.
- Instruction blocks: be concise; the model does not need exhaustive rules for every edge case.

---

## 3. Versioning and Lifecycle

- Start all new prompts with `status: experimental`.
- Write test cases in `eval/test-cases/` before promoting to production.
- Never overwrite an existing production prompt — create a new version.
- Mark old versions `status: deprecated` and add a `superseded_by` field.

---

## 4. Testing Prompts

- Write at least **3 test cases** per prompt: a happy path, an edge case, and an adversarial input.
- Use `eval/test-cases/` for structured pass/fail tests.
- Use `eval/benchmarks/` when comparing two prompt variants.
- Document test results in experiments if the evaluation is non-trivial.

---

## 5. Security and Safety

- **Never hardcode secrets, API keys, or PII** in prompt files. Use `{{variable_name}}` and inject at runtime.
- **Restrict model capabilities** to the minimum needed — use guardrails (`max_turns`, `content_filter`).
- **Validate model output** before passing it to downstream systems — especially for JSON output and code execution.
- **Sanitize user inputs** before injecting them into prompt templates to prevent prompt injection attacks.
- Treat experimental prompts as untrusted until test cases pass.

---

## 6. Model Compatibility

- Test prompts on all models listed in `model_compatibility` before publishing.
- Note model-specific quirks in the prompt's `Notes` section.
- Be aware that prompts tuned for GPT-4 may need adjustment for local models (Ollama/Llama3).

---

## 7. Documentation

- Every new prompt, agent, skill, and workflow **must** have:
  - A complete metadata header
  - At least one usage example
  - A note on known limitations
- Keep README files up to date when adding new files to a folder.
- Log all changes in [`changelog/CHANGELOG.md`](../changelog/CHANGELOG.md).

---

## 8. Code Review Checklist

Before merging a new prompt or config:

- [ ] Metadata header is complete and correct
- [ ] File name follows naming conventions
- [ ] Template variables are documented in an input table
- [ ] At least one example interaction is included
- [ ] Test cases exist in `eval/test-cases/`
- [ ] Changelog entry added
- [ ] No hardcoded secrets or PII
