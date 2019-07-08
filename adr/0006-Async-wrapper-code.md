# 6. Async wrapper code

Date: 2019-07-08

Architecture issue: [#256](https://github.com/home-assistant/architecture/issues/256)

## Status

Accepted

## Context

Currently, there is no strict guideline for async/sync core calls from integrations and where it should handle the wrapper code for a sync library/interface. The fact is, our core already has a highly optimized async/sync wrapper to interface with sync interfaces/libraries.

It makes it hard to read and review code when one is looking at many async functions, and later on, one would discover that the library is not async at all.

Some integrations handle this in their own wrapper, basically recreating the same code as the core wrapper already provides. We should prevent creating duplicate code.

A synced library might also do I/O on a next release, causing blocks. It is almost impossible to check every method/function of a library on an update to ensure every called function is async safe or not.

The problem might be self-inflicted, as we announced asyncio is faster. Correct; however, this is only the case if all things are truly async.

It is not wrong to use blocking I/O on libraries and implement the sync Home Assistant interface instead of the async interface.

## Proposal

The core is the only place that has async wrapper code for sync methods.

An integration that is not async must only use sync functions and also solely implement the sync interfaces of the Home Assistant entity.

One exception are locks. With lock entities, it's allowed to use a mix of sync and async until we have a solution for such constructs in our core.

## Consequences

We don't merge PRs that implement new integrations or features that mix up sync and async or are using wrappers to work around it.
