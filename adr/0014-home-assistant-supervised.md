# 0014. Installation method: Home Assistant Supervised

Date: 2020-06-03

## Status

Accepted

## Context

Define a supported installation method as per [ADR-0012](https://github.com/home-assistant/architecture/blob/master/adr/0012-define-supported-installation-method.md).

## Decision

This installation method provides the full Home Assistant experience on a regular operating system. This means, all components from the Home Assistant method are used, except for the Home Assistant Operating System. This system will run the Home Assistant Supervisor. The Supervisor is not just an application, it is a full appliance that manages the whole system. It will clean up, repair or reset settings to default if they no longer match expected values.

By not using the Home Assistant Operating System, the user is responsible for making sure that all required components are installed and maintained. Required components and their versions will change over time. Home Assistant Supervised is provided as-is as a foundation for community supported do-it-yourself solutions. We only accept bug reports for issues that have been reproduced on a freshly installed, fully updated Debian with no additional packages.

This method is considered advanced and should only be used if one is an expert in managing a Linux operating system, Docker and networking.

### Supported Operating System, System dependencies and versions

Docker CE (Community Edition) is the only supported containerization method for Home Assistant Supervised. We only support FHS 3.0 on the host file system.

- Docker CE >= 19.03
- Systemd >= 239
- NetworkManager >= 1.14.6
- Avahi >= 0.7
- AppArmor == 2.13.x (built into the kernel)
- Debian Linux Debian 10 aka Buster (no derivatives)

Only the above-listed version of Debian Linux is supported for running this installation method. When a new major version of Debian is released, the previous major version is dropped, with a deprecation time of 4 months. An exception to this rule occurs if the new version does not meet the requirements of the Supervisor.

### Additional supported conditions

This installation method can easily be broken if one manages the operating system incorrectly. Therefore, the following additional conditions apply:

- The operating system is dedicated to running Home Assistant Supervised.
- All system dependencies are installed according to the manual.
- No additional software, outside of the Home Assistant ecosystem, is installed.
- Docker needs to be correctly configured to use overlayfs2 storage and journald as the logging driver.
- NetworkManager is installed and enabled in systemd.

In case any abnormality is detected that prevents Home Assistant from functioning, the Home Assistant Supervisor will report this to the user and block updates to prevent installations from breaking.

### Required Expertise

- **Installation**
  The user first needs to install Debian 10 and make sure all the required components are installed and are the correct version. They then need to run the installer script.

- **Start when the system is started:** done by the installer
- **Run with full network access:** done by the installer
- **Access USB devices:** done by the installer

- **Maintaining the Home Assistant installation**
  Home Assistant can be maintained from the Supervisor. This includes a rollback when the update fails.

  - **Python upgrades:** Included in Home Assistant updates
  - **Installing Python dependencies:** Included in Home Assistant updates
  - **Updating Home Assistant:** Via the UI

- **Maintaining the Operating System**
  The user is responsible for maintaining the operating system. Since it is a supervised installation, the user is also responsible for updating the components that are required by the Supervisor. The user is also responsible for not installing or changing anything on their system that will interfere with the Supervised installation. Examples are software that will update Docker containers managed by the Supervisor.

  - **Security updates for OS:** Responsibility of the user.
  - **Maintaining the components required for the Supervisor:** Responsibility of the user. Over time as Supervisor requirements change, you might have to upgrade your OS to be able to use the required version.

**Conclusion:** Expert. Maintaining a Debian 10 installation to a very specific set of requirements is hard.

## Consequences

Update documentation on how to install this method, the required experience and the expected maintenance.

Move existing related documentation that does not match the requirements to the community guides wiki.

Notify user during onboarding of expected maintenance for their installation method.

Add checks to Home Assistant to warn (but not forbid) when not all requirements are met for this installation method.
