# Product Overview

Filament RAG adds a production-ready RAG chatbot layer to a Laravel + Filament app.

## What This Plugin Adds

- A Filament-native control plane for managing bots, sources, ingestion, retrieval, and conversations
- An embeddable chat widget you can place on your website or product frontend
- Queue-driven source ingestion for text, files, and public URLs
- Retrieval tuning per bot, including top-k, similarity thresholds, and context limits
- Provider and model configuration per bot
- Operational tooling such as setup checks, source health, testing actions, and analytics

## Why Someone Would Use It

- To add a support, onboarding, product-help, or internal-assistant chatbot to an existing Filament app
- To ship a sellable AI feature faster without building the entire RAG admin layer from scratch
- To manage multiple assistants from one admin panel
- To let non-developers manage bot behavior, sources, and widget branding inside Filament

## What It Does Not Do On Its Own

This plugin does not automatically turn a Filament panel into a complete AI product by itself.

You still need to provide the rest of the product stack that depends on your business:

- Billing
- Tenancy
- Product-specific workflows
- User permissions beyond the plugin's assistant access controls
- Your own domain data and source content

## Core Feature Areas

### Bot Management

- Multiple bots with separate prompts, models, providers, and widget branding
- Public, member, and admin context areas
- Domain allowlists and signed widget embeds

### Knowledge Ingestion

- Text, file, and URL sources
- Queue-based ingestion and retry handling
- Chunking, embedding, and vector persistence
- Re-ingest and rebuild tooling when settings change

### Retrieval and Answers

- Configurable retrieval depth and similarity
- Source-backed chat responses with citations
- Streaming responses for the widget experience
- Markdown or plain-text answer formats

### Widget and Embeds

- One-script embed for websites
- NPM loader for SPA frameworks
- Style templates, accent colors, titles, subtitles, quick prompts, and welcome text

### Operations and Security

- Setup diagnostics through `php artisan filament-rag:doctor`
- Queue and ingestion visibility in the panel
- Domain restrictions, signing, and rate limiting
- Privacy endpoints for export and deletion workflows

## Best Starting Points

- Read [Core Concepts](/docs/core-concepts) for the product model and terminology
- Read [Bots](/docs/bots) for assistant configuration
- Read [RAG Sources](/docs/rag-sources) for source types and creation flow
- Read [Ingestion and Retrieval](/docs/ingestion-and-retrieval) for how grounding works
- Read [Chat Widget](/docs/chat-widget) for embedding and UX
- Read [Quickstart](/docs/quickstart) for installation and first setup
