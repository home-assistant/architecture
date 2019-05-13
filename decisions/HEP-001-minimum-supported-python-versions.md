# Minimum supported Python version

Architecture issue: [#167](https://github.com/home-assistant/architecture/issues/167)

Home Assistant will change the rules on how to determine the minimum supported Python version. We are moving away from following Debian stable and instead will support the latest two supported Python versions.

## Implementation

- June 1, 2019: Deprecate Python 3.5
- August 1, 2019: Adopt that Home Assistant will support the latest 2 major releases of Python. This means that we drop support for Python 3.5.
