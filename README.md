# RAG Chatbot (Filament Plugin)

Build, manage, and embed production-ready **RAG chatbots** directly from your Filament panel.

> [!IMPORTANT]
> **PHP 8.4+ is required** (this plugin depends on `laravel/ai`, which requires PHP `^8.4`).

## Table of contents

- [What you get](#what-you-get)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Create your first bot](#create-your-first-bot)
- [Embed the widget](#embed-the-widget)
- [Operations](#operations)
- [Security & privacy](#security--privacy)
- [Multi-tenancy](#multi-tenancy)
- [Support & policies](#support--policies)

## What you get

- Filament resources for **Bots**, **Sources**, and **Conversations**
- **Queue-based ingestion** (text, file, URL) with status visibility
- **pgvector** (recommended) or **ChromaDB** vector backends
- **Streaming chat (SSE)** with citations/sources
- **Embeddable widget** (script tag) + optional NPM loader
- Privacy workflows: **export** and **delete** chat history per session
- Operational readiness via `php artisan filament-rag:doctor`

## Requirements

- PHP `8.4+`
- Laravel `12+`
- Filament `5.2+`
- Postgres + `pgvector` (recommended) or ChromaDB (optional)
- An AI provider key compatible with `laravel/ai` (Gemini/OpenAI/Anthropic/xAI)

## Installation

1. Purchase the plugin on Anystack and configure Composer auth (Anystack handles the private package access).
2. Install the package:

```bash
composer require heiner/filament-rag
php artisan vendor:publish --tag=filament-rag-config
php artisan migrate
```

3. Register the plugin in your panel provider:

```php
use Heiner\FilamentRag\FilamentRagPlugin;

// …
->plugins([
    FilamentRagPlugin::make(),
])
```

4. Start your queue worker (ingestion runs on queues):

```bash
php artisan queue:work
```

## Configuration

### 1) Provider credentials

Set the default chat + embedding providers/models via env (override per bot is supported inside Filament).

```env
# Defaults (can be overridden per bot)
RAG_CHAT_PROVIDER=gemini
RAG_CHAT_MODEL=gemini-2.5-flash-lite
RAG_EMBEDDING_PROVIDER=gemini
RAG_EMBEDDING_MODEL=gemini-embedding-001

# Provider keys (examples)
GEMINI_API_KEY=your-key-here
OPENAI_API_KEY=your-key-here
ANTHROPIC_API_KEY=your-key-here
XAI_API_KEY=your-key-here
```

### 2) Vector database (pgvector recommended)

This plugin can use a separate Postgres connection for RAG storage, even if your main app uses MySQL/SQLite.

```env
RAG_VECTOR_BACKEND=pgvector
RAG_DB_CONNECTION=rag_pgsql
RAG_DB_HOST=127.0.0.1
RAG_DB_PORT=5432
RAG_DB_DATABASE=filament_rag
RAG_DB_USERNAME=postgres
RAG_DB_PASSWORD=secret

# Required for pgvector column sizing / indexing decisions
RAG_VECTOR_DIMENSIONS=1536
```

> [!NOTE]
> For higher-dimensional embeddings (> 2000 dims), the plugin skips pgvector ANN index creation to avoid migration failures.

### 3) Widget signing (recommended)

```env
RAG_WIDGET_SIGNING_ENABLED=true
RAG_WIDGET_SIGNING_KEY=your-long-random-secret
RAG_WIDGET_SIGNING_TTL_MINUTES=43200
```

## Create your first bot

1. Open your Filament panel
2. Create a bot in **RAG Bots**
3. Add one or more sources in **RAG Sources**
4. Wait until ingestion is **completed**
5. Test retrieval/chat from the bot page

## Embed the widget

Use the **Embed Snippet** action on the bot edit page to generate the exact script tag for your bot:

```html
<script
  src="https://your-app.com/filament-rag/widget.js"
  data-bot="YOUR_BOT_PUBLIC_ID"
  data-token="SIGNED_WIDGET_TOKEN"
  defer
></script>
```

## Operations

### Readiness check

Run:

```bash
php artisan filament-rag:doctor
```

### Queue and retries

- Ingestion is queue-driven; you must run a worker in production.
- You can isolate ingestion to its own queue via env:

```env
RAG_INGESTION_QUEUE_CONNECTION=database
RAG_INGESTION_QUEUE=rag-ingestion
```

### Rate limiting

Defaults are configurable via env:

```env
RAG_MAX_REQUESTS_PER_MINUTE=40
RAG_MAX_REQUESTS_PER_MINUTE_PER_IP=120
```

## Security & privacy

Built-in controls:

- Domain allowlist per bot
- Optional signed embed tokens (recommended)
- URL ingestion blocks localhost/private networks by default (SSRF protection)
- Export/delete endpoints for session history

Endpoints:

- Export: `GET /api/filament-rag/chat/{botPublicId}/history/export?session_id=...`
- Delete: `DELETE /api/filament-rag/chat/{botPublicId}/history` with JSON `{ "session_id": "..." }`

## Multi-tenancy

Out of the box, this plugin stores all data in shared `rag_*` tables.

If you need tenant isolation, you have two realistic options:

1. **Per-tenant database/schema** for the RAG connection (advanced): set `RAG_DB_*` to a tenant-specific database/schema at runtime.
2. **Tenant-scoped models/tables** (advanced): fork/customize migrations and models to include `tenant_id` and enforce global scopes.

> [!IMPORTANT]
> Tenancy requirements vary wildly between apps; if this is critical for you, validate the approach in a spike before production rollout.

## Support & policies

- Support: `webdevislife2021@gmail.com`
- Release notes: `https://github.com/heinergiehl/rag-filament-docs/blob/main/RELEASE_NOTES_v1.0.0.md`
- Support policy: `https://github.com/heinergiehl/rag-filament-docs/blob/main/SUPPORT_POLICY.md`
- Refund/license terms: `https://github.com/heinergiehl/rag-filament-docs/blob/main/REFUND_AND_LICENSE.md`
- Security & privacy: `https://github.com/heinergiehl/rag-filament-docs/blob/main/SECURITY_AND_PRIVACY.md`
- Data retention: `https://github.com/heinergiehl/rag-filament-docs/blob/main/DATA_RETENTION_POLICY.md`
