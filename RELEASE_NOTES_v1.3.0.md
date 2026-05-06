# Release Notes v1.3.0

This release improves buyer-facing provider flexibility, admin review workflows, and operational feedback loops.

## Added

- OpenRouter chat provider support for hosted model IDs such as Qwen and DeepSeek through OpenRouter.
- Direct DeepSeek chat provider support for `deepseek-chat` and `deepseek-reasoner`.
- Dedicated bot analytics page for usage, source health, and citation coverage.
- Feedback report command for privacy-aware clustering of real chat questions into product and docs themes.
- Conversation review previews that surface latest user questions and recent activity.

## Changed

- Embedding credential resolution now reuses a bot-level chat key only when chat and embedding providers match.
- Docs now clarify database/API ingestion boundaries, IP/private-network usage, offline caveats, and multi-site widget embedding.
- Bot, source, and conversation tables have clearer scan-friendly labels, badges, filters, and actions.

## Notes

OpenRouter and DeepSeek are chat providers. RAG still requires embeddings from an embedding-capable provider such as Gemini or OpenAI unless your project adds and validates a custom/local embedding setup.

Database/API data can be integrated with custom Laravel code by syncing records or API responses into text sources or by registering a custom content extractor. A generic no-code database/API connector is not included in this release.
