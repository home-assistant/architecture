# 0017. Hardware screening for OS

Date: 2021-08-03

## Status

Accepted

## Context

We need take a decision for the minimal requirements of a IoT-Board to become a Home Assistant OS image.
Also how we deal with old or replaced generation/boards.

## Decision

Follow are the requirements of a Board:

- The board needs to be available for private customers in most major economic regions
- The manufacturer produces and sells the boards for at least 3 years
- The manufacturer is know for his continuity
- The board needs to be supported by the upstream Linux kernel
- The board needs to be supported by the upstream U-Boot or Barebox boot loader

It's allowed to use some patches as long they only minimal touch the Kernel and marked for upstream.

We support the board as long as possible with updates. We might remove support if we see major downsides or we don't have hardware
for the sporadic tests.  When a next generation is available, we provide updates for the old but don't recommend it anymore.

If a board is no longer supported, no updates for the operating system or Home Assistant Core will be provided according ADR0015.
It's recommended to update the hardware to next generation or a new board.

## Consequences

All contributions for new images must follow these specifications to qualify, but will also need a signoff from the core team to be accepted.
