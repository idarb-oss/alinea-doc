---
title: Multi-tenant Application
summary: Alineas multi-tenant solution
authors:
  - Idar Bergli
date: 2021-04-01
---

Altough designing for an multi-tenant solution might looke like an over kill, it is also a good excersise on how this type of solution can be done.

The multi-tenancy will be based on an `tenant` that can have multiple `projects`. In many cases a `tenant` can be similar to an enterprise or just a regular user that has theyr own setup and configuration. `Projects` are an seperation of individual parts innside an tenant.

## Model

The basic model for the multi-tenancy is a table storing the needed information on where the tenant data is located. In this solution we will be able to either use multiple databases where one tenant has theire own or collect multipe tenants in one database. The way we can do this is to give a tenant their own keyspace. See the difference in figure1.


