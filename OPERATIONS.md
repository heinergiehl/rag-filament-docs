# Operations Guide

Use this page when you are moving from a local proof of concept to something stable enough for real users.

## Quick Summary

Production readiness mostly comes down to four things: healthy ingestion, stable queues, secure widget delivery, and confidence that the bot answers from the right content.

## Daily Operating Checklist

For a healthy deployment, keep these basics in place:

- a running queue worker
- passing health checks
- a configured vector backend
- signed widget tokens in production
- allowed domains set per bot
- a repeatable way to retry failed ingestion jobs

If these basics are in place, most production issues become faster to diagnose and safer to recover from.

## Queue Worker

Ingestion jobs run on Laravel queues.

```bash
php artisan queue:work
```

If using a dedicated ingestion queue:

```bash
php artisan queue:work database --queue=rag-ingestion
```

`pending` sources are expected while waiting for retries after provider rate limits. If pending items do not move for several minutes, verify worker health, queue configuration, and provider quotas.

## Dev Bootstrap

For local onboarding, run:

```bash
php artisan filament-rag:dev-bootstrap
```

Useful flags:

- `--no-docker` if services already run
- `--services=pgsql,chroma` to start both services for backend switching tests
- `--no-doctor` to skip readiness checks

## Health Check

Run the built-in readiness command:

```bash
php artisan filament-rag:doctor
```

Treat `FAIL` as a release blocker.

## Signed Widget Tokens

If enabled, all widget API requests require a valid signed token.

```env
RAG_WIDGET_SIGNING_ENABLED=true
RAG_WIDGET_SIGNING_KEY=your-long-random-secret
```

Keep signing enabled in production unless you have a very specific reason not to.

## Go-Live Baseline

Before production launch:

- use `RAG_VECTOR_BACKEND=pgvector` unless you explicitly need another backend
- if the app DB is MySQL, set `RAG_DB_CONNECTION=rag_pgsql` and configure `RAG_DB_*` PostgreSQL env vars
- supervise the queue worker with systemd, Supervisor, or Horizon
- set `RAG_WIDGET_SIGNING_ENABLED=true` with a strong signing key
- configure a domain allowlist per bot
- run at least one successful load test against a production-like environment
- confirm privacy export and deletion flows work as expected

## Load Test Baseline

Use a tool like k6 against chat endpoints. Start with:

- 10 virtual users
- 2–5 minutes duration
- realistic message payload size

Track:

- response latency (`p50` and `p95`)
- provider rate-limit responses
- queue backlog during ingestion
- error rate by endpoint and bot

## Recovery Playbook

- If ingestion fails: inspect `rag_sources.meta.error`, then retry ingestion from the Sources table.
- If ingestion is pending: inspect `rag_sources.meta.retry_after` and `rag_sources.meta.retry_delay_seconds`.
- If you changed vector backend or model settings: use `Re-Ingest Bot Sources` on the bot page or `Re-Ingest All Sources` from the sources list.
- If chat is rate-limited: reduce traffic burst and add retry backoff in clients.
- If retrieval quality drops: tune `top_k`, `min_similarity`, and source quality before changing many settings at once.

## Best Practices

- Keep one environment for clean-room installation testing.
- Treat `doctor` output and ingestion failures as operational signals, not one-off annoyances.
- Re-ingest after meaningful model, backend, or chunking changes.
- Document which bots are public and which require authenticated access.

## Recommended Production Mindset

Treat bots like real product surfaces. That means you should monitor quality, access, and operational health over time instead of only verifying that the widget technically loads.
