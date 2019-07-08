# 5. Code Formatting

Date: 2019-07-07

Architecture issue: [#249](https://github.com/home-assistant/architecture/issues/249)

## Status

Accepted

## Context

Proper code formatting is vital to the long-term viability and maintainability of a code
base. In place, a codebase with such formatting is:

- easier to review
- easier to diff against previous commits
- more likely to be free of linting errors

## Proposal

- We use [Black](https://github.com/python/black) as the de-facto code formatter for the entire back-end codebase
- We use Black's default options
- We utilize a bot (such as [the black_out bot](https://github.com/Mariatta/black_out)) to ensure PRs are appropriately formatted before being accepted

## Consequences

- Flake8: we set `max-line-length = 88` and `ignore = E501, W503` to `setup.cfg`.
- Pylint: we disable `bad-continuation` on `pylintrc`
