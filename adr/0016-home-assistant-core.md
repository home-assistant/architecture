# 0016. Installation method: Home Assistant Core

Date: 2020-07-01

## Status

Accepted

## Context

Define a supported installation method as per [ADR-0012](https://github.com/home-assistant/architecture/blob/master/adr/0012-define-supported-installation-method.md).

## Decision

This is for running just the Home Assistant Core application directly on Python. It does not provide the full Supervisor experience and thus does not provide the Supervisor panel and add-ons.

### Supported Operating Systems and versions

- All major Linux distributions, latest stable and major versions.
- Windows; only using WSL.
- macOS; Python via Brew.

### Supported Python versions

Running Home Assistant Core is only supported when running the application using the official Python virtual environment. Running Home Assistant Core without a virtual environment, system/globally installed Python packages, is not supported.

The latest two released minor Python versions are supported (3.7 & 3.8 at the time of writing). Once a new minor Python version is released, the to be dropped minor version will be deprecated for a period of 2 months, after which it will be removed.

### Required Expertise

- **Installation**
  Requires installing Python 3 with venv support (the default except on Debian based systems). Then create a virtual environment and install Home Assistant Core via pip.

  For packages that require compilation, the user will need to install compilers and other development packages. If those development packages are not the same as provided by the operating system, you can break your system.

* **Start when the system is started:** This is the responsibility of the user. It is based on their operating system.
* **Run with full network access:** Works, is the only option.
* **Access USB devices:** This works out of the box.

* **Maintaining the Home Assistant installation**
  Maintenance requires more time, effort, skills, and experience than the other methods.

  - **Python upgrades:** Home Assistant upgrades Python every year. It can happen that your current operating system doesn’t support the new minimum required version out of the box. In that case, you need to find unofficial Python packages for your system or compile Python from source.
  - **Installing Python dependencies:** Some Python packages need compilation. Users are responsible for having the right compilers and development packages installed.
  - **Updating Home Assistant:** Updating happens via the pip command-line tool.

- **Maintaining the Operating System**
  Home Assistant Core runs in a Python virtual environment. Anything outside of that is the responsibility of the user.

* **Security updates for OS:** Responsibility of the user.

* **Maintaining the components required for the Supervisor:** No supervisor, so N/A

**Conclusion:**
This is an expert installation method. Based on the integrations that you’re running, you will need a lot of extra packages installed.

## Consequences

Update documentation on how to install this method, the required experience and the expected maintenance.

Move existing documentation that does not match supported installation methods to the community guides wiki.

Notify user during onboarding of expected maintenance for their installation method.
