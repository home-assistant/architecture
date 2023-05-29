# 0020. Minimum supported Python version

Date: 2023-05-29

Architecture discussion: [#903](https://github.com/home-assistant/architecture/discussions/903)

Replaces: [ADR-0002](./0002-minimum-supported-python-version.md)

## Status

Accepted

## Context

Home Assistant currently sets the minimum Python requirement to the penultimate minor Python version released upstream.

## Decision

Home Assistant supports a single minor version of Python at a time, with the goal of supporting the latest minor upstream Python version.

When a new minor Python version is released, the Home Assistant project will provide compatibility and support for that new version as soon as possible.

Once the new version is supported by the Home Assistant project and rolled out in our container distributions, the previous Python version will be deprecated for a period of two release cycles. Home Assistant will print a warning message during the deprecation period to inform the user.

Development during the deprecation period will take place on the newer Python version. We rely on automated tests and linting to catch backward incompatible changes. The deprecated version will stay in our CI for an additional release cycle (that is a total of three),
to accommodate the patch releases that belong last and previous release of the deprecation period.

The minimal supported micro/patch Python version is decided on a minor version basis and codified in the version check code in Home Assistant proper.

## Consequences

We have three installation methods that are based on Docker containers (Home Assistant OS, Container, and Supervised), which are maintained and kept up to date by the Home Assistant project. A significant majority of our user base uses these installation methods. For users using the Core installation method (running their own a Python environment), they need to upgrade their Python version at least once a year.
