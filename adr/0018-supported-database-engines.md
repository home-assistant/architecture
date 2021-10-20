# 0020. Supported database engines

Date: 2021-10-20

## Status

TBD

## Context

We currently have no restrictions on supported database engines for the recorder, and the documentation states:

 > Home Assistant uses SQLAlchemy, which is an Object Relational Mapper (ORM). This means that you can use any 
 > SQL backend for the recorder that is supported by SQLAlchemy, like MySQL, MariaDB, PostgreSQL, or MS SQL Server.

This claim is not correct, SQLAlchemy will behave differently on different database engines, and features relied
on by the recorder may work differently, or not at all, in different database engines. 

As a matter of fact, we have a lot of special handling for the different databases out there, which makes
maintenance and implementing changes really hard. For contributors, it is impossible to test changes against
all database services SQLAlchemey could work with. This often causes bugs being reported for the less common
databases after a new release.

TL;DR: We advertise a lot of different databases being supported, without version limitation mentioned. This
causes the end-user to think their database and version of choice will work with Home Assistant. Unfortunately,
that is not always the case; The user experience for those using a less popular database engine or an outdated
version is very poor, with frequent issues preventing the recorder from working after an upgrade of Home Assistant.

## Decision

Support a limited set of database engines, and also limit the supported versions for the supported engines
in a similar way as we have limited installation types for Home Assistant. This greatly reduces the
maintenance effort, making it easier to implement and test new features and giving the end-user a better
experience with reliable upgrades.

Limit DB-support to the following:

- MariaDB ≥ 10.3 (2017)
- MySQL ≥ 8.0 (2018)
- PostgreSQL ≥ 12 (2019)
- SQLite ≥ 3.32.1 (2020)

Start to emit warnings about unsupported database engines in Home Assistant 2021.11, the recorder will fail
to start with Home Assistant 2022.2.

The above listed minimal versions may change over time. In case a minimal version bump is required, this will
be announced as a breaking change, with a depreciation period of 2 release cycles (2 months).


## Consequences

Existing users of database engines that are not in the supported list above, will have to move to a different
database solution. The recommendation is to use the built-in SQLite database when possible.
