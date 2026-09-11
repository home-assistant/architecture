# 23. LLM API scopes

Date: 2026-09-11

## Status

Proposed

## Context

Home Assistant offers tools to an LLM through an LLM API. An integration registers an API, and a consumer selects the APIs it wants. The consumers today are the conversation agents and the MCP server.

There is one general purpose API: `assist`. It offers the entities a user exposed to an assistant, and the intents that control them. Entity exposure is what makes it safe. A user decides what an assistant can see, and the API can do nothing outside that set.

Other useful tools have nowhere to go. Searching the registries, reading the logs and writing an automation act on Home Assistant itself, not on an exposed entity. An entity search that hides unexposed entities cannot help a user configure the device they have not exposed yet. Operations that exist only inside an external ecosystem, such as healing a Z-Wave node or reading a KNX group address, do not fit either.

Putting these tools in `assist` is wrong for two reasons. It gives every voice assistant tools that manage the system. It also grows one API with no rule that says what belongs in it.

## Decision

Home Assistant defines three scopes of LLM API.

### `assist`

The `assist` API controls and reads the entities a user exposed to an assistant. This is the existing API and it does not change. It stays available to any user, and to voice.

### `management`

The `management` API manages Home Assistant itself. It offers tools such as:

- Search the entity, device, area, floor and label registries.
- Read the error log and the system log.
- Read and write automations, scripts and scenes.
- Read the state of integrations and config entries.

The `management` API is not bounded by entity exposure. It searches every entity, because a user asks it about the entity they have not exposed yet. It requires an administrator instead.

### One API per external ecosystem

An integration registers its own API when it brings an external ecosystem into Home Assistant, and that ecosystem has concepts Home Assistant does not model. Z-Wave, KNX, Zigbee and Music Assistant are examples. Such an API requires an administrator.

The test is the ecosystem, not the code owner. Automations, scripts and scenes each live in their own integration, but they are part of Home Assistant itself. Their tools belong in `management`. The same holds for the registries, the logs and config entries.

### Combining APIs

A consumer selects one or more APIs. Home Assistant does not define a combined API for a set that a user wants. A user who wants an external agent to both control the home and manage it selects `assist` and `management` on that consumer.

## Consequences

Entity exposure is no longer the boundary for every LLM API. It bounds `assist` only. For `management` and for an ecosystem API, the administrator requirement takes its place.

A consumer that cannot identify an administrator can offer `assist` only. A voice satellite has no authenticated user, so a voice pipeline cannot use `management`.

An integration decides per API which tools it contributes. An integration that owns part of Home Assistant contributes to `management`. An integration that brings in an ecosystem registers its own API.

A user can build the tool set an external agent receives, by selecting APIs on the MCP server. A later change can let a user turn an API on or off per conversation.

Every selected API adds its prompt and its tools to each request. The number of scopes must stay small, and a scope should offer few tools that search, rather than one tool per operation.

Every new tool now needs an answer to the question of which scope it belongs to. The ecosystem test gives reviewers that answer.
