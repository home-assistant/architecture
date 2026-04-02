# 0020. Minimum supported Python version

Date: 2023-05-29

Architecture discussion: [#903](https://github.com/home-assistant/architecture/discussions/903)

Replaces: [ADR-0002](./0002-minimum-supported-python-version.md)

Changelog:
 - 2026-04-02 Removed Supervised and Core from the list of installation methods. Removed Core-specific deprecation period policy.

## Status

Accepted

## Context

Home Assistant currently sets the minimum Python requirement to the penultimate minor Python version released upstream.

## Decision

Home Assistant supports a single minor version of Python at a time, with the goal of supporting the latest minor upstream Python version.

When a new minor Python version is released, the Home Assistant project will provide compatibility and support for that new version as soon as possible.

The minimal supported micro/patch Python version is decided on a minor version basis and codified in the version check code in Home Assistant proper.

## Consequences

We have two installation methods that are based on Docker containers (Home Assistant OS and Container), which are maintained and kept up to date by the Home Assistant project. A significant majority of our user base uses these installation methods.
