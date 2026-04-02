# Quickstart

This guide is current for the Filament RAG 1.2.x release line. It is written for buyers who want to install the plugin, create one working bot, ingest real content, and embed the chat widget without digging through internal project notes.

## 1. Requirements

- PHP 8.4+
- Laravel 12+
- Filament 5.2+
- One provider API key such as `GEMINI_API_KEY`, `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, or `XAI_API_KEY`
- PostgreSQL with `pgvector` for the recommended setup, or Chroma as an optional backend

## 2. Install the Package

If your purchase includes private Composer repository credentials or a private VCS URL, configure that first. Then install the package in your Laravel app:

```bash
composer require heiner/filament-rag
php artisan vendor:publish --tag=filament-rag-config
```

If you are starting from a fresh Laravel app, install Filament before adding Filament RAG:

```bash
composer create-project laravel/laravel my-app
cd my-app
composer require filament/filament "^5.2"
php artisan filament:install --panels --no-interaction
composer require heiner/filament-rag
php artisan vendor:publish --tag=filament-rag-config
```

## 3. Register the Plugin

Add the plugin to your Filament panel provider:

```php
use Heiner\FilamentRag\FilamentRagPlugin;

->plugins([
    FilamentRagPlugin::make(),
])
```

## 4. Configure `.env`

Minimal `pgvector` setup:

```env
RAG_VECTOR_BACKEND=pgvector
RAG_DB_CONNECTION=rag_pgsql
RAG_DB_HOST=127.0.0.1
RAG_DB_PORT=5432
RAG_DB_DATABASE=filament_rag
RAG_DB_USERNAME=postgres
RAG_DB_PASSWORD=secret
RAG_CHAT_PROVIDER=gemini
RAG_CHAT_MODEL=gemini-2.5-flash-lite
RAG_EMBEDDING_PROVIDER=gemini
RAG_EMBEDDING_MODEL=gemini-embedding-001
RAG_VECTOR_DIMENSIONS=1536
RAG_CONTEXT_DEFAULT_AREA=public
RAG_CONTEXT_ALLOWED_AREAS=public,member,admin
RAG_WIDGET_SIGNING_ENABLED=true
RAG_WIDGET_SIGNING_KEY=replace-with-a-long-random-secret
GEMINI_API_KEY=your-key-here
```

If your main app database is MySQL or SQLite, that is fine. Point Filament RAG at a separate PostgreSQL connection for vector storage.

Optional Chroma setup:

```env
RAG_VECTOR_BACKEND=chroma
RAG_CHROMA_URL=http://127.0.0.1:8001
RAG_CHROMA_TENANT=default_tenant
RAG_CHROMA_DATABASE=default_database
RAG_CHROMA_COLLECTION=filament-rag
```

Set `RAG_CHROMA_URL` to the server root URL, for example `http://127.0.0.1:8001`, not `/api/v2`.

## 5. Run Migrations and Start a Worker

```bash
php artisan migrate
php artisan queue:work
```

For deployments, also publish Filament assets:

```bash
php artisan filament:assets
```

## 6. Verify the Installation

Run the built-in setup check:

```bash
php artisan filament-rag:doctor
```

Fix anything marked `FAIL` before you start ingesting real content.

## 7. Create Your First Bot

1. Open your Filament panel.
2. Go to `RAG Bots` and click `Create`.
3. Enter a clear bot name and a stable `public_id`.
4. Choose the provider and model.
5. Configure the widget title, subtitle, welcome message, and quick prompts.
6. Set the correct context area and allowed domains.
7. Save the bot.

## 8. Add Your First Source

1. Open `RAG Sources`.
2. Click `Create`.
3. Select your bot.
4. Choose the source type:
   - `Text` for short FAQs or policy content
   - `File` for Markdown, text, HTML, JSON, CSV, or text-based PDFs
   - `URL` for public docs pages or help articles
5. Save the source and wait until the status changes to `completed`.

Once ingestion finishes, the bot can retrieve grounded context from that source.

## 9. Test the Bot in Filament

On the bot edit page, use the built-in testing actions:

- `Test Retrieval` to check whether the right chunks are being found
- `Test Bot Answer` to validate the final response

If the answer is weak, improve the source quality first, then tune retrieval settings such as `top_k` and `min_similarity`.

## 10. Embed the Widget

Use the `Embed Snippet` action on the bot edit page to generate the exact script tag for that bot.

Example:

```html
<script
  src="https://your-app.com/filament-rag/widget.js"
  data-bot="YOUR_BOT_PUBLIC_ID"
  data-token="SIGNED_TOKEN"
  defer
></script>
```

If widget signing is enabled, keep `data-token` in place. If you embed the widget on multiple domains, make sure each host is allowed on the bot.

## Common Setup Issues

- `Source pending`: the queue worker is not running, stalled, or cannot reach the provider.
- `This domain is not allowed`: add the correct host to the bot's `allowed_domains`.
- `Please sign in to access this chat area`: the bot is using a non-public area and the current user is not authorized for it.
- `Could not reach chroma ... /api/v2/heartbeat`: Chroma is not running or `RAG_CHROMA_URL` is wrong.
- Provider authentication errors: verify the API key in `.env` or the per-bot credentials in Filament.

## Read Next

- [Product Overview](/docs/product-overview)
- [Bots](/docs/bots)
- [RAG Sources](/docs/rag-sources)
- [Ingestion and Retrieval](/docs/ingestion-and-retrieval)
- [Chat Widget](/docs/chat-widget)
