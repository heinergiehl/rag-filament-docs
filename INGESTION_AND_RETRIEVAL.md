# Ingestion and Retrieval

This page explains how content enters the system and how the bot finds relevant context later.

## Quick Summary

If the bot feels smart, grounded, and trustworthy, ingestion and retrieval are usually working well. If it feels vague, noisy, or inconsistent, this is one of the first places to inspect.

## Ingestion

Ingestion is the pipeline that prepares source content for retrieval.

### What Happens During Ingestion

For each source, Filament RAG:

1. extracts readable content
2. normalizes the text
3. splits the content into chunks
4. creates embeddings for those chunks
5. stores the document and chunks
6. writes vectors to the configured backend

If all steps succeed, the source moves to `completed`.

The key idea is simple: retrieval quality starts with source quality and chunk quality, not only model choice.

## Retrieval

Retrieval is the runtime step that finds relevant chunks when the user asks a question.

### What Happens During Retrieval

1. the user message is embedded
2. the vector backend finds nearby chunks
3. chunk filtering applies `top_k` and `min_similarity`
4. the selected chunks are formatted as context
5. the chat model answers from that context

This is what makes the chatbot grounded in your docs instead of relying only on the base model.

## Important Retrieval Settings

### `top_k`

Controls how many chunks are retrieved.

- higher = broader context
- lower = tighter context

### `min_similarity`

Controls how strict the relevance filter is.

- higher = only strong matches
- lower = more forgiving retrieval

### Context Budget

Limits how much retrieved text can be sent into the answer prompt.

Too low can remove useful context. Too high can add noise, latency, and cost.

## Recommended Starting Approach

For most first setups:

- start with a smaller source set
- keep retrieval reasonably strict
- test with real user questions
- adjust only one setting at a time

## How To Improve Answer Quality

If answer quality is weak, check these areas in order:

1. source quality — is the content current, structured, and specific?
2. chunk quality — are documents being split into meaningful sections?
3. retrieval tuning — are `top_k` and `min_similarity` too loose or too strict?
4. prompt design — does the bot instruction clearly explain how to answer from sources?
5. model choice — is the selected model good enough for your content and use case?

## Common Failure Patterns

- Good docs, bad answers: retrieval settings are often too loose or too strict.
- Good answers sometimes, bad answers often: source quality or duplication is usually the issue.
- Correct answer but weak citation: source naming or chunk metadata may be weak.
- Source ingests fine but retrieval feels random: too much noisy content may be competing with the important content.

## Why Citations Work

Each chunk keeps metadata such as:

- source name
- section or heading
- page number for PDFs when available
- canonical source link when available

That metadata is used to show citations and deep links in the widget.

## When To Re-Ingest Sources

Re-ingest when:

- the source content changed
- chunking strategy changed
- the embedding model changed
- the vector backend changed
- retrieval quality dropped after config changes

Use the source or bot re-ingest actions inside Filament to rebuild indexed content.

## Queue Behavior

Ingestion normally runs on Laravel queues.

That means a source can stay `pending` for a short time while the worker picks it up or while a retry delay is active.

If it stays pending too long:

- confirm the queue worker is running
- inspect the source error details
- check retry metadata if provider limits were hit
- run `php artisan filament-rag:doctor`

## Practical Tuning Advice

- Start with a smaller, cleaner source set before adding more documents.
- Prefer well-structured source content over dumping large, noisy files.
- Change one retrieval setting at a time so you can observe cause and effect.
- Re-test using the bot's retrieval and answer tools after each meaningful change.

## Best Practice

Treat retrieval tuning as a product-quality task, not a one-time technical checkbox. The right settings depend on your audience, source quality, and the type of questions the bot needs to answer.

## Related Docs

- [RAG Sources](/docs/rag-sources)
- [Operations](/docs/operations)
- [Security and Privacy](/docs/security-and-privacy)
