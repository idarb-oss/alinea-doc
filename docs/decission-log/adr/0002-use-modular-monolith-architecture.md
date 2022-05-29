---
title: Use Modular Monolith Architecture
summary:
authors:
  - Idar Bergli
date: 2022-05-28
---

|        |            |
|--------|------------|
| Status | Accepted   |
| Date   | 2022-05-28 |

## Context

Prepare runtime of service to run as an monolith bur prepare them to be split up into microservices. DDD will be done for each service.

## Decision

Create nontrivial modular monolith architecture and Domain Driven Design patterns.

## Consequences

- All modules must run in one single process as single application (Monolith)
- All modules must be able to be split up into separate microservices when needed
- DDD Bounded Contexts will be used to divide services into modules
- DDD tactical patterns will be used to implement modules

## References

TBD
