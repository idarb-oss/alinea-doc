---
title: Alinea
summary:
authors:
  - Idar Bergli
date: 2021-03-28
---

!!! warning
    Alinea is under ideation

> "Beginning of a new way of thinking" or _a linea_, refering to &para; new paragraph.

Alinea is trial to look into how to get an on-premise solution for data gathering / collection from devices in an industiral setting. Alltough we will here start by creating an `monolith` type of application it will have clear seperations of `bouded contextes`. This will make the possibility to split out and segregate the diffrent parts later if needed depengin on how certain services needs to scale.

As mentioned this project will _try_ to have clear and descriptive `bounded contextes` that which is a description comming from the `Domain Driven Design` terminology created by `Eric Evans`.
> **Bounded Context** explicitly define the context within which a model applies. Explicitly set boundaries in terms of team organization, usage within specific parts of the application, and physical manifestations such as code bases and database schemas. Apply Continuous Integration to keep model concepts and terms strictly consistent within these bounds, but don't be distracted or confused by issues outside. Standardize a singel development process withn the context, which need not be used elsewhere.

For more information on the process to find contextes and the driving design descision read more in [design]() part of this documentation space.

After a lot of thinking the following main programing lanauge will be used in this project and that is [.NET 5 C#](https://dotnet.microsoft.com/) alltough [Python](https://www.python.org/) will also be used in certain
situations. When it comes to the frontend the desision and been take to use [Svelte](https://svelte.dev/) as the web base solution alltough everything will be API base so any frontend can be created for the solution.

## Event Driven

[Event Driven](https://martinfowler.com/articles/201701-event-driven.html) architecture or patterns will be an central part of this system.

## What to solve

- **Multi-tenancy** how can be have a split diffrent parts in their own tenants.
- **Non-intrusive** microservice to enable deep customization in multi-tenancy.
- **Discovery** how do we learn about other nodes and services on the network.
- **Presence** how do we track when other nodes and services come and go.
- **Connectivity** how do we actually connect one node or service to another.
- **Distributed Logging** how do we track what this cloud of nodes/service is doing so we can detect performance problems and failures.

## License MIT

Copyright (c) 2021-2021 Idar Bergli

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
