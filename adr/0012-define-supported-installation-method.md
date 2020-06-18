# 0012. Define supported installation method

Date: 2020-06-03

## Status

Accepted

## Context

If you look at our documentation, it’s all over the place. Install it in Docker, in a VM, on a NAS or on one of the many Linux distributions.

The reason we have this many guides is that since the start of the Home Assistant website, we have always gladly accepted every contribution to get Home Assistant running on any platform. The more the merrier!

However, in software, nothing ever stays the same. All software gets updates to fix bugs, fix security vulnerabilities, improve performance or to add new features. As a software application you need to grow along or else you get stuck with an insecure system.

So as Home Assistant grows and evolves, some of these installation guides became outdated and stopped working. We wouldn’t even know it was broken until a user raised an issue. But when they do, we wouldn’t know how to fix it unless we could get a hold of the original contributor.

This can be frustrating. Any guide on our official website should lead to a working system. A system that not only works today, but also tomorrow.

## Decision

A supported installation method in the Home Assistant context means:

> A way of installing and running Home Assistant in a way that is supported by the Home Assistant developers. Supported means the installation method is tested and documented in the official documentation. Running Home Assistant using such a supported method, leads to the optimal user experience.

The Home Assistant team will not prevent you from running Home Assistant using an unofficial method. However, we cannot help with issues that you encounter. We are open for contributions that improve compatibility with a community-supported method as long as they do not impact officially supported methods, add a significant amount of code exceptions or future maintenance burden on the Home Assistant development team.

## Consequences

Move existing documentation that does not match supported installation methods to the community guides wiki. This includes OS-specific instructions in integration documentations that are not supported.

It is clear to the users what to expect when installing Home Assistant based on our documentation.
