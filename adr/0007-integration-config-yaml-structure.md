# 7. Config YAML structure for integrations

Date: 2020-04-05

## Status

Accepted

---

## Context

There are currently many different ways of structuring an integration and its config in Home Assistant. We allow config YAML, either under the integration key or under platform keys with the integration name. We allow a config flow with configuration entered via the GUI, stored in config entries. We allow importing config YAML to a config entry via config flow.

This ADR focuses on the configuration YAML structure and its use in integrations.

The many options for configuration impact how the integration is structured and what Home Assistant backend APIs are used.

1. Integrations that only have a single platform, place the configuration under a platform key and use `async_setup_platform` to set up the platform.
2. Integrations that have multiple platforms, sometimes centralize the configuration to the integration domain name key and load platforms via `discovery.async_load_platform`, which in turn calls `async_setup_platform`.
3. If the integration has multiple platforms but doesn't have a centralized config, the user needs to add configuration under the integration domain to set up the integration and each platform key to set up each platform. This loads each platform via `async_setup_platform`.

All backend APIs in point 1-3 above also have sync versions, to further increase the options.

This multitude of options for config and integration structure makes it:

A. Harder for users to know how to configure the integrations.
B. Harder for contributors to know what way to implement and support.
C. Harder for Home Assistant to support confused users and contributors and harder to support and extend a disparate code base.

---

## Decision

We limit the configuration YAML structure to one way for new integrations. We require all YAML configuration, if present, for an integration to be located under the integration domain key in configuration YAML, for all new integrations (point 2 above).

For existing integrations we don't allow changes to YAML configuration in platform sections until the integration has been refactored and the configuration moved under the integration domain key.

```yaml
# Allowed
awesome_integration:
  username: user

# Not allowed
sensor:
  - platform: awesome_integration
    username: user
```

### Exceptions

- Implementing an optional `unique_id` configuration key to set the unique ID of the entity created by the platform is allowed.

## Consequences

- New integrations have to put the integration configuration section under the integration domain key if configuration YAML is used.
- Configuration YAML in platform sections in existing integrations is frozen until the integration is refactored.
- Integration platforms have to be loaded via `discovery.async_load_platform` or `discovery.load_platform` if not using configuration entries. This is a small increase in complexity for new integration contributions.
- Decreases the number of breaking changes when an integration wants to add more platforms and move the configuration from a per platform configuration to a centralized integration domain configuration section.
- Increases the consolidation of Home Assistant Core configuration YAML integration structure; instead of increasing disparity.
