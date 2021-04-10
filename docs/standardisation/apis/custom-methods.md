---
title: Custom Methods
summary: The standarisation of API creation.
authors:
  - Idar Bergli
date: 2021-04-06
---

This chapter will discuss how to use custom methods for API designs.

Custom methods refer to API methods besides the 5 standard methods. They **should** only be used for functionality that cannot be easily expressed via standard methods. In general, API designers **should** choose standard methods over custom methods whenever feasible. Standard Methods have simpler and well-defined semantics that most developers are familiar with, so they are easier to use and less error prone. Another advantage of standard methods is the API platform has better understanding and support for standard methods, such as error handling, logging, monitoring.

A custom method can be associated with a resource, a collection, or a service. It **may** take an arbitrary request and return an arbitrary response, and also supports streaming request and response.

Custom method names **must** follow [method naming conventions](naming-conventions.md ).

## HTTP Mapping

For custom methods, they **should** use the following generic HTTP mapping:

```
https://service.name/v1/some/resource/name:customVerb
```

The reason to use `:` instead of `/` to separate the custom verb from the resource name is to support arbitrary paths. For example, undelete a file can map to `POST /files/a/long/file/name:undelete`

The following guidelines **shall** be applied when choosing the HTTP mapping:


- Custom methods **should** use HTTP `POST` verb since it has the most flexible semantics, except for methods serving as an alternative get or list which **may** use `GET` when possible. (See third bullet for specifics.)
- Custom methods **should not** use HTTP `PATCH`, but **may** use other HTTP verbs. In such cases, the methods **must** follow the standard [HTTP semantics](https://tools.ietf.org/html/rfc2616#section-9) for that verb.
- Notably, custom methods using HTTP `GET` **must** be idempotent and have no side effects. For example custom methods that implement special views on the resource **should** use HTTP `GET`.
- The request message field(s) receiving the resource name of the resource or collection with which the custom method is associated **should** map to the URL path.
- The URL path **must** end with a suffix consisting of a colon followed by the custom _verb_.
- If the HTTP verb used for the custom method allows an HTTP request body (this applies to `POST`, `PUT`, `PATCH`, or a custom HTTP verb), the HTTP configuration of that custom method **must** use the `body: "*"` clause and all remaining request message fields **shall** map to the HTTP request body.
- If the HTTP verb used for the custom method does not accept an HTTP request body (`GET`, `DELETE`), the HTTP configuration of such method **must not** use the `body` clause at all, and all remaining request message fields **shall** map to the URL query parameters.

!!! warning
    If a service implements multiple APIs, the API producer **must** carefully create the service configuration to avoid custom verb conflicts between APIs.

## Use Cases

Some additional scenarios where custom methods may be the right choice:

- **Batch methods.** For performance critical methods, it **may** be useful to provide custom batch methods to reduce per-request overhead.

A few examples where a standard method is a better fit than a custom method:

- Query resources with different query parameters (use standard `list` method with standard list filtering).
- Simple resource property change (use standard `update` method with field mask).
- Dismiss a notification (use standard `delete` method).

## Common Custom Methods

The curated list of commonly used or useful custom method names is below. API designers **should** consider these names before introducing their own to facilitate consistency across APIs.

| Method Name | Custom verb | HTTP verb | Note                                                                                                                       |
| ----------- | ----------- | --------- | -------------------------------------------------------------------------------------------------------------------------- |
| Cancel      | :cancel     | POST      | Cancel an outstanding operation, such as [operations.cancel]().                                                            |
| BatchGet    | :batchGet   | GET       | Batch get of multiple resources. See details in [the description of List](standard-methods.md).                            |
| Move        | :move       | POST      | Move a resource from one parent to another, such as [folders.move]().                                                      |
| Search      | :search     | GET       | Alternative to List for fetching data that does not adhere to List semantics, such as [services.search]().                 |
| Undelete    | :undelete   | POST      | Restore a resource that was previously deleted, such as [services.undelete](). The recommended retention period is 30-day. |
 |
