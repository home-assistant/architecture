# 3. Monitor condition and data selectors

Date: 2019-05-23

Architecture issue: [#100](https://github.com/home-assistant/architecture/issues/100)

## Status

Accepted

## Context

A lot of Home Assistant integrations use config options like `CONF_MONITORED_CONDITIONS` to allow the user to select which data the integration should expose from the data. This means that the user needs to know what the different data points means while setting up the component/platform. While configuring it's Lovelace UI, the user has the option to include the entity or not. This means that we allow the user to pick twice.

## Decision

Integrations should expose all available data to the backend if that data is fetched in a single API request.

We should redouce selector logic mostly to end point if it make sense in context of interface. User should not read available documentations and API descrition to find out which data they want have.

```
Layer model:

      Fetch            Manage           View
  -------------      ---------      ------------
  | Interface | ---> | Core  | ---> | Frontend |
  -------------      ---------      ------------
```

## Consequences

We don't merge PRs with useless data selectors. And remove available uneeded selectors to Home Assistant 1.0
We focus us to a good view/frontend to select and group available data.
