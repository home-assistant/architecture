# LLM API scopes: keep `assist` for control, add `management` for Home Assistant itself

## Summary

Today Home Assistant has one general purpose LLM API: `assist`. It exposes exposed entities and the intents that control them. Everything an LLM can do for a user must fit in that one API, or live outside the LLM API system.

This proposes three tiers of LLM API:

| API | Scope | Audience |
| --- | --- | --- |
| `assist` | Control and read exposed entities. Unchanged. | Any user, voice included. |
| `management` | Manage Home Assistant itself. Search the registries, read logs, create automations. | Administrators. |
| `<integration>` | One integration's own domain. For example `knx` or `modbus`. | Administrators. |

A user then selects one or more APIs per consumer. The `mcp_server` integration already does this, so a user can build the tool set an external agent gets. A later chat UI change can let a user turn an API on or off per conversation, the way Claude and ChatGPT do.

## What already exists

Most of the machinery is in place. This is worth stating, because the proposal is mostly a naming and policy decision, not new plumbing.

- `homeassistant/helpers/llm.py` holds the registry. `async_register_api`, `async_get_api`, `API`, `APIInstance` and `Tool` are already generic over multiple APIs.
- The `llm` integration owns the `AssistAPI` and an integration platform. An integration implements `async_get_tools(hass, llm_context, api_id)` in its `llm.py` and returns tools for the API it is asked about.
- Fifteen integrations already ship that platform. Each one starts with `if api_id != LLM_API_ASSIST: return None`. A second API is the case this signature was written for.
- `MergedAPI` already merges several APIs into one instance, and prefixes each tool with the API name.
- `mcp_server` already lets a user select several APIs in its config flow, and serves them as one MCP server.
- The `llm/api/list` websocket command already lists the registered APIs for the frontend.
- The `mcp` client integration already registers one API per config entry. It is the precedent for integration owned APIs.

So the missing parts are the `management` API itself, its tools, and a permission model. The rest is naming and convention.

## What goes in `management`

Tools that act on Home Assistant as a system:

- Search entities, devices, areas, floors and labels. Including entities that are not exposed.
- Read the error log and the system log.
- Read and write automations, scripts and scenes.
- Read integration and config entry state.

Tools that do not go in `management`:

- Controlling entities. That stays `assist`.
- Anything specific to one integration. That goes in that integration's own API.

## The part that needs a decision: permissions

`assist` is safe because of entity exposure. A user decides which entities an assistant can see. `management` has no equivalent gate, and it must not have one. The point of an entity search tool is to find the entity that is not exposed yet.

Today the only gate is a string comparison in `mcp_server/http.py`:

```python
if api_id != llm.LLM_API_ASSIST and not request["hass_user"].is_admin:
    raise Unauthorized
```

That line breaks as soon as a third API exists. It assumes every non-Assist API needs an administrator, and it lives in one consumer. A conversation agent configured with `management` does not pass through it at all.

**Recommendation:** the requirement belongs on the API, not on the consumer. Add a declaration to `llm.API`, for example `admin_only: bool`. Enforce it once, in `async_get_api`, against the user in `LLMContext.context`. Then delete the comparison above, and let config flows filter the list they offer.

This is the decision the ADR should record. The rest follows from it.

## Open questions

**1. Voice, and any context without a user.** A voice satellite has no authenticated user. `LLMContext.context.user_id` is `None`. If `management` is administrator only, a voice pipeline configured with it fails at runtime, not at configuration time. Filtering at selection time is better than an error mid conversation. Should a config flow hide an administrator only API when the consumer cannot supply a user?

**2. Read before write.** Creating an automation writes configuration. Home Assistant has no concept of a tool call that needs user confirmation, and MCP clients handle confirmation differently. The smallest useful first version is search, logs and read only inspection. Writes can follow once the permission model has landed. Is that split worth it, or does `management` ship with writes from the start?

**3. Tool name prefixes.** `MergedAPI` prefixes each tool with the API name. Tools are already required to be prefixed with the domain that offers them, and the unprefixed case breaks in 2027.3. Merging both gives `management__config__create_automation`. The API prefix is now redundant for tools we control. Should `MergedAPI` stop prefixing tools that already carry a domain prefix, and keep prefixing only for APIs whose tool names we do not control, such as the `mcp` client?

**4. Identifiers for integration APIs.** The `mcp` client uses `mcp-<slug>`. KNX and Modbus each have a config entry per bus, so they would do the same. Without a convention the list a user picks from becomes hard to read. Suggestion: `<domain>` for a single instance integration, `<domain>-<entry slug>` when several entries exist, and administrator only by default.

**5. Where a user composes a tool set.** `mcp_server` is `single_config_entry: true`. A user gets one combined server, plus one endpoint per single API. A user who wants one tool set for a desktop agent and a different one for a coding agent cannot express that. Do we allow several `mcp_server` config entries, or introduce a named tool set object that both MCP and the chat UI can reference?

**6. Prompt and tool budget.** Every API contributes a prompt fragment, and merging concatenates them. Three APIs mean three prompts and the sum of their tools. `management` should contribute few tools that search, rather than one tool per operation. Is that a guideline, or a limit we enforce?

**7. The name.** `management` reads well and does not collide. `config` collides with the `config` integration. `admin` describes the permission, not the scope. Proposal is to keep `management`.

## Suggested first step

1. Add the permission declaration to `llm.API` and enforce it in `async_get_api`. Remove the comparison in `mcp_server`.
2. Register a `management` API in the `llm` integration, administrator only, with no tools.
3. Add entity, device and area search, plus log reading, through the existing `llm.py` platform.
4. Decide on writes separately, once the first three are in use.
