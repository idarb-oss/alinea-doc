---
title: Alinea
summary:
authors:
  - Idar Bergli
date: 2021-03-28
---

# Alinea

!!! warning
    Alinea is under ideation


> "Beginning of a new way of thinking" or _a linea_, refering to &para; new paragraph.

Alinea is a collection of services to create a chassis for services. Tough this is a start of it all, we will end up with an data platform based on the [IBM best practise architecture](https://developer.ibm.com/articles/eda-and-microservices-architecture-best-practices/).

## What to solve

**User Access**

- [ ] _Tenant_ have own user management and access control for the system
- [ ] _Identity_ represetns users or clients that can access parts of the system
- [ ] _Roles_ represents collection of permissions and not permissions that identities can have
- [ ] _Permissions_ are fine graned permissions that identities can have to access systems 

**Configuration Management**

- [ ] _Clients_ (Services) with different configurations
- [ ] _Environment_ different clients is running in with different configurations
- [ ] _Clusters_ services can be running in different clusters and with that have different configurations
- [ ] _Section_ configuration can be divided into different sections with items
    - [ ] _Item_ belongs to sections wich are configurations for the client
- [ ] _Templates_ Create default templates that can be automaticaly created when creating new clients

**Feature Flagging**

- [ ] **Feature Flagging**: A common way to control features in youre environment, both for configuration and feature development

**Governance**

- [ ] **Policy Management**: How to fullfill governance by using policy control points

### External Services

**Networking**

- [ ] [_Consul_](https://www.hashicorp.com/products/consul) use consul to setup secure connections and servie discovery

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
