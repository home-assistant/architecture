# 0012. Installation method: Home Assistant Container

Date: 2020-06-03

## Status

Accepted

## Context

Define a supported installation method as per [ADR-0012](https://github.com/home-assistant/architecture/blob/master/adr/0012-define-supported-installation-method.md).

## Decision

This is for running just the Home Assistant Core application on native OCI compatible containerization system. It does not provide the Supervisor experience, and thus does not provide the Supervisor panel and add-ons.

This is a general installation method that is recommended as an alternative to the Home Assistant OS installation method. Due to the shared image with the Home Assistant OS installation method, almost all documentation applies to the Home Assistant Container as well.

The only supported way to run the container is on the host network as root with full privileges.

### Supported Containerization system and version

- Any Open Container Initiative (OCI) compatible containerization system.

### Supported boards/hardware/machines

- Machines of the following architectures: amd64, i386, armhf, aarch64, armv7

### Supported Operating Systems and versions

- Running Home Assistant Container is only supported on Linux.
- Windows and BSD installations (e.g., macOS and FreeBSD) are not supported.

### Additional notes

There is a wide variety of containerization software available. From that perspective, Home Assistant will only actively document the use of Docker.

### Required Expertise

- **Installation**
  This requires the user to have an existing system that can run Docker containers. Installation is either done by running a command from the Docker-cli or via a user interface (Synology, Portainer)

* **Start when the system is started:** The user is responsible for configuring the system to start the container when the system is started.
* **Run with full network access:** Default installation instructions prescribe net=host to be configured.
* **Access USB devices:** It is up to the user to ensure that all devices are correctly passed through to the container.

* **Maintaining the Home Assistant installation**
  If using the Docker-cli the user needs to manually update the run command. If using a UI the user might be notified of an upgrade or automatically update – automatically applying updates may result in the system not coming back online. There is no rollback in case the instance does not come online after an update.

  - **Python upgrades:** Included in the Home Assistant container
  - **Installing Python dependencies:** Included in the Home Assistant container
  - **Updating Home Assistant:** Included in the Home Assistant container

- **Maintaining the Operating System**
  Since this is just the core container, all OS responsibilities are with the user.

  - **Security updates for OS:** Responsibility of the user.

  - **Maintaining the components required for the Supervisor:** No supervisor, so N/A

**Conclusion:** medium expertise required. Some Docker UIs make it easy to run and update containers. Mapping devices and manually updating Home Assistant will be challenging as they depend per platform.

## Consequences

Update documentation on how to install this method, the required experience and the expected maintenance.

Move existing related documentation that does not match the requirements to the community guides wiki.

Notify user during onboarding of expected maintenance for their installation method.

Add checks to Home Assistant to warn (but not forbid) when not all requirements are met for this installation method.
