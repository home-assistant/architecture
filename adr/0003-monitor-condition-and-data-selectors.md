# 3. Monitor condition and data selectors

Date: 2019-05-23

Architecture issue: [#100](https://github.com/home-assistant/architecture/issues/100)

## Status

Accepted

## Context

A lot of Home Assistant integrations use config options like `CONF_MONITORED_CONDITIONS` to allow the user to select which data the integration should expose from the data. This means that the user needs to know what the different data points mean while setting up the integration. While configuring its Lovelace UI, the user has the option to include the entity or not. This means that we allow the user to pick twice.

## Decision

Integrations should expose all available data to the backend if that data is fetched in a single API request.

Integrations should only include selector logic if it make sense in the context of interface, like if it would require extra requests. User should not have to read the available documentation and API descriptions to find out which data they want have.

```
Layer model:

      Fetch            Manage           View
  -------------      ---------      ------------
  | Interface | ---> | Core  | ---> | Frontend |
  -------------      ---------      ------------
```

Integrations can set the `entity_registry_enabled_default` property on their entity objects to instruct Home Assistant to disable certain entities by default ([docs](https://developers.home-assistant.io/docs/entity_registry_disabled_by/#integrations-setting-default-value-of-disabled_by-for-new-entity-registry-entries)).

## Consequences

We don't merge PRs with data selectors. We remove existing unneeded selectors until Home Assistant 1.0.
We focus on a good view/frontend to select and group available data.
