# 3. Monitor condition and data selectors

Date: 2019-05-23

Architecture issue: [#100](https://github.com/home-assistant/architecture/issues/100)

## Status

Accepted

## Context

Home Assistant components use mostly `CONF_MONITORED_CONDITIONS` or other config options that allow user to select which data the interface should send to backend. The user need known what the different data points means on setup of the component/platform. Later he can select the entity again, they he want see on UI.

## Decision

Home Assistant components/platforms should grap all available data and send this to backend. Available means data they are anyway sended from API to instance or if they need only spare resource to get all data.

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
