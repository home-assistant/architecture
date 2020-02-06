# 0009. Do not remove YAML config keys.

Date: 2020-02-06

## Status

Accepted

## Context

When integrations evolve, config keys can change. This means that options that are currently valid can become invalid in a new version.

## Decision

When a config key is no longer used in a configuration schema, don't remove it but instead wrap it in `cv.deprecated`. The user will get a warning and that is fine.

## Consequences

Reduce the number of breaking changes and instead have more deprecation warnings.

Remove the version number from `cv.deprecated` validator.
