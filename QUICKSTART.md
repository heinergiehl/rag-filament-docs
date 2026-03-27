# Quick Start

This guide is optimized for first-time setup and clean-room testing.

## Read This First If You Are New

If you are still learning the product model, start with:

- [Core Concepts](/docs/core-concepts)
- [Bots](/docs/bots)
- [RAG Sources](/docs/rag-sources)
- [Ingestion and Retrieval](/docs/ingestion-and-retrieval)
- [Chat Widget](/docs/chat-widget)

## 1. What You Are Installing

Filament RAG adds a managed RAG chatbot layer to a Laravel + Filament app:

- Filament resources for bots, sources, and conversations
- Retrieval and provider controls per bot
- An embeddable widget for your app or external frontend
- Operational checks, analytics, and privacy endpoints

It helps you ship AI assistants inside your product faster. It does not replace your core app logic, billing, tenancy, or other product-specific workflows.

## 2. Prerequisites

- PHP 8.4+
- Laravel 12+
- PostgreSQL with `pgvector` (default/recommended), or ChromaDB as an optional backend
- A provider API key (`GEMINI_API_KEY`, `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, or `XAI_API_KEY`)

## 3. Choose Your Start Path

### Path A: Existing Filament App

Install package:

```bash
composer require heiner/filament-rag
php artisan vendor:publish --tag=filament-rag-config
```

Register plugin in your panel provider:

```php
use Heiner\FilamentRag\FilamentRagPlugin;

->plugins([
    FilamentRagPlugin::make(),
])
```

### Path B: Brand-New Laravel App (from scratch)

```bash
composer create-project laravel/laravel my-app
cd my-app
composer require filament/filament "^5.2"
php artisan filament:install --panels --no-interaction
composer require heiner/filament-rag
php artisan vendor:publish --tag=filament-rag-config
```

Then add `FilamentRagPlugin::make()` in `app/Providers/Filament/AdminPanelProvider.php`.

### If Package Is On GitHub (Not Packagist)

In the host app:

```bash
composer config repositories.filament-rag vcs https://github.com/heinergiehl/rag-filament.git
composer require heiner/filament-rag:dev-main
```

For private repos, configure Composer auth first:

```bash
composer config --global --auth github-oauth.github.com YOUR_GITHUB_TOKEN
```

## 4. Configure `.env`

Minimum baseline:

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
RAG_MAX_REQUESTS_PER_MINUTE=40
RAG_MAX_REQUESTS_PER_MINUTE_PER_IP=120
RAG_CONTEXT_DEFAULT_AREA=public
RAG_CONTEXT_ALLOWED_AREAS=public,member,admin
RAG_WIDGET_SIGNING_ENABLED=true
RAG_WIDGET_SIGNING_KEY=replace-with-a-long-random-secret
GEMINI_API_KEY=
```

Optional Chroma profile:

```env
RAG_VECTOR_BACKEND=chroma
RAG_CHROMA_URL=http://127.0.0.1:8001
RAG_CHROMA_TENANT=default_tenant
RAG_CHROMA_DATABASE=default_database
RAG_CHROMA_COLLECTION=filament-rag
```

Filament RAG auto-detects Chroma API (`/api/v2` first, then `/api/v1` fallback for older servers). Set `RAG_CHROMA_URL` to the server root URL (for example `http://127.0.0.1:8001`), not `/api/v2`.

Production profile (recommended):

```env
RAG_VECTOR_BACKEND=pgvector
RAG_DB_CONNECTION=rag_pgsql
RAG_DB_HOST=127.0.0.1
RAG_DB_PORT=5432
RAG_DB_DATABASE=filament_rag
RAG_DB_USERNAME=postgres
RAG_DB_PASSWORD=secret
```

If your main app DB is MySQL, use dedicated PostgreSQL for RAG:

```env
RAG_DB_CONNECTION=rag_pgsql
RAG_DB_HOST=127.0.0.1
RAG_DB_PORT=5432
RAG_DB_DATABASE=filament_rag
RAG_DB_USERNAME=postgres
RAG_DB_PASSWORD=secret
```

## 5. Run Migrations + Queue Worker

```bash
php artisan migrate
php artisan queue:work
```

Optional (recommended for deployments):

```bash
php artisan filament:assets
```

Or run one local bootstrap command:

```bash
php artisan filament-rag:dev-bootstrap
```

## 6. Validate Setup Immediately

```bash
php artisan filament-rag:doctor
```

Treat `FAIL` as blocking.

## 7. Create Your First Bot

1. Open Filament admin.
2. Create a bot in `RAG Bots`.
3. Add a source in `RAG Sources` (text/file/url).
4. Wait until source status is `completed`.
5. Use `Test Retrieval` and `Test Bot Answer` on bot edit page.
6. Use `Setup Check` actions on bot/source pages if anything is blocked.

## 8. Embed Widget

Use the `Embed Snippet` action on bot edit page.

Example:

```html
<script src="https://your-app.com/filament-rag/widget.js" data-bot="YOUR_BOT_PUBLIC_ID" defer></script>
```

If signing is enabled, include `data-token` from generated snippet.

## 9. From-Scratch Test Checklist

Use this before publishing:

1. New Laravel app installs package without manual hacks.
2. `php artisan migrate` succeeds.
3. `php artisan filament-rag:doctor` has no `FAIL`.
4. A source ingests to `completed`.
5. Widget answers a test prompt.

### One-Command Smoke Script (PowerShell)

Run from repository root:

```powershell
powershell -ExecutionPolicy Bypass -File scripts/smoke/smoke-install.ps1 `
  -DbHost 127.0.0.1 `
  -DbPort 5435 `
  -DbUsername sail `
  -DbPassword password `
  -GeminiApiKey "<your-key>" `
  -RunIngestionProbe
```

Install from GitHub VCS instead of local path:

```powershell
powershell -ExecutionPolicy Bypass -File scripts/smoke/smoke-install.ps1 `
  -InstallMode vcs `
  -RepositoryUrl "https://github.com/heinergiehl/rag-filament.git" `
  -PackageConstraint "dev-main" `
  -GitHubToken "<optional-token-for-private-repo>" `
  -DbHost 127.0.0.1 `
  -DbPort 5432 `
  -DbUsername postgres `
  -DbPassword secret
```

## Common First-Run Issues

- `Source pending`: queue worker not running.
- `This domain is not allowed`: missing bot `allowed_domains`, or host mismatch. Use host entries (`app.example.com`, `*.example.com`). `localhost:8000` and full URLs are accepted and normalized to host.
- `Please sign in to access this chat area`: area is non-public and current user/guard is not authorized, or session auth context is disabled. Keep `RAG_API_INCLUDE_SESSION_AUTH_CONTEXT=true` for `member/admin` areas.
- `Failed to clone the git@github.com:...` during `composer require`: GitHub VCS fallback hit SSH. For private repos, add a GitHub token (`composer config --global --auth github-oauth.github.com ...`) or pass `-GitHubToken` in the smoke script.
- `Could not reach chroma ... /api/v2/heartbeat`: start Chroma and verify `RAG_CHROMA_URL`.
- `The route api/v2/heartbeat could not be found`: `RAG_CHROMA_URL` points to Laravel/app HTTP, not Chroma.
- `vector_backend_not_implemented`: backend set to Pinecone (scaffold only right now).
- `column cannot have more than 2000 dimensions for ivfflat index`: high-dimensional embedding + ANN index limit. Installation now skips ANN index in this case; retrieval still works.

## 10. VPS Demo Deployment (Optional)

If you want a public demo app on VPS (separate from local sandbox), use:

- Workflow: `.github/workflows/deploy-demo.yml`
- Runbook: `docs/DEMO_DEPLOYMENT.md`

This deploys a dedicated Laravel demo app and seeds public/admin demo bots automatically.
