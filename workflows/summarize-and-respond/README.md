# Workflow: Summarize and Respond

Takes an input document, summarises it, then generates a user-facing reply or action recommendation.

## Goal

Transform long input documents into concise, actionable responses without the agent needing to read the full text every turn.

## Steps

1. **summarize** — Call the summarization prompt to reduce the document to key points.
2. **respond** — Use the summary as context to draft a user-facing reply.

## Inputs

| Variable | Type | Description |
|---|---|---|
| `document` | string | The document to summarise and respond to |
| `user_query` | string | The user's question or request about the document |

## Output

A structured reply containing a summary and a direct answer to the user's query.

## Configuration

See [`workflow.yaml`](workflow.yaml).
