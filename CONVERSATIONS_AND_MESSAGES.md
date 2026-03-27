# Conversations and Messages

Filament RAG stores chat history in conversations and messages.

## Conversation

A conversation represents one session of interaction for one bot.

It is scoped by:

- bot
- session ID
- context area

## Message

A message is one entry inside a conversation.

Messages can be:

- `user`
- `assistant`

Assistant messages can also store cited sources and rendering metadata.

## Why This Matters

Stored conversations let you:

- review how users interact with a bot
- inspect answer quality
- measure citation coverage
- support export and delete workflows

## Privacy Considerations

Because conversations contain user input, you should define a retention policy and provide deletion/export flows where needed.

Filament RAG includes privacy-oriented endpoints for these workflows.

## Related Docs

- [Core Concepts](/docs/core-concepts)
- [Security and Privacy](/docs/security-and-privacy)
- [Support Policy](/docs/support-policy)
