**Abstract**

Join us for a full PostgreSQL training day, adjacent to the SCaLE LA event. This training runs as a full day single track with a 90 minute lunch break. Attendees can attend some or all of the event and still attend other Postgres or SCaLE talks depending on their schedule.

This training day is aimed both at new Postgres users and those migrating from other db systems. From start to finish - it will cover the full breadth of Postgres, from building a database and schema, to administrative tasks, and architecture.

**What**

A full PostgreSQL training day adjacent to the SCaLE LA event. 6 hours total, running for a full day as a single track (with 90 minute lunch break). Attendees can attend some or all of the event. Aimed both at new Postgres users and those migrating from other db systems.

**Why**

There’s a big need for education in our community. This is an initial experiment to see how this works and if it should be included in future events or changed.

**Logistics**

We will create some kind of repository of slides, sample data, and sample code so attendees can run exercises during the instruction time. However, this will not be done as a hands-on lab in real time - the logistics of that with so many Postgres distributions isn’t realistic. This repository will also be part of the community and could be reused for future community training days - to be run by anyone in the community (not just the authors of this particular series of talks). This idea is loosely based on the [Djangogirls](https://github.com/djangogirls) project - where content is easily reused, though the technical specifics are much different.

**Timeline**

Confirm outline and abstracts (for submission to SCaLE) Nov 1

Formalize repo and working logistics Dec 1

Draft content  Jan 1

Finish/polish content Feb 1

Time tests /adjustments March 1

Day March 5th

## PostgreSQL for the Absolute Beginner

Join us to hear about using Postgres for the very first time. If you’re new to Postgres, or just want to level set after trying lots of different things, we’ll start with the basics. We’ll have sample code to get started for users that want to try exercises hands on. If you plan to attend the full day training with us, start here. We’ll have 3 experts to make sure all questions are answered.

- Why Postgres?
- Running local Postgres vs other install options
- Connecting via GUI/psql
- psql intro
- Users and permissions
- Schemas
- Object and data types
- Extensions

## Getting Started with SQL in Postgres - Hour 2 of Postgres Training day

From basics to advanced, this course will get you comfortable with SQL functions in Postgres. If you’re attending the full day Postgres training, this is a great next step. Folks not attending the full day training are welcome to attend this comprehensive overview of SQL. Example code provided, experts available to answer questions.

- PostgreSQL dialect
- Basic SQL CRUD
- JOIN types
- Arrays, JSON
- Window functions
- CTEs
- plpgsql functions
- datatype 
    -- ids, UUIDs
    -- bigints
- schema design, best practices
- column layout, data type sizing 
- search and full text search

## Postgres DBA Basics - Hour 3 of Postgres Training Day

This session will review the basic jobs, terminology, and technical details for DBA tasks in Postgres. If you’re attending the full day Postgres training, this is an essential session. Folks not attending the full day training are welcome to attend. Example code provided, experts available to answer questions.

- (very) Brief review of Postgres hosting options
- Backups, dump/restore, basebackup, other backup tools
- Upgrades and versions
- DR & HA concepts and basics
- Logical Replication
- Connection management
- Disk / storage and vacuum
- Table partitioning

## Postgres Troubleshooting - Hour 4 of Postgres Training Day

This session will review essential troubleshooting for Postgres, reviewing how to monitor and log Postgres, using Postgres’ internal catalogs, and common problems and fixes. If you’re attending the full day Postgres training, this is an essential session. Folks not attending the full day training are welcome to attend. Example code provided, experts available to answer questions.

- Postgres’ internal catalog tables
- Logging
- Killing malicious actors
    - Statement timeout
    - Finding things in pg_stat_activity
- Monitoring
- Where to get help from the Postgres community

## Postgres Configuration and Performance Tuning - Hour 5 of Postgres Training Day

This session will review essential performance tuning and configurations for Postgres. If you’re attending the full day Postgres training, this is an essential session. Folks not attending the full day training are welcome to attend. Example code provided, experts available to answer questions.

- Postgres architecture and data movement
- Memory configs/tuning
    - Cache hit ratio
    - Shared buffers
    - Working memory
- I/O
- Vacuum
- Scaling out reads and writes

## Postgres Query Tuning - Hour 6 of Postgres Training Day

This session will review essential query performance tuning and for Postgres, an essential skill for developers working with Postgres daily. We’ll cover how to log query plans and where to add beneficial indexes to improve performance. If you’re attending the full day Postgres training, this is an essential session. Folks not attending the full day training are welcome to attend. Example code provided, experts available to answer questions.

- EXPLAIN
- Pg_stat_statements
- Indexes and sample use cases
- Other performance topics, joins, etc
