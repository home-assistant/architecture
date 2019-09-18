# 8. Code owners

Date: 2019-09-18

Architecture issue: [#287](https://github.com/home-assistant/architecture/issues/287)

## Status

Draft

## Context

We get contributed a lot of new integrations, new features to integrations and refactors of integrations. The Home Assistant project is honored to receive so many great contributions to our project!

Unfortunately, as a contributor, adding oneself as (the, or one of the) code owners of the integration contributed or contributed to, doesn't happen spontaneously that often.

Not adding oneself as a code owner has drawbacks for the project:

- The contributor doesn't "own" (in terms of taking responsibility) his code, and thus contribution, in a more formal fashion.
- Without being listed as a code owner, our GitHub bot will not notify the contributor, when an issue for the integration is reported, quite possibly affecting his contribution.
- Most integrations end up with having no or a single code owner.

As a result of this:

- Bugs are less likely to be resolved in a timely fashion (turn-around time).
- Integrations are more prone to break in the future.
- Integration with a single code owner:
  - Do not benefit from multiple code owners being familiar with the integration in terms of code review and general turn-around time.
  - Become largely unmaintained when the single listed code owner can no longer contribute to the project.

During the design discussion of this ADR, it also became clear, that the term "code owner" has different meanings to our members and contributors. Some interpret it as an honorable mention of contribution; others see it as "taking responsibility".

## Decision

Code ownership for an integration defined:

The willingness of a contributor to try, at best effort, to maintain the integration. Providing the intention for handling issues, providing bug fixes, or other contributions to the integration he is listed on as a code owner.

### Rules

In order to support having (multiple) code owners for integration, to raise the quality and interaction on integration in our codebase, we have a set of rules (and exceptions in the next chapter).

For the following cases, adding oneself as a code owner is required:

- When contributing a new integration.
- When contributing a new platform to an integration.
- When contributing a new feature to an integration.
- When contributing a significant refactor or rewrite of an integration.

Contributions to our integrations, in the above-listed scopes, without having the contributor listed or added as the code owner, is no longer accepted.

### Exceptions

Some exceptions are in place, to prevent contributors to become demotivated to contribute; and are mainly based around smaller, low-impact contributions.

In the following cases, code ownership may be omitted:

- Contributions that solely provides a bug fix(es).
- Contributions that only provide additional unit test(s).
- Contributions to integrations marked as "internal". These integrations are code owned by the Home Assistant core team.
- Contributions refactoring across multiple integrations, caused by changes to our core codebase. E.g., due to changes to the used platforms.
- Small or low impact contributions to an integration. A currently active code owner for the integration or a Home Assistant code reviewer can decide it may be omitted.
- The contributor pro-actively rejects to be listed as a code owner; however, a currently active code owner is willing to accept and take ownership for the contribution provided by the contributor.

Code owner(s) and Home Assistant code reviewers are encouraged to ask a contributor to join an integration code ownership, even when the contribution matches one of the exceptions above.

### Withdrawing as a code owner

Withdrawing code ownership can happen, and it is sad to see an active code owner leaving the project.

A code owner may open up a PR to removing oneself as a code owner. However, this should only be accepted after the last contribution to the integration, made by the contributor, is released in a stable Home Assistant release.

## Consequences

This ADR may negative result in the following:

- Requiring code ownership may lead to fewer contributions.
- Requiring code ownership may lead to new code owners, that don't take up on that ownership.
- Integrations owned by multiple code owners may lead to a struggle between the code owners.

This ADR may positively result in the following:

- Less "unmaintained" or "abandoned" integrations.
- It will lead to multiple code owners for the same integrations. Adding: Fallback, code reviewers with context, and possible faster turn-around.

This ADR may also result in the following (both positive and negative):

- Requiring code ownership may remove the feeling of honor from the contribution; however, it does add a more formal sense of responsibility.

### Adjustments

The following label needs to be added to the code base: `code-owner-missing` allowing to indicate a code owner is missing.

The following changes could be made to our GitHub bot, supporting this ADR:

- In case the label `new-integration` or `new-platform` is added, or an update to the PR with that label took place; the bot should check if the contributor is set as a code owner. In case it does fail this check, the `code-owner-missing` must be added, or ensure absent otherwise.
- In case the label `code-owner-missing` is applied, the GitHub bot must add a failed build status, preventing the merging of the PR.
- In case the label `code-owner-missing` is removed, the failed build status needs to be adjusted to successful.

Furthermore, our developer documentation needs to be adjusted to document the contents of this ADR for our contributors.
