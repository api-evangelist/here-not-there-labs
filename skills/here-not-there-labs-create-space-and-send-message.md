---
name: create-space-and-send-message
description: Use the Towns React SDK to connect a wallet, create or join a space, and send messages in a channel.
api: Towns Protocol
provider: Here Not There Labs
source: https://docs.towns.com/build/react-sdk/getting-started
packages:
  - "@towns-protocol/react-sdk"
  - "@towns-protocol/sdk"
operations:
  - TownsSyncProvider
  - signAndConnect
  - useCreateSpace
  - useJoinSpace
  - useSendMessage
  - useTimeline
  - useScrollback
  - useReactions
---

# Create a Space and Send a Message

Build a client that talks to Towns Protocol with the React SDK. Every hook named
below is a real hook from `@towns-protocol/react-sdk` (enumerated in
docs.towns.com/llms.txt).

## Steps

1. **Wrap the app.** Mount `TownsSyncProvider` at the root so hooks share one
   sync agent.

2. **Authenticate.** Use `signAndConnect` (or `connectTowns` /
   `connectTownsWithBearerToken`) to establish a session by signing with the
   user's Ethereum wallet (Sign-In with Ethereum). See
   authentication/here-not-there-labs-authentication.yml.

3. **Create or join a space.** Call `useCreateSpace` to mint a new space
   (on-chain, ERC-721 ownership) or `useJoinSpace` to join an existing one.
   Manage channels with `useCreateChannel` / `useChannel`.

4. **Send a message.** Use `useSendMessage` to post into a channel, DM
   (`useCreateDm`), or group DM (`useCreateGdm`). React with `useSendReaction`.

5. **Render and page history.** Use `useTimeline` for the live message list and
   `useScrollback` to page older events (Towns pages by walking miniblocks
   backward, not by offset/cursor). Use `useReactions`, `useThreads`,
   `useMemberList` for richer UI.

## Conventions

- Real-time updates arrive over server-streaming sync (SyncStreams). Do not poll.
- Messages are end-to-end encrypted. See
  conventions/here-not-there-labs-conventions.yml.
