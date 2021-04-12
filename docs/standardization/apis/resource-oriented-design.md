---
title: Resource Oriented Design
summary: The standarisation of API creation.
authors:
  - Idar Bergli
date: 2021-04-06
---

The goal for this Design Guide is to help developers design **simple, consistent and easy-to-use** networked APIs.

The architectural style of REST was introduced, primarily designed to work well with HTTP/1.1, but also to help tackle this problem. Its core principle is to define named resources that can be manipulated using a small number of methods. The resources and methods are known as _nouns_ and _verbs_ of APIs. With the HTTP protocol, the resource names naturally map to URLs, and methods naturally map to HTTP methods POST, GET, PUT, PATCH, and DELETE. This results in much fewer things to learn, since developers can focus on the resources and their relationship, and assume that they have the same small number of standard methods.

On the Internet, HTTP REST APIs have been hugely successful. In 2010, about 74% of public network APIs were HTTP REST (or REST-like) APIs, most using JSON as the wire format.

While HTTP/JSON APIs are very popular on the Internet, the amount of traffic they carry is smaller than traditional RPC APIs. For example, about half of Internet traffic in America at peak time is video content, and few people would consider using HTTP/JSON APIs to deliver such content for obvious performance reasons. Inside data centers, many companies use socket-based RPC APIs to carry most network traffic, which can involve orders of magnitude more data (measured in bytes) than public HTTP/JSON APIs.

In reality, both RPC APIs and HTTP/JSON APIs are needed for various reasons, and ideally, an API platform should provide best support for all types of APIs. This Design Guide helps you design and build APIs that conform to this principle. It does so by applying resource-oriented design principles to general API design and defines many common design patterns to improve usability and reduce complexity.

!!! note
    This Design Guide explains how to apply REST principles to API designs independent of programming language, operating system, or network protocol. It is NOT a guide solely to creating REST APIs.

## What is a REST API?

A REST API is modeled as _collections_ of individually-addressable _resources_ (the _nouns_ of the API). Resources are referenced with their [resource names](resource-names.md) and manipulated via a small set of _methods_ (also known as verbs or operations).

_Standard methods_ for REST APIs (also known as REST methods) are `List`, `Get`, `Create`, `Update`, and `Delete`. _Custom methods_ (also known as _custom verbs_ or _custom operations_) are also available to API designers for functionality that doesn't easily map to one of the standard methods, such as database transactions.

!!! note
    Custom verbs does not mean creating custom HTTP verbs to support custom methods. For HTTP-based APIs, they simply map to the most suitable HTTP verbs.

## Design Flow

The Design Guide suggests taking the following steps when designing resource- oriented APIs (more details are covered in specific sections below):

- Determine what types of resources an API provides.
- Determine the relationships between resources.
- Decide the resource name schemes based on types and relationships.
- Decide the resource schemas.
- Attach minimum set of methods to resources.

## Resources

A resource-oriented API is generally modeled as a resource hierarchy, where each node is either a _simple resource_ or a _collection resource_. For convenience, they are often called a resource and a collection, respectively.

- A collection contains a list of resources of **the same type**. For example, a user has a collection of contacts.
- A resource has some state and zero or more sub-resources. Each sub-resource can be either a simple resource or a collection resource.

For example, Gmail API has a collection of users, each user has a collection of messages, a collection of threads, a collection of labels, a profile resource, and several setting resources.

While there is some conceptual alignment between storage systems and REST APIs, a service with a resource-oriented API is not necessarily a database, and has enormous flexibility in how it interprets resources and methods. For example, creating a calendar event (resource) may create additional events for attendees, send email invitations to attendees, reserve conference rooms, and update video conference schedules.

## Methods

The key characteristic of a resource-oriented API is that it emphasizes resources (data model) over the methods performed on the resources (functionality). A typical resource-oriented API exposes a large number of resources with a small number of methods. The methods can be either the standard methods or custom methods. For this guide, the standard methods are: `List`, `Get`, `Create`, `Update`, and `Delete`.

Where API functionality naturally maps to one of the standard methods, that method **should** be used in the API design. For functionality that does not naturally map to one of the standard methods, _custom methods_ **may** be used. [Custom methods](custom-methods.md) offer the same design freedom as traditional RPC APIs, which can be used to implement common programming patterns, such as database transactions or data analysis.

## Examples

The following sections present a few real world examples on how to apply resource-oriented API design to large scale services.

In these examples, the asterisk indicates one specific resource out of the list.

### RAW API

The RAW API service implements the RAW API and exposes most of RAW functionality. It has the following resource model:

API service: raw

- A collection of databases: projects/\*/databases/\*
    - A collection of tabels: projects/\*/databases/\*/tables/\*
        - A collection of row: projects/\*/databases/\*/tables/\*/rows/\*
    - A collection of labels: projects/\*/databases/\*/labels/\*
    - A collection of change history: projects/\*/databases/\*/history/\*

### Assets API

The Asset service implements the Asset API, which defines the following resource model:

API service: Assets

- A collection of assets: projects/\*/assets/\*
    - A collection of subscriptions: projects/\*/assets/\*/lables/\*

### Cloud Spanner API

The Data Set service implements the Data Set API, which defines the following resource model:

API service: dataset

- A collection of datasets: projects/\*/datasets/\*
  - Detail information about datasets: projects/\*/datasets/\*/detail/
  - A collection of columns: projects/\*/datasets/\*/labels/\*
