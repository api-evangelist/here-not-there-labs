---
name: build-towns-bot
description: Scaffold, implement, and deploy a Towns Protocol bot that responds to messages, mentions, and slash commands.
api: Towns Protocol
provider: Here Not There Labs
source: https://docs.towns.com/build/bots/getting-started
packages:
  - towns-bot
  - "@towns-protocol/bot"
operations:
  - onMessage
  - onSlashCommand
  - onReaction
  - onMessageEdit
  - onRedaction
---

# Build a Towns Bot

Create a bot on Towns Protocol that reacts to events in spaces and channels.
All handler names below are real handlers from the `@towns-protocol/bot`
framework (see docs.towns.com/build/bots/events).

## Steps

1. **Register the bot.** In the developer portal at `app.towns.com/developer`,
   create a new bot. Save the two issued credentials: `APP_PRIVATE_DATA` (the
   bot's private key + encryption device) and `JWT_SECRET` (used to verify
   inbound webhook requests from Towns servers). Store both as secrets.

2. **Scaffold the project.** Run `bunx towns-bot init my-bot`, then
   `cd my-bot && bun install`. Requires Node.js v20+ and Bun.

3. **Choose a forwarding mode.** In bot settings pick the message forwarding
   mode: `All Messages` (enables `onTip`, `onChannelJoin`, `onChannelLeave`,
   `onEventRevoke`), the default `Mentions, Commands, Replies & Reactions`, or
   `No Messages` for external-only bots.

4. **Implement handlers.** Register handlers on the bot instance:
   - `onMessage` — read `event.message`, `event.isMentioned`, `event.mentions`,
     `event.threadId`, `event.replyId`.
   - `onSlashCommand` — read `event.command` and `event.args`.
   - `onReaction`, `onMessageEdit`, `onRedaction` — moderation/analytics.
   Use the `BasePayload` fields (`userId`, `spaceId`, `channelId`, `eventId`,
   `createdAt`, `isDm`); remember `spaceId` is `null` in DMs — branch on `isDm`.

5. **Deploy and register the webhook.** Deploy the bot service (the getting
   started guide uses Render.com) and set its public webhook URL in bot
   settings. Verify inbound requests against `JWT_SECRET` before processing.

## Conventions

- Messages are end-to-end encrypted; the framework decrypts using
  `APP_PRIVATE_DATA`. See conventions/here-not-there-labs-conventions.yml.
- Errors surface as Connect/gRPC status codes, not RFC 9457 problem+json.
