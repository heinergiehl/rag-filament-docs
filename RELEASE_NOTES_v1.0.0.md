# Release Notes - v1.0.0 (2026-02-24)

## Summary

`v1.0.0` is the first commercial-ready release of Filament RAG.
It focuses on production stability, strict quality gates, and marketplace packaging for paid distribution.

## Highlights

- Filament-native management for bots, sources, and conversations.
- Embeddable widget runtime with configurable branding/templates.
- Queue-based ingestion and retrieval flow with safer source re-indexing behavior.
- Privacy workflows for conversation export/delete and domain allowlist enforcement.

## Quality & Reliability

- CI now enforces linting, tests, and static analysis on supported PHP `8.4`.
- PHPStan and Pint are configured for deterministic package-level checks.
- Full test suite is green (`62 tests`, `164 assertions`).
- Orphaned Filament page code was removed to reduce maintenance and routing drift risk.

## Backward Compatibility Notes

- No intentional breaking API or config changes were introduced in this final `v1.0.0` cut.
- Existing integrations using documented env keys and plugin registration remain valid.
- If you consumed a pre-release or earlier draft tag, retest install/upgrade paths against `v1.0.0`.

## Distribution Notes

- This package is prepared for paid listing on Filament Plugins via `anystack_id`.
- The source repository can remain private; public documentation/legal/support links are still required for listing.
