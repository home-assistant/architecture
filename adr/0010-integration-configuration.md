# 0010. Integration configuration

Date: 2020-04-14

## Status

Accepted

## Context

Home Assistant has been relying on YAML for its configuration format for a long
time. However, in certain cases, this caused issues or did not work at all.
These cases are best explained by listing the different categories of
integrations that we have in Home Assistant:

- Integrations that integrate devices. Examples include Hue, TP-Link.
- Integrations that integrate services. Examples include AdGuard, Snapcast.
- Integrations that integrate transports. These integrations allow users to
  define their own protocol. Examples include MQTT, serial, GPIO.
- Integrations that process Home Assistant data and make this available to
  other integrations. Examples are template, stats, derivative, utility meter.
- Integrations that provide automations. Examples include automation,
  device_sun_light_trigger, alert.
- Integrations that help controlling devices and services.
  Examples include script, scene.
- Integrations that expose Home Assistant data to other services.
  Examples include Google Assistant, HomeKit.

In all but the first two cases, YAML does just fine. The configuration is static,
is not discovered and relies on the user setting it up. These cases have been
solved by providing a hybrid approach. We offer YAML with a reload service and
we offer Storage Collections, which allows the user to create/manage these
integrations via the UI.

However, in the first two cases, it doesn’t work. Integrations are discovered.
Integrations require users logging in on the vendor’s website and authorize
linking (OAuth2) or users are required to press buttons on the hub to authorize
linking (i.e. Hue).

In the cases that people can authorize an integration by just putting their
username and password in the YAML file, they don’t want to, because it prevents
them from sharing their configuration. This is solved currently by using YAML
secrets that are substituted during load. This results in one file that provides
the structure of your configuration and one file that provides the values.
See below for an anonymized example as can be found on GitHub:

```yaml
camera:
 platform: onvif
 name: bedroom
 host: !secret camera_onvif_bedroom_host
 port: !secret camera_onvif_bedroom_port
 username: !secret camera_onvif_bedroom_username
 password: !secret camera_onvif_bedroom_password
```

So to solve these first two cases, we’ve introduced config entries (a
centralized config object) and config flows. Config flows handle creating config
entries with data from different sources. It can handle a new entry created via
the user interface, automatic discovery, but also is able to handle importing
configuration from YAML. Config entries allow for migrations during upgrades,
limiting the breaking changes we have to induce on our users.

Config flows empower users of all knowledge levels, to use and enjoy
Home Assistant. Since the introduction of config flows we’ve kept it open to
contributors of individual integrations to decide if they want to implement
YAML and/or a user-facing config flow.

Some contributors have decided to drop the YAML import to reduce their
maintenance and support burden. A burden that they volunteer to do in their
spare time. This has sadly resulted in a few pretty de-motivating comments,
towards the contributors and the project in general. These comments often
violate the code of conduct we have in place to protect the Home Assistant
community.

This induces the risk of losing contributors and maintainers, halts our project
goals and slows down innovation. As an open source project, maintaining our
contributors is our highest priority as they are the creators of the project
in the first place. They should be highly admired and valued for their
contributions.

From a project perspective, we have not provided the necessary guidelines on
this matter for our contributors to work with and have therefore not managed
the expectations of our users to a full extent.

## Decision

To protect project goals and to provide clarity to our users and contributors,
we’re introducing the following rules on how integrations need to be configured:

- Integrations that communicate with devices and/or services are only configured via
  the UI. In rare cases, we can make an exception.
- All other integrations are configured via YAML or via the UI.

These rules apply to all new integrations. Existing integrations that should
not have a YAML configuration, are allowed and encouraged to implement a
configuration flow and remove YAML support. Changes to existing YAML
configuration for these same existing integrations, will no longer be accepted.

## Consequences

- Power to our contributors! ❤
- Removes confusion and questions around the future of the YAML configuration
  for all users and contributors.
- Builds upon the goals that have been set out, as presented at the
  State of the Union 2019.
- This might impact the number of integrations contributed. This requires
  configuration flows, which require tests. However, we do provide scaffolding
  scripts for this (`python3 -m script.scaffold`).
