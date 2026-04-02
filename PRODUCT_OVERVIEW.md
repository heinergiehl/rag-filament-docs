# Product Overview

Filament RAG adds a production-ready RAG chatbot layer to a Laravel + Filament app.

It is designed for teams that want to ship AI assistants without building the full admin and ingestion stack from scratch.

## Best Fit

Filament RAG is a strong fit if you want to:

- add a support or docs bot to an existing Filament product
- launch internal knowledge assistants for staff or admin users
- manage multiple bots with separate sources, prompts, and widget branding
- give non-developers a Filament UI for managing chatbot behavior and content

## Probably Not A Fit

It is likely not the right tool if you need:

- a hosted chatbot SaaS with no Laravel or Filament app behind it
- a full billing, tenancy, and customer portal product out of the box
- a generic AI wrapper with no grounding in your own content

## In One Sentence

You manage bots, sources, retrieval settings, widget behavior, and conversation history from Filament, while your Laravel app handles runtime, security, and integration.

## What This Plugin Adds

- A Filament-native control plane for bots, sources, ingestion, retrieval, and conversations
- An embeddable chat widget for websites, apps, and external frontends
- Queue-driven ingestion for text, files, and public URLs
- Per-bot provider, model, and retrieval settings
- Operational tooling such as setup checks, retries, testing actions, and analytics-friendly workflows

## Why Teams Use It

- Add a support, onboarding, or product-help assistant to an existing Filament app
- Launch an internal copilot or staff-facing knowledge assistant
- Run multiple bots with separate prompts, models, sources, and branding
- Let non-developers manage bot behavior and knowledge from inside Filament

## What It Does Not Replace

This plugin gives you the chatbot control plane. It does not replace the rest of your application stack.

You still own:

- billing and subscriptions
- tenancy or account isolation
- product-specific workflows
- advanced app permissions beyond the plugin's assistant access controls
- your own source content, policies, and business logic

## Why Not Build It Yourself?

You can absolutely build a chatbot stack by hand, but the plugin removes a lot of repeated product work:

- admin CRUD for bots, sources, and conversations
- source ingestion workflows and retries
- retrieval tuning UI and testing actions
- widget embedding, branding, and access controls
- operational checks and privacy workflows

That lets you focus on your product, data, and user experience instead of rebuilding the management layer.

## Core Feature Areas

### Bot Management

- Multiple bots with separate prompts, models, providers, branding, and answer settings
- Public, member, and admin context areas
- Domain allowlists and signed widget embeds

### Knowledge Ingestion

- Text, file, and URL sources
- Queue-based ingestion and retry handling
- Chunking, embedding, and vector persistence
- Re-ingest and rebuild tooling when models or settings change

### Retrieval and Answers

- Configurable retrieval depth and similarity filtering
- Source-backed answers with citations
- Streaming responses for widget UX
- Markdown or plain-text answer formats

### Widget And Frontend Delivery

- One-script embed for websites
- NPM loader for SPA frameworks
- Titles, subtitles, prompts, welcome text, templates, and accent colors

### Operations And Security

- Setup diagnostics through `php artisan filament-rag:doctor`
- Queue and ingestion visibility in the panel
- Rate limiting, domain restrictions, and signed tokens
- Privacy export and deletion endpoints

## Recommended Reading Order

1. [Core Concepts](/docs/core-concepts) to understand the product model
2. [Quickstart](/docs/quickstart) to install and validate a clean setup
3. [Bots](/docs/bots) to configure assistant behavior
4. [RAG Sources](/docs/rag-sources) to add knowledge
5. [Ingestion and Retrieval](/docs/ingestion-and-retrieval) to tune answer quality
6. [Chat Widget](/docs/chat-widget) to ship the frontend experience
