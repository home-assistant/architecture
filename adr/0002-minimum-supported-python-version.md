# 2. Minimum supported Python version

Date: 2019-05-13, updated 2023-02-05 (Python version clarification)

Architecture issue: [#167](https://github.com/home-assistant/architecture/issues/167), [#278](https://github.com/home-assistant/architecture/issues/278)

## Status

Accepted

## Context

Home Assistant currently sets the minimum Python requirement to the penultimate minor Python version released upstream.

## Decision

Home Assistant will try to support the latest two released minor upstream Python versions. For example, if the current latest version is 3.11, we will support Python 3.10 and would like to support Python 3.11; however, Python 3.9 is no longer supported at this point.

Once a new minor Python version is released, the to be dropped minor version will be deprecated for a period of 2 months, after which it will be removed. While deprecated, Home Assistant will print a warning message to inform the user.

Supported micro versions are decided on minor version basis, and codified in the version check code in Home Assistant proper.

## Consequences

- June 1, 2019: Deprecate Python 3.5
- August 1, 2019: Adopt that Home Assistant will support the latest 2 minor releases of Python. This means that we drop support for Python 3.5.

We have three installation methods that are based on Docker containers (Home Assistant OS, Container, and Supervised), which are maintained and kept up to date by the Home Assistant project.
