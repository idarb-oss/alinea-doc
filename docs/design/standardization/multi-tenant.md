---
title: Multi-tenant Application
summary: Alineas multi-tenant solution
authors:
  - Idar Bergli
date: 2021-04-01
---

!!! warning
    Under progress

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
  "tenant_name": "company.com",
  "database_connection": ["database1:9042", "database2:9042"],
  "secrets": {
    "db_user": { "server": "https://vault.com", "key": "secret-key" },
    "db_password": { "server": "https://vault.com", "key": "secret-key" }
  },
  "create_time": "2014-07-30T10:43:17Z",
  "update_time": "2014-07-30T10:43:17Z",
  "delete_time": "2016-07-30T10:43:17Z"
}
```

| Key                 | Type          | Description                                                                                        |
| ------------------- | ------------- | -------------------------------------------------------------------------------------------------- |
| tenant_name         | string        | The tenant name that is an valid DNS name (as per [RFC 1035](http://www.ietf.org/rfc/rfc1035.txt)) |
| database_connection | array[string] | An array of strings with database connection infor in `database_connection_string`                 |
| secrets             | object        | And object with key that is the name of the secret with an object with secret server and key name  |
| create_time         | Timestamp     | When the tenant was created                                                                        |
| update_time         | Timestamp     | When the tenant was updated                                                                        |
| delete_time         | Timestamp     | When the tenant was registered for deletion                                                        |

### Project

```json
{
  "project_name": "project1",
  "database_schema": "schema_name",
  "identity_provider": "identity_provider",
  "secrets": {
    "client_id": { "server": "https://vault.com", "key": "client-id" },
    "client_secret": { "server": "https://vault.com", "key": "client-secret" }
  },
  "description": "Project looking to solving of ....",
  "create_time": "2014-07-30T10:43:17Z",
  "update_time": "2014-07-30T10:43:17Z",
  "delete_time": "2016-07-30T10:43:17Z"
}
```

| Key               | Type      | Description                                                                                      |
| ----------------- | --------- | ------------------------------------------------------------------------------------------------ |
| project_name      | string    | The name of the project                                                                          |
| database_schema   | string    | The name of the schema/keyspace in the database cluster                                          |
| identity_provider | string    | Information about the identity provider for the tenant                                           |
| secrets           | object    | An object with key's that is the name of the secret with an object with secret location and name |
| description       | string    | A short description of the proejct                                                               |
| create_time       | Timestamp | When the project was created                                                                     |
| update_time       | Timestamp | When the project was updated                                                                     |
| delete_time       | Timestamp | When the project was registered for deletion                                                     |

### URL

The url specification will be as following to show the tenant in and what project

> https://[domain]/@[tenant_name]/projects/[project_name]/

or

> https://[tenant_name].domain/projects/[project_name]/
