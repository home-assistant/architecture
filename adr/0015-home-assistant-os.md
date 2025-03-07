# 0015. Installation method: Home Assistant OS

Date: 2020-06-08

Changelog:
 - 2025-01-23 Updated the list of hardware whose support was added in the meantime

## Status

Accepted

## Context

Define a supported installation method as per [ADR-0012](https://github.com/home-assistant/architecture/blob/master/adr/0012-define-supported-installation-method.md).

## Decision

Home Assistant OS is the full installation of our all-inclusive home automation system. Best in class home automation is complemented with a UI for configuring your system, making backups and safe updates with automatic rollback.

This is the generally recommended installation method and the one that our website and documentation should be focused on.

### Supported boards/hardware/machines

- Home Assistant Yellow (with CM4 and CM5 modules)
- Home Assistant Green
- Raspberry Pi 3 Model B and B+ 32-bit
- Raspberry Pi 3 Model B and B+ 64-bit
- Raspberry Pi 4 Model B 32-bit
- Raspberry Pi 4 Model B 64-bit
- Raspberry Pi 5 Model B 64-bit
- ASUS Tinker Board
- ASUS Tinker Board S
- ODROID-C2
- ODROID-C4
- ODROID-N2/N2+
- ODROID-M1
- ODROID-M1S
- ODROID-XU4
- Bare-metal on x86-64 PCs (e.g. Intel NUC, via `generic-x86-64` image) on best effort basis (i.e. not all x86-64 machines are guaranteed to be supported fully)
- Virtual Machine (x86-64/AMD64-based by `ova` image, AArch64/ARM64-based by `generic-aarch64` image)

### Supported Operating Systems and versions

This installation method is only available and supported using the Home Assistant Operating System. Only the latest major version is supported.

When a new major version is released, the previous major version will be dropped with a deprecation period of 2 months. The last 3 minor releases are supported.

### Supported Hypervisors

The Home Assistant Operating System can be run on a Hypervisor and thus be run as a virtual machine. The following Hypervisors are supported:

- KVM/QEMU
- VirtualBox
- VMWare
- Xen

We will provide documentation for the following systems build on top of these technologies:

- Proxmox (KVM/QEMU based)
- Unraid (KVM/QEMU based)
- VirtualBox
- VMWare

### Required Expertise

- **Installation**
  Installation requires the user to install the disk image for their device, install an image flasher (Etcher) and flash the image to the disk. Then insert image and boot their device.

* **Start when the system is started:** Managed by installer
* **Run with full network access:** Managed by installer
* **Access USB devices:** Managed by supervisor

* **Maintaining the Home Assistant installation**
  Ongoing maintenance can be done via the user interface. User will be notified if an update is available for the operating system or for Home Assistant. Updates will be automatically rolled back if they fail.

  - **Python upgrades:** Managed via HA updates
  - **Installing Python dependencies:** Managed via HA updates
  - **Updating Home Assistant:** Via a UI

- **Maintaining the Operating System**
  Ongoing maintenance can be done via the user interface. User will be notified if an update is available for the operating system. Updates will be automatically rolled back if they fail.

* **Security updates for OS:** Managed via HA OS updates

* **Maintaining the components required for the Supervisor:** Managed via HA OS updates

**Conclusion:** low expertise required. Besides the installation methods, everything is done via the UI.

## Consequences

Update documentation on how to install this method, the required experience and the expected maintenance.

Move existing related documentation that does not match the requirements to the community guides wiki.

Add checks to Home Assistant to warn (but not forbid) when not all requirements are met for this installation method.
