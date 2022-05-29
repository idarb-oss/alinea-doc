---
title: Use DotNetCore and CSharp
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

As it is mainly a monolith, only one language must be selected for implementation.

## Decision

- .NET Core platform - it is new generation multi-platform, fully supported by Microsoft and open-source community, optimized and designed to replace old .NET Framework
- C# language - most popular language in .NET ecosystem

## Consequences

- Whole application will be implemented in C# object-oriented language in .NET Core framework
- .NET Core applications can be executed on Windows, MacOS, Linux
