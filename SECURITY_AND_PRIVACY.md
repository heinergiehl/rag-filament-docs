# Security and Privacy Notes

Use this page to understand what data is involved, which controls the plugin provides, and what you should still handle at the application level.

## Data Collected

- Conversation messages (`user`, `assistant`)
- Retrieval source metadata attached to assistant messages
- Session identifier used for conversation continuity

Depending on your implementation, your application may also associate chat sessions with authenticated users or tenants.

## Controls In This Plugin

- Per-bot domain allowlist checks
- Optional signed embed tokens (`RAG_WIDGET_SIGNING_ENABLED=true`)
- Request throttling by bot, session, and IP
- Friendly provider errors without stacktrace leaks in widget output
- URL ingestion safety checks that block localhost and private networks by default

## Recommended Privacy Policy Clauses

Your privacy policy should explain:

- what user text is stored and for how long
- which AI providers process prompts
- whether chats are used for support, analytics, or quality review
- how users can request export or deletion of chat history
- how to contact support for privacy requests

## Compliance Endpoints

- `GET /api/filament-rag/chat/{botPublicId}/history/export?session_id=...`
- `DELETE /api/filament-rag/chat/{botPublicId}/history` with JSON body `{ "session_id": "..." }`

These endpoints help you support privacy workflows without building the storage plumbing from scratch.

## Hardening Recommendations

- Use HTTPS only in production
- Place WAF or strict rate limits at the edge
- Set a retention schedule for old conversations
- Monitor AI provider quotas and abuse spikes
- Keep widget signing enabled for public embeds
- Review which bots are public versus authenticated

## Best Practices

- Separate public and internal bots instead of sharing one bot across very different audiences.
- Keep the minimum retention window that still meets your support and product needs.
- Document which providers process prompts so legal and support teams can answer questions clearly.
- Test export and deletion flows from the same client context your users will have in production.
