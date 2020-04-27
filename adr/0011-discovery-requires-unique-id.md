# 0011. Discovery Requires Unique ID

Date: 2020-04-27

## Status

Accepted

## Context

There is a mechanism to ignore discovered config flows using unique ids attached to config flow
but not all integrations may set a unique id due to lack of truly unique identifiers
or just not having been updated for concerns of backwards compatibility.

This creates an UX issue that has started to become more pronounced with more config flows
being added every release (which is awesome!). The real issue is not all discovery processes 
are created equal and that becomes an issue for consistency.

## Proposal

All future integratons with discoverable config flows should be required to set an
unique id during discovery.

All existing integrations should be reviewed for possible ways to improve identification
during discovery config flow. A whitelist could be developed to allow existing integrations
to remain functional under new requirements.

If a unique id can sometimes not be available such as older devices lack identifiers in 
zeroconf/ssdp/etc then the device should be prevented from being shown to user.

## Decision

To protect project goals and to provide clarity to our users and contributors,
we're introducing the following rules on how discoverable integrations need to be configured:

- Integrations that are discoverable must provide an unique id (via `async_set_unique_id`) 
to allow the user to ignore the discovered config entry.

These rules apply to all new integrations. Existing integrations should be reviewed 
for possible ways to improve identification during discovery.

## Consequences

- Removes confusion and questions around the ability to ignore unwanted discoveries
  for all users.
- This might impact the number of integrations that are able to be discovered.
