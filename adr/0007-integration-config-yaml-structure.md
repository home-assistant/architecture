# 7. Config yaml structure for integrations

Date: 2020-04-05

## Status

Accepted

---

## Context

There are currently many different ways of structuring an integration and its config in Home Assistant. We allow config yaml, either under the integration key, or under platform keys with the integration name. We allow a config flow with configuration entered via the GUI, stored in config entries. We allow importing config yaml to a config entry via config flow.

This ADR will only focus on the configuration yaml structure and use in integrations.

The many options for configuration impacts how the integration is structured and what Home Assistant backend APIs are used.

1. Integrations that only have a single platform puts the configuration under a platform key and just uses `async_setup_platform` to set up the platform.
2. Integrations that have many platforms sometimes centralize the configuration to the integration domain name key and loads platforms via `discovery.async_load_platform`, which in turn calls `async_setup_platform`.
3. If the integration has multiple platforms but doesn't have a centralized config, the user needs to enter config under the integration domain to set up the integration and each platform key to set up each platform. This loads each platform via `async_setup_platform`.

All backend APIs in point 1-3 above also have sync versions, to further increase the options.

This multitude of options for config and integration structure makes it:

A. Harder for users to know how to configure the integrations.
B. Harder for contributors to know what way to implement and support.
C. Harder for Home Assistant to support confused users and contributors and harder to support and extend a disparate code base.

---

## Decision

We limit the configuration yaml structure to one way for new integrations. We require all yaml config, if present, for an integration to be located under the integration domain key in config yaml, for all new integrations (point 2 above).

```yaml
# Allowed
awesome_integration:
  username: user

# Not allowed
sensor:
  - platform: awesome_integration
    username: user
```

## Consequences

- New integrations have to put the integration configuration section under the integration domain key if configuration yaml is used.
- Integration platforms have to be loaded via `discovery.async_load_platform` or `discovery.load_platform` if config entries is not used. This is a small increase in complexity for new integration contributions.
- We will decrease breaking changes when an integration wants to add more platforms and move the configuration from a per platform configuration to a centralized integration domain configuration section.
- We will increase the consolidation of Home Assistant Core configuration yaml integration structure instead of increasing disparity.
