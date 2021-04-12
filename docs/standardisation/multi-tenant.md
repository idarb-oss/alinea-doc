---
title: Multi-tenant Application
summary: Alineas multi-tenant solution
authors:
  - Idar Bergli
date: 2021-04-01
---

Altough designing for an multi-tenant solution might looke like an over kill, it is also a good excersise on how this type of solution can be done.

The multi-tenancy will be based on an `tenant` that can have multiple `projects`. In many cases a `tenant` can be similar to an enterprise or just a regular user that has theyr own setup and configuration. `Projects` are an seperation of individual parts innside an tenant.

## Explanation

The basic model for the multi-tenancy is a table storing the needed information on where the tenant data is located. In this solution we will be able to either use multiple databases where one tenant has theire own or collect multipe tenants in one database. The way we can do this is to give a tenant their own keyspace as seen in the figures.

![figure1](../diagrams/multi-tenant-figure1.svg)

**Figure1:** Shows tenants with their own database and keyspace

![figure2](../diagrams/multi-tenant-figure2.svg)

**Figure2:** Shows tenants in a commond database with their own keyspaces

## Models

So the diffrent models for the multi-tenant solutions is as following.

### Tenant Directory

```json
{
  "tenant": "company.com",
  "database_cluster": ["database1:9042", "database2:9042"],
  "keyspace": "keyspace_name",
  "secrets": {
    "db_user": { "server": "https://vault.com", "key": "secret-key" },
    "db_password": { "server": "https://vault.com", "key": "secret-key" }
  }
}
```

| Key              | Type          | Description                                                                                        |
| ---------------- | ------------- | -------------------------------------------------------------------------------------------------- |
| tenant           | string        | The tenant name that is an valid DNS name (as per [RFC 1035](http://www.ietf.org/rfc/rfc1035.txt)) |
| database_cluster | array[string] | An array of strings with database connection infor in <ip/dns>:<port>                              |
| keyspace         | string        | The name of the keyspace in the database cluster                                                   |
| secrets          | object        | And object with key that is the name of the secret with an object with secret server and key name  |

The url specification will be as following to show the tenant in use

> https://[domain]/@[tenant]/projects/[project_id]/

### Project

```json
{
  "id": "7dfb5f8a-a6f0-412c-8b61-320dc85b083d"
}
```

| Key | Type   | Description                   |
| --- | ------ | ----------------------------- |
| id  | string | The id of the project in UUID |
