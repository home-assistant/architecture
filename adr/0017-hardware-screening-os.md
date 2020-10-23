# 0017. Hardware screening for OS

Date: 2020-10-23

## Status

Draft

## Context

We need take a decision for the minimal requirements of a IoT-Board to become a Home Assistant OS image.
Also how we deal with old or replaced generation/boards.

## Decision

Follow are the requirements of a Board:

- The board need to be available for everbody
- The manifacture produce and sell the board at least 3 years
- The manifacture is know for his continuity
- The board need to be supported by mainstream Linux kernel with current or next LT
- The board need supported by u-boot/barebox bootloader

It's allow to use some patches as long they only minimal touch the Kernel and marked for upstream.

We support the board so long as possible with Updates as we don't have a downside and we have hardware
for the sporadic tests. If there comes the next generation, we provide updates for the old but don't recommend it anymore.

If a board is not supported anymore, they get no Updates from Operating-System or Home Assistant Core and according ADR0015.
It's recommended to update the Hardware to next Generation or a new Board.

## Consequences

All contribution for new Images with not follow this specs are not accepted.
