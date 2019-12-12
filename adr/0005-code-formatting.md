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

- We use [Black](https://github.com/python/black) as code formatter for the back-end codebase.
- We use Black's default options.
- We CI to ensure PRs are appropriately formatted before being accepted.
- We recommend and document the setup of `Black`, `pylint`, `flake8`, and `isort` in the development environment.

## Consequences

- We retain Flake8, but set `max-line-length = 88` and `ignore = E501, W503, E203` to `setup.cfg`.
- We retain pylint, but disable all formatting checks and `wrong-import-order`. The import order is handled by `isort`.
