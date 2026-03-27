# Core Concepts

This page explains the main building blocks of Filament RAG and links to the detailed docs for each one.

## The Short Version

Filament RAG gives you a Filament-native control plane for running RAG chatbots inside a Laravel app.

The core flow is:

1. Create a **bot**
2. Add **RAG sources**
3. Let the system **ingest** those sources
4. Retrieve relevant **chunks** at question time
5. Answer through the **chat widget** or your own frontend
6. Review **conversations** and operating health inside Filament

## Concept Map

| Concept | What It Means | Learn More |
|---|---|---|
| Bot | A configured assistant with its own prompt, model, retrieval settings, access rules, and widget branding | [Bots](/docs/bots) |
| RAG Source | A piece of knowledge you want the bot to use, such as text, a file, or a URL | [RAG Sources](/docs/rag-sources) |
| Document | The normalized stored version of a source after extraction | [Ingestion and Retrieval](/docs/ingestion-and-retrieval) |
| Chunk | A smaller searchable section of a document used for retrieval and citations | [Ingestion and Retrieval](/docs/ingestion-and-retrieval) |
| Ingestion | The pipeline that extracts, normalizes, chunks, embeds, and stores source content | [Ingestion and Retrieval](/docs/ingestion-and-retrieval) |
| Retrieval | The step where relevant chunks are selected for a user question | [Ingestion and Retrieval](/docs/ingestion-and-retrieval) |
| Widget | The embeddable chat UI for websites or product frontends | [Chat Widget](/docs/chat-widget) |
| Context Area | The access scope for a bot, such as public, member, or admin | [Context Areas](/docs/context-areas) |
| Conversation | A stored chat session for one bot and one session identifier | [Conversations and Messages](/docs/conversations-and-messages) |
| Message | An individual user or assistant entry inside a conversation | [Conversations and Messages](/docs/conversations-and-messages) |

## How The Pieces Fit Together

### Bot

The bot is the central runtime definition. It decides:

- which model and provider answer questions
- which sources belong to it
- how strict retrieval should be
- which users or areas can access it
- how the widget looks and behaves

### Sources, Documents, and Chunks

A source is the input. During ingestion, that source becomes a normalized document, and that document is split into chunks. Those chunks are what retrieval actually searches.

That means the bot does not search entire files or pages at once. It searches smaller grounded pieces of content.

### Retrieval and Answers

When a user asks a question:

1. the system embeds the query
2. finds relevant chunks
3. formats those chunks as context
4. sends that context to the model
5. returns an answer, often with citations

### Conversations and Widget Runtime

The widget is only the interface layer. The bot, sources, retrieval, and conversations live in your Laravel app and are managed from Filament.

## Read These Next

- [Product Overview](/docs/product-overview)
- [Bots](/docs/bots)
- [RAG Sources](/docs/rag-sources)
- [Ingestion and Retrieval](/docs/ingestion-and-retrieval)
- [Chat Widget](/docs/chat-widget)
