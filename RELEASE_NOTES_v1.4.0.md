# Release Notes v1.4.0

Filament RAG 1.4.0 hardens ingestion, retrieval evidence, production diagnostics, and the Filament admin experience for commercial deployments.

## Added

- Renewable widget credentials that exchange a publishable embed key for short-lived, host- and session-bound access tokens.
- A plugin-wide `authorizeUsing(...)` hook that can narrow the existing Filament resource policies.
- Explicit ingestion limits for source bytes, redirects, timeouts, and stale-retry batches.

## Changed

- The Sources table now prioritizes source health, last successful ingestion, and actionable failure notes without allowing long text to distort the table layout.
- The Conversations table keeps long questions scan-friendly with bounded text and full-value tooltips.
- Supported dependency floors move to PHP 8.4, Laravel 12.61.1+, and Filament 5.6.5+ and exclude known-vulnerable HTTP, Markdown, JMESPath, and Symfony versions.
- Production diagnostics now fail when ingestion uses the synchronous queue or private-network URL access is enabled.

## Reliability and Security Fixes

- Replacement ingestion is published as one atomic generation. A failed, cancelled, superseded, or recovered job cannot expose partial knowledge or remove the last successful generation.
- Stale workers are fenced from overwriting newer attempts, and crash recovery removes superseded document and vector generations safely.
- Retrieval citations and serialized evidence now reference only chunks that actually fit into the model context budget.
- Chroma retrieval honors `min_similarity` without returning an unsafe nearest-result fallback.
- URL ingestion revalidates and DNS-pins every redirect and enforces streaming response-size limits.
- Invalid or dangling citation markers are rejected instead of being linked to unrelated sources.

## Upgrade Steps

1. Update the package to `^1.4`.
2. Run the normal additive migrations with `php artisan migrate --force` during deployment. Do not use `migrate:fresh` on an existing installation.
3. Publish Filament assets with `php artisan filament:assets`.
4. Restart long-running queue workers with `php artisan queue:restart`.
5. Run `php artisan filament-rag:doctor` and resolve every `FAIL` before opening traffic.

Existing sources, documents, chunks, bots, and conversations are preserved by the additive migration. Production must use a real queue connection; `sync` remains suitable only for local development and tests.

## Compatibility

- PHP 8.4+
- Laravel 12.61.1+
- Filament 5.6.5+
- Laravel AI 0.1.5+, 0.6.7+, or 0.7.x

