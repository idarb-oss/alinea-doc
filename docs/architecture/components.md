---
title: Components
summary: Component selections in Alinea
authors:
  - Idar Bergli
date: 2021-04-09
---

Here we will document and list up the diffrent components not developed in Alinea that will be taken into use.

Some standard things will be used everywhere and that is:

- **[Docker](https://www.docker.com/)** for containerize.
- **[CloudEvents](https://cloudevents.io/)** for messaging.
- **[msgpack](https://msgpack.org/)** for binary serialization format.
- **[Open Telemetry](https://opentelemetry.io/)** for getting metrics and logs out of custom services.
- **[MkDocs](https://www.mkdocs.org/)** for all documentations with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) as common theme. If needed for a diffrent documentation system [sphinx](https://www.sphinx-doc.org/en/master/) can be taken into use.

## Database

Databases that will be and might be used in Alinea

### NoSQL

- **[ScyllaDB](https://www.scylladb.com/)** will be the main database used for services created.

### SQL

- **[SQLite](https://www.sqlite.org/index.html)** some services can be using the SQLite database in the background.
- **[PostgreSQL](https://www.postgresql.org/)** will be used if a SQL database is needed in certain situations.

### Graph

- **[JanusGraph](https://janusgraph.org/)** there might be some experimental with Graph. Since _ScyllaDB_ will be the main NoSQL storage used JanusGraph fits perfectly inn with that. JanusGraph uses the [gremlin](https://tinkerpop.apache.org/) query language.

## Messaging

- **[ZeroMQ](https://zeromq.org/)** will be the main messaging libary used in Alinea.
- **[MQTT](https://mqtt.org/)** is an popular IoT messaging standard used many places. This will be taken into use in Alinea for external types of streams and connections. Tough internal messaging will be what is described in [Event Backbone](#event backbone).
- **[NATS](https://nats.io/)** if we need a broker inbetween and _ZeroMQ_ is not enough _NATS_ will be looked into.

## Serializing

- **[Blosc](https://www.blosc.org/) super fast library for serializing data.
- **[msgpack](https://msgpack.org/) efficient binary serializing format.

## Search

- **[MeiliSearch](https://www.meilisearch.com/)** will be the main service used for searchable parts in Alinea.

## Web & APIs

The diffrent components that we will standarize on for web and API development.

### Gateway

- **[Ambassador](https://www.getambassador.io/)** to be used as a API gateway.

### Backends

- **[Fastapi](https://fastapi.tiangolo.com/)** will be the main API development framework for [python](https://www.python.org/).

### Web development

- **[Typescript]()** will be the go to language used for the web.
- **[React](https://reactjs.org/)** or **[Angular](https://angular.io/)** will be the main JavaScript library used for web frontend development.

## Security

Diffrent components used for security

### Security and Identity

- **[SPIRE](https://spiffe.io/docs/latest/spire-about/)** that is based on [SPIFFE](https://spiffe.io/)
  > A universal identity control plane for distributed systems

### Secret Store

- **[Vault](https://www.hashicorp.com/products/vault)** will be the secret store to be used when not running in the cloud.
- **[Key Vault](https://azure.microsoft.com/nb-no/services/key-vault/)** will be the used secret store used in Azure Cloud.

### Networking

- **[Consule](https://www.hashicorp.com/products/consul)** used for dicovery and to connect securily across any cloud runtimes.

## Automation

- **[Terraform](https://www.hashicorp.com/products/terraform)** will be the go to infrastructure as code if the need is there.
