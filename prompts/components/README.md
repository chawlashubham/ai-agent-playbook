# prompts/components/

Reusable prompt **fragments** that can be composed into larger prompts. Components are building blocks, not complete prompts on their own.

## Sub-directories

| Folder | Contents |
|---|---|
| [`persona/`](persona/) | Character or role descriptions for the model (e.g., "You are a helpful assistant…") |
| [`format/`](format/) | Output format instructions (e.g., "Respond only in JSON…") |

## When to Use

- Extract repeated sections from your prompts into components.
- Reference a component in the `components` metadata field of a prompt file.
- Mix and match components to build specialised prompts quickly.

## Naming Convention

```
<type>-<description>-v<version>.md
```

Examples:
- `persona-helpful-assistant-v1.md`
- `format-json-output-v1.md`
- `format-bullet-list-v1.md`

## Example Files

| File | Description |
|---|---|
| [`persona/helpful-assistant-v1.md`](persona/helpful-assistant-v1.md) | Standard helpful-assistant persona |
| [`format/json-output-v1.md`](format/json-output-v1.md) | Instruction to respond strictly in JSON |
