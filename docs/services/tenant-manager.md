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

## Model

