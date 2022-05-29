---
title: Decision Log
summary: Decisions taken for Alinea
authors:
  - Idar Bergli
date: 2021-04-08
---

This is Alinea's Decision Logs and requirement documentation.

## Naming and Formatting

Decision Logs are requested to follow RFC (request for comments) naming standard. Specifically, authors should name their documents with a sequentially increasing integer (or serial number) and then the topic: (sequence number - topic). Example: 0001-SeparateConfigurationInterface. The sequence is a global sequence for all Alinea decisions. Per RFC and Michael Nygard suggestions the makeup of the ADR document should generally include:

- Title
- Status (proposed, accepted, rejected, deprecated, superseded, etc.)
- Context and Proposed Design
- Decision
- Consequences/considerations
- References
- Document history is maintained via Github history.

## Decision Logs

| Name                                                                                                              | Short Description                                                 |
|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| [0001-record-architecture-decisions](./adr/0001-record-architecture-decisions.md)                                 | Architecture decision should be recorded                          |
| [0002-use-modular-monolith-architecture](./adr/0002-use-modular-monolith-architecture.md)                         | Create monolith runtimes, but prepare to split into microservices |
| [0003-use-dotnetcore-and-csharp](./adr/0003-use-dotnetcore-and-csharp.md)                                         | Use dotnet and c# as the main language for services               |
| [0004-create-facade-between-api-and-business-module](./adr/0004-create-facade-between-api-and-business-module.md) | All modules should implement an façade interface                  |
| [0005-use-cqrs-architectural-style](./adr/0005-use-cqrs-architectural-style.md)                                   | Implementation should use command and query principals            |
| [0006-allow-return-result-after-command-processing](./adr/0006-allow-return-result-after-command-processing.md)   | Commands can return results                                       |
| [0007-use-feature-slicing](./adr/0007-use-feature-slicing.md)                                                     | Software architecture should follow feature slicing               |
| [0008-create-rich-domain-models](./adr/0008-create-rich-domain-models.md)                                         | Domain Driven Design must be used for all modules                 |
| [0009-use-domain-driven-design-tactical-patterns](./adr/0009-use-domain-driven-design-tactical-patterns.md)       | Use common design principles in software development for DDD      |
| [0010-protect-business-invariants-using-exceptions](./adr/0010-protect-business-invariants-using-exceptions.md)   | Exceptions must be used to protect invariants errors              |
| [0011-event-driven-communication-between-modules](./adr/0011-event-driven-communication-between-modules.md)       | Event driven architecture must be used between modules            |
