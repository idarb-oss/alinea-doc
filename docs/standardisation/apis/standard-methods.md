---
title: Standard Methods
summary: The standarisation of API creation.
authors:
  - Idar Bergli
date: 2021-04-06
---

This chapter defines the concept of standard methods, which are `List`, `Get`, `Create`, `Update`, and `Delete`. Standard methods reduce complexity and increase consistency.

The following table describes how to map standard methods to HTTP methods:

| Standard Method | HTTP Mapping                | HTTP Request Body | HTTP Response Body        |
| --------------- | --------------------------- | ----------------- | ------------------------- |
| List            | GET <collection URL>        | N/A               | Resource\* list           |
| Get             | GET <resource URL>          | N/A               | Resource\*                |
| Create          | POST <collection URL>       | Resource          | Resource\*                |
| Update          | PUT or PATCH <resource URL> | Resource          | Resource\*                |
| Delete          | DELETE <resource URL>       | N/A               | Empty\*\*                 |

\* The resource returned from `List`, `Get`, `Create`, and `Update` methods **may** contain partial data if the methods support response field masks, which specify a subset of fields to be returned. In some cases, the API platform natively supports field masks for all methods.

\*\* The response returned from a `Delete` method that doesn't immediately remove the resource (such as updating a flag or creating a long-running delete operation) **should** contain either the long-running operation or the modified resource.

A standard method **may** also return a [long running operation](https://nothing-yet) for requests that do not complete within the time-span of the single API call.

The following sections describe each of the standard methods in detail.

## List

The `List` method takes a collection name and zero or more parameters as input, and returns a list of resources that match the input.

`List` is commonly used to search for resources. `List` is suited to data from a single collection that is bounded in size and not cached. For broader cases, the [custom method](custom-methods.md) `Search` **should** be used.

A batch get (such as a method that takes multiple resource IDs and returns an object for each of those IDs) **should** be implemented as a custom `BatchGet` method, rather than a `List`. However, if you have an already-existing `List` method that provides the same functionality, you **may** reuse the `List` method for this purpose instead. If you are using a custom BatchGet method, it **should** be mapped to HTTP GET.

Applicable common patterns: [pagination](design-paterns.md), [result ordering](design-paterns.md).

Applicable naming conventions: [filter field](naming-conventions.md), [results field](naming-conventions.md)

HTTP mapping:

- The `List` method **must** use an HTTP `GET` verb.
- The request message field(s) receiving the name of the collection whose resources are being listed **should** map to the URL path. If the collection name maps to the URL path, the last segment of the URL template (the [collection ID](resource-names.md)) **must** be literal.
- All remaining request message fields **shall** map to the URL query parameters.
- There is no request body; the API configuration **must not** declare a `body` clause.
- The response body **should** contain a list of resources along with optional metadata.

## Get

The `Get` method takes a resource name, zero or more parameters, and returns the specified resource.

HTTP mapping:

- The `Get` method **must** use an HTTP `GET` verb.
- The request message field(s) receiving the resource name **should** map to the URL path.
- All remaining request message fields **shall** map to the URL query parameters.
- There is no request body; the API configuration **must** not declare a `body` clause.
- The returned resource **shall** map to the entire response body.

## Create

The `Create` method takes a parent resource name, a resource, and zero or more parameters. It creates a new resource under the specified parent, and returns the newly created resource.

If an API supports creating resources, it **should** have a `Create` method for each type of resource that can be created.

HTTP mapping:

- The `Create` method **must** use an HTTP `POST` verb.
- The request message **should** have a field `parent` that specifies the parent resource name where the resource is to be created.
- The request message field containing the resource **must** map to the HTTP request body.
- The request **may** contain a field named `<resource>_id` to allow callers to select a client assigned id. This field **may** be inside the resource.
- All remaining request message fields **shall** map to the URL query parameters.
- The returned resource **shall** map to the entire HTTP response body.

If the `Create` method supports client-assigned resource name and the resource already exists, the request **should** either fail with error code `ALREADY_EXISTS` or use a different server-assigned resource name and the documentation **should** be clear that the created resource name may be different from that passed in.

The `Create` method **must** take an input resource, so that when the resource schema changes, there is no need to update both request schema and resource schema. For resource fields that cannot be set by the clients, they **must** be documented as "Output only" fields.

## Update

The `Update` method takes a request message containing a resource and zero or more parameters. It updates the specified resource and its properties, and returns the updated resource.

Mutable resource properties **should** be mutable by the `Update` method, except the properties that contain the resource's[name or parent](resource-names.md). Any functionality to rename or move a resource **must not** happen in the `Update` method and instead **shall** be handled by a custom method.

HTTP mapping:

- The standard `Update` method **should** support partial resource update, and use HTTP verb `PATCH` with a `FieldMask` field named `update_mask`. [Output fields](design-paterns.md) that are provided by the client as inputs should be ignored.
- An `Update` method that requires more advanced patching semantics, such as appending to a repeated field, **should** be made available by a [custom method](custom-methods.md).
- If the `Update` method only supports full resource update, it **must** use HTTP verb `PUT`. However, full update is highly discouraged because it has backwards compatibility issues when adding new resource fields.
- The message field receiving the resource name **must** map to the URL path. The field **may** be in the resource message itself.
- The request message field containing the resource **must** map to the request body.
- All remaining request message fields **must** map to the URL query parameters.
- The response message **must** be the updated resource itself.

If the API accepts client-assigned resource names, the server **may** allow the client to specify a non-existent resource name and create a new resource. Otherwise, the `Update` method **should** fail with non-existent resource name. The error code `NOT_FOUND` **should** be used if it is the only error condition.

An API with an `Update` method that supports resource creation **should** also provide a `Create` method. Rationale is that it is not clear how to create resources if the `Update` method is the only way to do it.

## Delete

The `Delete` method takes a resource name and zero or more parameters, and deletes or schedules for deletion the specified resource. The `Delete` method **should** return `Empty`.

An API **should not** rely on any information returned by a `Delete` method, as it **cannot** be invoked repeatedly.

HTTP mapping:

- The `Delete` method **must** use an HTTP `DELETE` verb.
- The request message field(s) receiving the resource name **should** map to the URL path.
- All remaining request message fields **shall** map to the URL query parameters.
- There is no request body; the API configuration **must not** declare a `body` clause.
- If the `Delete` method immediately removes the resource, it **should** return an empty response.
- If the `Delete` method initiates a long-running operation, it **should** return the long-running operation.
- If the `Delete` method only marks the resource as being deleted, it **should** return the updated resource.

Calls to the `Delete` method should be [idempotent](http://tools.ietf.org/html/rfc2616#section-9.1.2) in effect, but do not need to yield the same response. Any number of `Delete` requests **should** result in a resource being (eventually) deleted, but only the first request should result in a success code. Subsequent requests should result in a `NOT_FOUND`.
