# Chat Widget

The chat widget is the embeddable UI layer for a bot.

It gives end users a chat interface that connects to your configured bot through your Laravel app. The widget does not contain the knowledge base by itself. It is the delivery layer for the bot, sources, retrieval, and conversation storage already configured in Filament.

## Quick Summary

Use the widget when you want a bot to be available on a website, inside your product, or on an authenticated internal surface without building the chat UI from scratch.

## What You Can Customize

Per bot, you can customize:

- title
- subtitle
- welcome message
- quick prompts
- accent color
- template
- compact mode
- response format

## Choose The Right Embed Style

### Script Tag

Use this for simple website embeds and standard server-rendered applications.

```html
<script
  src="https://your-app.com/filament-rag/widget.js"
  data-bot="YOUR_BOT_PUBLIC_ID"
  data-token="SIGNED_TOKEN"
  defer
></script>
```

### NPM Loader

Use the NPM package when you want tighter control in React, Vue, or another SPA frontend. This is the better choice if you need custom mount points, dynamic routing, or application-driven widget state.

## Which Embed Style Should You Choose?

- Choose the script tag for the fastest path and the lowest integration effort.
- Choose the NPM loader if the host app controls auth, routing, or UI lifecycle in a frontend framework.
- In both cases, generate the bot-specific snippet from Filament so your bot ID and token settings stay correct.

## Recommended Setup Flow

1. Create and validate the bot in Filament first.
2. Use the bot's `Embed Snippet` action instead of hand-building attributes.
3. Add the domain you will embed from to the bot allowlist.
4. Keep signing enabled for production embeds.
5. Test the widget from the exact production host before launch.

## What End Users Actually Experience

The widget experience is shaped by three layers:

- bot prompt and sources determine what it can answer
- retrieval settings determine how grounded and relevant it feels
- widget copy and branding determine whether the user immediately understands its purpose

That means a good widget is not only technically embedded. It is clearly positioned for the user on the page.

## Widget Security

The widget supports:

- signed tokens
- per-bot domain allowlists
- area-based access rules
- rate limiting

That means you can expose a public bot on a website while still keeping internal bots restricted.

## Public vs Internal Widgets

### Public Widget

Best for:

- marketing site questions
- public docs
- onboarding help

Recommended settings:

- `public` context area
- domain allowlist enabled
- signed embeds enabled
- conservative rate limits

### Internal Widget

Best for:

- admin support
- back-office workflows
- authenticated internal knowledge

Recommended settings:

- `member` or `admin` context area
- session auth context enabled
- signed embeds enabled
- tighter domain and guard restrictions

## Common Embed Problems

- Widget does not load: confirm the script URL is reachable from the browser and that assets were published for the deployment.
- Widget says the domain is not allowed: add the exact host to the bot allowlist or use a matching wildcard where supported.
- Widget asks the user to sign in: the bot is using a non-public context area and the current session is not authorized.
- Widget works locally but not in production: verify the production host, token generation, HTTPS, and reverse-proxy headers.

## Best Practices

- Generate embed snippets from the panel instead of copying old ones between environments.
- Use one bot per distinct audience or use case instead of one giant catch-all assistant.
- Keep welcome text and quick prompts specific to the page or flow where the widget appears.
- Re-test widget behavior after changing context areas, domain rules, or signing settings.

## Recommended Launch Checklist

- confirm the bot answers real questions from real sources
- confirm the target host is allowlisted
- confirm signed embed tokens work in the target environment
- confirm public versus authenticated access behaves as expected
- confirm the widget copy explains what the assistant can help with

## Related Docs

- [Bots](/docs/bots)
- [Context Areas](/docs/context-areas)
- [Security and Privacy](/docs/security-and-privacy)
