# 7. Require config yaml and flow

Date: 2019-09-17

## Status

Proposed

---

## Context

This ADR has two faces that it wants to address:

1. The configuration structure disparity, and its consequences.
2. What Home Assistant is promising users in terms of ways of configuration.


### Config structure disparity

There are currently many different ways of structuring an integration and its config in Home Assistant. We allow config yaml, either under the integration key, or under platform keys with the integration name. We allow a config flow with configuration entered via the GUI, stored in config entries. We allow importing config yaml to a config entry via config flow.

These many options for configuration also impacts how the integration is structured and what Home Assistant backend APIs are used.

1. Integrations that only have a single platform puts the configuration under a platform key and just uses `async_setup_platform` to set up the platform.
2. Integrations that have many platforms sometimes centralize the configuration to the integration domain name key and loads platforms via `discovery.async_load_platform`, which in turn calls `async_setup_platform`.
3. If the integration has multiple platforms but doesn't have a centralized config, the user needs to enter config under the integration domain to set up the integration and each platform key to set up each platform. This loads each platform via `async_setup_platform`.
4. Integrations that support config flow, uses `async_setup_entry` to set up the config entry for the integration and `config_entries.async_forward_entry_setup` to set up platforms. Setting up platforms via `discovery.load_platform` is not available for these integrations.

All backend APIs in point 1-3 above also have sync versions, to further increase the options.

This multitude of options for config and integration structure makes it:

1. Harder for users to know how to configure the integrations.
2. Harder for contributors to know what way to implement and support.
3. Harder for Home Assistant to support confused users and contributors and harder to support and extend a disparate code base.


### Home Assistant's promise to users

On more than one occassion Home Assistant official representatives have said that config yaml will not go away as a way of configuring Home Assistant, even after introducing config flow. If we are serious about that promise, Home Assistant needs to take responsibility for the way that integrations offer ways of configuration. If we do not require integrations to offer yaml as a way of configuring, we have to change what we say to users and inform them that we can not promise that yaml will stay.

---

## Proposal

1. We limit the configuration structure to one way for new integrations:
  - We require all yaml config for an integration to be located under the integration domain key in config yaml, for all new integrations.
  - We require both config yaml and config flow support for all new integrations.
2. We do not allow removing support for either config yaml or config flow in existing integrations.


This proposal will solve both faces of the ADR purpose.

---

## Consequences

1. The contributor work of implementing a single platform will become greater. Config flow requires unit tests, which not all contributors are familiar with.
2. New integration PRs will become longer and require more from code review.
3. Integration maintenance by developers will increase since they have to support both config yaml and config flow.
4. It will be easier to recommend approaches for implementation to contributors and we can simplify our development documentation, with fewer approved ways of structuring an integration.
5. We will have fewer breaking changes, since integrations will implement the best way from the beginning with a centralized config and config entries support migration.
6. We can send a clearer message to users about how configuration is done in Home Assistant.
