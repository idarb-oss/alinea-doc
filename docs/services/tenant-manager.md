---
title: Tenant Manager
summary: Description of the Tenant Manager service
authors:
  - Idar Bergli
date: 2021-04-12
---

!!! warning
    Under progress

The tenant-manager is a service that manges all the registered tenants in the system. The manager will be used to register the different customizastion points used for the _non-intrusive_ service created by a tenant.

The manager will have an database bassen on _ScyllaDB_ where everything is registered and where the standard services and check what extension points has been registered and the endpoints for where the events should be sendt.

The manager will provide an administrative API that can be used to add, remove or alter customizastion registrations without having to redeploy the Tenant Manager or accessing the database directly.

## Event Isolation

Isolation of events in an integral part of tenant isolation. That means that one tenant should not be able to access the data of another tenant in any shape or form.

The  Event  Bus  implementation  and  architecture  in  the  main  product  must  ensure  that  tenant isolation is still preserved. Instead of having one connection to a single event bus, there must be multiple connections, one per tenant. One example of such an event bus implementation is base on [NATS](https://nats.io/) that can make use of [accounts](https://docs.nats.io/nats-server/configuration/securing_nats/accounts). Another way if the solution should be running in Azure Cloud is to use [Service Bus]() where we have a topic per tenant. Or each tenant could have their own service bus, but that would increase the resource usage and cost.

This way allows us to easly seperate logicaly per tenant, and the permission can easily be set so that each tenant is only allowed to interact with their own account, topic or event bus. The tenant’s microservices, however, only need one single connection to theirevent bus. Because there are now multiple connections to different event buses in the main product, all  the  events  must  have  the  tenant  information  set,  so  that  the  main  product  knows  which event bus to use.

![event isolation sequence](../diagrams/)

## Model

