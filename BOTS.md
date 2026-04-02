# Bots

This is the specific documentation page to share when someone asks:

- what a bot is
- how to create a chatbot in Filament RAG
- what can be customized per bot
- how public and internal bots differ

## Quick Summary

A bot is one complete assistant definition: prompt, model, retrieval behavior, access rules, widget presentation, and linked sources.

If two assistants should answer differently, use different sources, or serve different audiences, they should usually be different bots.

## What A Bot Is

A bot is one assistant configuration inside Filament RAG.

Think of it as the runtime definition for one chatbot experience. A bot tells the system:

- what the assistant is for
- which knowledge sources it can use
- how retrieval should behave
- who can access it
- how the widget should look

This is why one Laravel + Filament app can run multiple assistants with different behavior from one panel.

## What You Can Customize Per Bot

Each bot owns its own:

- name and public ID
- system prompt and response behavior
- provider and model
- retrieval settings
- allowed domains
- context areas
- widget branding and prompts
- linked sources

This separation is one of the biggest strengths of Filament RAG. It lets one app run multiple focused assistants instead of one overloaded bot that tries to do everything.

In practice, this means you can create:

- a public product-help bot
- an admin-only setup assistant
- a customer-facing onboarding bot
- a tenant- or customer-specific knowledge bot

## How To Create A Bot

1. Open **RAG Bots** in your Filament panel.
2. Click **Create**.
3. Enter a clear **name** and stable **public ID**.
4. Write the **system prompt** that defines the bot's job.
5. Select the **provider** and **model**.
6. Configure **retrieval** settings.
7. Configure **allowed domains** and **context areas**.
8. Customize the **widget** title, subtitle, welcome message, and quick prompts.
9. Save the bot.
10. Add one or more [RAG Sources](/docs/rag-sources) and test retrieval.

Creating the bot is only the first step. A bot becomes useful when it has the right sources and retrieval settings behind it.

## Important Bot Fields

### Name

The human-readable label used in Filament and often in the widget header.

Use a name that tells the visitor what the bot is for, such as:

- `Filament RAG Plugin Guide`
- `Customer Onboarding Assistant`
- `Internal Ops Assistant`

### Public ID

The stable identifier used by the widget and public chat endpoints.

This is what your embed snippet references. Keep it predictable and slug-like because it is part of the integration surface.

### System Prompt

The system prompt defines the bot's role, scope, tone, and response rules.

Use it for:

- role definition
- audience targeting
- answer style
- boundaries and fallback behavior
- docs-linking behavior

Do not use it as a substitute for real source content. The prompt guides behavior; the sources provide the grounded knowledge.

Good prompt goals:

- define role and audience clearly
- explain how to behave when sources are weak or missing
- control tone and answer format
- tell the bot when to stay narrow instead of guessing

### Provider And Model

You can choose which provider and model a bot uses for chat.

This lets you optimize different bots for:

- speed
- response quality
- cost
- provider-specific capabilities

### Retrieval Settings

The most important retrieval settings are:

- `top_k`: how many chunks to retrieve
- `min_similarity`: how strict the relevance filter is
- context budget: how much retrieved content can be passed into the answer prompt

These settings strongly influence whether the bot feels too vague, too strict, or well-grounded.

Recommended first defaults:

- start conservative
- test with real user questions
- change one retrieval setting at a time

### Allowed Domains

Allowed domains limit where the widget can be embedded.

Use this when you want a public bot on your marketing site but do not want the widget embedded elsewhere.

### Context Areas

Context areas help separate different assistant experiences such as:

- `public`
- `member`
- `admin`

Use different bots when access scope or audience meaningfully changes.

### Widget Settings

Each bot can have its own:

- title
- subtitle
- welcome message
- quick prompts
- accent color
- style template
- compact mode

This matters because the widget is what the end user actually sees. The title and subtitle should explain why the chatbot exists on that page.

## How Bot Customization Affects The User Experience

### Prompt + Sources

This controls what the bot is supposed to do and what it can answer from.

### Retrieval Settings

This controls whether the bot feels grounded, noisy, or too narrow.

### Context And Access

This controls whether the bot is public, member-only, or admin-only.

### Widget Copy And Branding

This controls whether a visitor instantly understands the purpose of the chatbot.

For example:

- a vague bot title creates confusion
- a clear subtitle explains why the bot is on the page
- strong quick prompts help users ask better questions

## When To Create A Separate Bot

Create a separate bot when you need a different:

- audience, such as public visitors vs internal admins
- source set, such as product docs vs internal runbooks
- tone or prompt behavior
- widget design or onboarding copy
- provider/model combination
- access policy

Do not create separate bots only to change one small answer. Start with the bot prompt and source set first.

## Example Bot Setups

### Public Docs Bot

Good for:

- feature questions
- setup guidance
- docs navigation

Recommended approach:

- `public` context area
- only public docs sources
- clear widget subtitle like `Ask about setup, features, and troubleshooting`

### Member Help Bot

Good for:

- account help
- onboarding flows
- customer-only guidance

Recommended approach:

- `member` context area
- authenticated session context enabled
- customer-facing docs and private help content only

### Admin Ops Bot

Good for:

- runbooks
- incident response notes
- setup and maintenance instructions

Recommended approach:

- `admin` context area
- strict domain and access rules
- internal-only sources

## Typical Bot Patterns

### Public Product Guide

Use for:

- presales questions
- onboarding questions
- public documentation
- feature discovery

### Internal Ops Assistant

Use for:

- admin-only support
- runbooks
- setup help
- maintenance workflows

### Customer-Specific Assistant

Use when you need:

- customer-specific docs
- tenant-specific knowledge
- branded per-customer assistants

## Best Practices

- Keep each bot focused on one job and one audience.
- Use different bots when source quality or permissions differ.
- Write a system prompt that explains exactly what the bot is for.
- Give the widget title and subtitle a clear user-facing purpose.
- Add quick prompts that reflect real user intent.
- Test retrieval after adding or changing sources.

## Related Docs

- [Core Concepts](/docs/core-concepts)
- [RAG Sources](/docs/rag-sources)
- [Ingestion and Retrieval](/docs/ingestion-and-retrieval)
- [Context Areas](/docs/context-areas)
- [Chat Widget](/docs/chat-widget)
