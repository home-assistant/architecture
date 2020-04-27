# 0011. Discovery Requires Unique ID

Date: 2020-04-27

## Status

Accepted

## Context

There is a mechanism to ignore discovered config flows using unique ids attached to config flow
but not all integrations may set an unique id due to lack of truly unique identifiers
or just not having been updated for concerns of backward compatibility.

This creates an UX issue that has started to become more pronounced with more config flows
being added every release (which is awesome!). The real issue is that not all discovery processes 
are created equal and that becomes an issue for consistency.

## Proposal

All new integrations with discoverable config flows should be required to set a
unique id during discovery.

All existing integrations with discoverable config flows should be reviewed for
possible ways to improve identification during discovery. A whitelist could be developed
to allow existing integrations to remain functional under new requirements.

If a unique id can not be made available, such as on older devices that lack unique identifiers
in zeroconf/SSDP/API/etc, then the config entry should not be shown to the user.

## Decision

To protect project goals and to provide clarity to our users and contributors,
we're introducing the following rules on how discoverable integrations need to be designed:

- Integrations that are discoverable must provide an unique id (via `async_set_unique_id`) 
to allow the user to ignore the discovered config entry.

These rules apply to all new integrations. Existing integrations should be reviewed 
for possible ways to improve identification during discovery.

## Consequences

- Removes confusion and questions around the ability to ignore unwanted discoveries.
- This might impact the number of integrations that are able to be discovered.
