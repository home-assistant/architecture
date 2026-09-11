# 23. LLM API scopes

Date: 2026-09-11

## Status

Proposed

## Context

Home Assistant offers tools to an LLM through an LLM API. An integration registers an API, and a consumer selects the APIs it wants. The consumers today are the conversation agents and the MCP server.

There is one general purpose API: `assist`. It offers the entities a user exposed to an assistant, and the intents that control them. Its scope is the exposed entity.

Other useful tools have nowhere to go. Searching the registries, reading the logs and writing an automation act on Home Assistant itself, not on an exposed entity. An entity search that hides unexposed entities cannot help a user configure the device they have not exposed yet. Operations that exist only inside an external ecosystem, such as healing a Z-Wave node or reading a KNX group address, do not fit either.

Putting these tools in `assist` is wrong for two reasons. It gives every assistant, voice included, tools that have nothing to do with controlling the home. It also grows one API with no rule that says what belongs in it.

## Decision

Home Assistant defines three scopes of LLM API.

### `assist`

The `assist` API controls and reads the entities a user exposed to an assistant. This is the existing API and it does not change.

### `management`

The `management` API manages Home Assistant itself. It offers tools such as:

- Search the entity, device, area, floor and label registries.
- Read the error log and the system log.
- Read and write automations, scripts and scenes.
- Read the state of integrations and config entries.

The `management` API is not bounded by entity exposure. It searches every entity, because a user asks it about the entity they have not exposed yet.

### One API per external ecosystem

An integration registers its own API when it brings an external ecosystem into Home Assistant, and that ecosystem has concepts Home Assistant does not model. Z-Wave, KNX, Zigbee and Music Assistant are examples.

## Consequences

Tools that manage Home Assistant have a place to live and to grow. A tool that does not act on an exposed entity no longer has to be forced into `assist` or left out.

An integration that brings in an external ecosystem may register its own LLM API. That is now an accepted pattern rather than an exception.
