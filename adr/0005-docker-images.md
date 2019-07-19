
# 5. Docker images

Date: 2019-07-19

Architecture issue: [#243](https://github.com/home-assistant/architecture/issues/243)

## Status

Accepted

## Context

We have now `homeassistant/home-assistant` as amd64 image based on Debian Linux with a size of 761 (compressed). On the other side, we have `homeassistant/...-homeassistant` with support 5 architectures based on Alpine Linux with a size of 376 (compressed). This image is also used by Hass.io and the Dockerfiles are checked by hadolint and mostly best praxis. Hass.io support more components out of the box. We build in the background a wheels repository for Alpine and want to extend that support in the future. It's true that the image supports more components but TF and chrome are currently not supported out of the box.

## Decision

We are currently fragmented with 2 Docker images and build system. Our Docker team and resource are very limited and we should focus all our resource to one Project/image system. Otherwise, we need to solve every problem twice. Also, can the regular Docker installation improved by coming Hass.io feature with modular component installation and smaller images.

I want that we have one build system and `homeassistant/home-assistant` going to be a meta repository that points to the correct architecture of `homeassistant/...-homeassistant`.

## Consequences

We need to extend the azure builds to support the tagging system `dev`, `beta` and `stable`.  We need also move forward with the wheels repository and the Home Assistant core pip installation for Docker environment.
