---
title: Use Domain Driven Design Tactical Patterns
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

We decided create Rich Domain Models ([ADR #8](./0008-create-rich-domain-models.md)) for each module. We need to define or use some construction
elements / building blocks to implement our architecture and business logic.

## Decision

We decided to use **Domain Driven Design** tactical patterns. They focus on the Domain Model implementation. Especially we will use the following building blocks:

- Command - public method on Aggregate (behavior)
- Domain Event - the immutable class which represents important fact occurred on a special point of time (behavior)
- Entity - class with identity (identity cannot change) with mutable attributes which represents concept from domain
- Value Object - immutable class without an identity which represents concept from domain
- Aggregate - cluster of domain objects (Entities, Value Objects) with one class entry point (Entity as Aggregate Root) which defines the boundary of transaction/consistency and protects business rules and invariants
- Repository - collection-like abstraction to persist and load particular Aggregate
- Domain Service - stateless service to execute some business logic which does not belong to any of Entity/Value Object

## Consequences

- We need to define entities and value objects
- We need to define aggregates boundaries
- We need to add repositories for each aggregate
- We can invoke only public methods on Aggregate Roots, everything else should be hidden
- Developers need to be familiar with DDD tactical patterns
