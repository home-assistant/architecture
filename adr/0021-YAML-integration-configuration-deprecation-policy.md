# 0021. YAML integration configuration deprecation policy

Date: 2023-06-15

Architecture discussion: [#920](https://github.com/home-assistant/architecture/discussions/920)

## Status

Accepted

## Context

When a YAML integration configuration option (or options) are to be removed, we
generally account for the deprecation period in which we still support or handle
the migration of such YAML configuration option(s).

The risk of a too short deprecation period is that if a user does not upgrade
their Home Assistant instance in time, they will miss out on the possible
migrations offered during the deprecation period and thus run into
breaking changes.

## Decision

Give users more time to upgrade and thus reduce the risk of running into
breaking changes; the deprecation period for YAML configuration options is
set to at least 6 months (being 6 release cycles at the time of writing).

Additionally, the following requirements for deprecation are set:

- If a migration (for example, using n config flow import) is possible, it
  becomes required to add a migration. If migration has failed, this must be
  reflected by raising a repair issue on what happened and what a user has
  to do to resolve the issue.

- Using deprecated YAML integration configuration options must raise an
  issue in the users' repairs dashboard. This issue should explain why
  this happens and what a user has to do to resolve the issue. In case an
  migration is available, this message should only be shown after a
  successful migration.

## Consequences

- We need to keep around imports and deprecated YAML configuration options
  around longer.
- The risk of a user running into a breaking change by skipping a couple of
  releases will be reduced.
