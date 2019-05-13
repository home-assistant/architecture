# 2. Minimum supported Python version

Date: 2019-05-13

## Status

Accepted

## Context

Architecture issue: [#167](https://github.com/home-assistant/architecture/issues/167)

Home Assistant currently sets the minimum Python requirement to whatever is shipped in Debian Stable. As most installations are now based on Docker, it is easier to upgrade.

## Decision

Home Assistant will support the latest two released minor Python versions. For example, currently we will support Python 3.6 and Python 3.7. Once Python 3.8 is released, we will support only Python 3.7 and Python 3.8.

Once a new minor Python version is released, the to be dropped minor version will be deprecated for a period of 2 months, after which it will be removed. While deprecated, Home Assistant will print a warning message to inform the user.

## Consequences

- June 1, 2019: Deprecate Python 3.5
- August 1, 2019: Adopt that Home Assistant will support the latest 2 major releases of Python. This means that we drop support for Python 3.5.

We have three installation methods. Both Hass.io and Docker are already using the latest Python version. The next Debian Stable will ship with Python 3.7, so Hassbian users might not need to do anything besides updating their distro.

