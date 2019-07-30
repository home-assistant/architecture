
# 6. Docker images

Date: 2019-07-19

Architecture issue: [#243](https://github.com/home-assistant/architecture/issues/243)

## Status

Accepted

## Context

We have now `homeassistant/home-assistant` as an amd64 image, based on Debian Linux, with a size of 761 (compressed).

On the other side, we have `homeassistant/...-homeassistant` with support for 5 different architectures based on Alpine Linux with a size of 376 (compressed). Hass.io uses these images.

The Hass.io images follow (most) best practices, and the Dockerfiles are checked with hadolint.

The Hass.io images support more integrations out of the box. Also, we pre-build all the Python Wheels for Alpine in the background. Additionally, support for pre-build Alpine packages is planned for the future.

Even though, the Hass.io images support more integrations, some things are not supported on Alpine. For example, TensorFlow and Chrome (the last one is less of an issue now web scraping is banned in [ADR-0004](0004-webscraping.md)).

In the current situation, we basically release and maintain two types of Docker images. The current Debian based one and the ones based on Alpine Linux. This requires us to solve issues twice or, in case of issues related to a specific Linux version, becomes harder to resolve to find out the culprit.

## Decision

Our Docker team and resources are limited and we should focus all our resource to one docker image.

Our regular Docker installation can be improved by tagging along with the Hass.io images, which features a more modular approach, follow better practices, are well maintained, supports five architectures, and have smaller images. Users will benefit from the pre-builds we provide and deliver, which results in a faster set up and installation of integrations.

We are going to transform our current `homeassistant/home-assistant` image into a Docker manifest image that provides multiple architectures. Each of the supported architectures in that manifest will point to one of the Alpine based Dockerfiles; we currently use for Hass.io: `homeassistant/...-homeassistant`.

Resulting in a single Docker base to maintain, while providing lots of benefits.

Timeline:
- **0.96**: Start with new Tagging system
- **0.97**: Maintain the end of homeassistant/home-assistant as debian based in Release Blogpost
- **0.98**: Start with manifest tagging

## Consequences

- We need to extend the Azure builds to support the tagging system `dev`, `beta` and `stable`.
- We need also move forward with the Python Wheels repository and the Home Assistant core pip installation for Docker environment. 
- We need to migrate our regular Home Assistant Docker image into a Docker manifest.

The plan is to complete this before the Home Assistant 1.0 release. Debian based images are deprecated with accepting of this ADR.
