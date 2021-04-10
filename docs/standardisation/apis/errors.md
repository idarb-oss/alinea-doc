---
title: Errors
summary: The standarisation of API creation.
authors:
  - Idar Bergli
date: 2021-04-06
---

This chapter provides an overview of the error model for Alinea APIs. It also provides general guidance to developers on how to properly generate and handle errors.

Alinea APIs use a simple protocol-agnostic error model, which allows us to offer a consistent experience across different APIs, different API protocols (such as gRPC or HTTP), and different error contexts (such as asynchronous, batch, or workflow errors).

## Error Model

The error model for Alinea APIs is logically defined by [`Status`](), an instance of which is returned to the client when an API error occurs. The following code snippet shows the overall design of the error model:

```
from pydantic import BaseModel

class Status(BaseModel):
    code: int
    message: str
    details: dict
```

Because most Alinea APIs use resource-oriented API design, the error handling follows the same design principle by using a small set of standard errors with a large number of resources. For example, instead of defining different kinds of "not found" errors, the server uses one standard `NOT_FOUND` error code and tells the client which specific resource was not found. The smaller error space reduces the complexity of documentation, affords better idiomatic mappings in client libraries, and reduces client logic complexity while not restricting the inclusion of actionable information.

## Error Codes

Alinea APIs **must** use the canonical error codes defined by [`Code`](). Individual APIs **must** avoid defining additional error codes, since developers are very unlikely to write logic to handle a large number of error codes. For reference, handling an average of 3 error codes per API call would mean most application logic would just be for error handling, which would not be a good developer experience.

## Error Messages

The error message should help users **understand and resolve** the API error easily and quickly. In general, consider the following guidelines when writing error messages:

- Do not assume the user is an expert user of your API. Users could be client developers, operations people, IT staff, or end-users of apps.
- Do not assume the user knows anything about your service implementation or is familiar with the context of the errors (such as log analysis).
- When possible, error messages should be constructed such that a technical user (but not necessarily a developer of your API) can respond to the error and correct it.
- Keep the error message brief. If needed, provide a link where a confused reader can ask questions, give feedback, or get more information that doesn't cleanly fit in an error message. Otherwise, use the details field to expand.

!!! warning
    Error messages are not part of the API surface. They are subject to changes without notice. Application code **must not** have a hard dependency on error messages.

## Error Details

Alinea APIs define a set of standard error payloads for error details, which you can find in [error_details](). These cover the most common needs for API errors, such as quota failure and invalid parameters. Like error codes, developers should use these standard payloads whenever possible.

Additional error detail types should only be introduced if they can assist application code to handle the errors. If the error information can only be handled by humans, rely on the error message content and let developers handle it manually rather than introducing additional error detail types.

Here are some example `error_details` payloads:

- `ErrorInfo`: Provides structured error information that is both **stable** and **extensible**.
- `RetryInfo`: Describes when clients can retry a failed request, may be returned on `Code.UNAVAILABLE` or `Code.ABORTED`
- `BadRequest`: Describes violations in a client request, may be returned on `Code.INVALID_ARGUMENT`

## Error Info

[`ErrorInfo`]() is a special kind of error payload. It provides **stable and extensible** error information that both humans and applications can depend on. Each `ErrorInfo` has 3 pieces of information: an error domain, an error reason, and a set of error metadata, such as this [example](). For more information, see the [`ErrorInfo`]() definition.

For Alinea APIs, the primary error domain is `alinea.io`, and the corresponding error reasons are defined by `ErrorReason` enum. For more information, see the [`ErrorReason`]() definition.

### Error Localization

The `message` field in [Status]() is developer-facing and **must** be in English.

If a user-facing error message is needed, use [LocalizedMessage]() as your details field. While the message field in [LocalizedMessage]() can be localized, ensure that the message field in [Status]() is in English.

By default, the API service should use the authenticated user’s locale or HTTP `Accept-Language` header or the `language_code` parameter in the request to determine the language for the localization.

## Handling Errors

Below is a table containing all of the error codes defined in [Code]() and a short description of their cause. To handle an error, you can check the description for the returned status code and modify your call accordingly.

| HTTP | gRPC                | Description                                                                                                                                                                                                                                                         |
| ---- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200  | OK                  | No error.                                                                                                                                                                                                                                                           |
| 400  | INVALID_ARGUMENT    | Client specified an invalid argument. Check error message and error details for more information.                                                                                                                                                                   |
| 400  | FAILED_PRECONDITION | Request can not be executed in the current system state, such as deleting a non-empty directory.                                                                                                                                                                    |
| 400  | OUT_OF_RANGE        | Client specified an invalid range.                                                                                                                                                                                                                                  |
| 401  | UNAUTHENTICATED     | Request not authenticated due to missing, invalid, or expired OAuth token.                                                                                                                                                                                          |
| 403  | PERMISSION_DENIED   | Client does not have sufficient permission. This can happen because the OAuth token does not have the right scopes, the client doesn't have permission, or the API has not been enabled.                                                                            |
| 404  | NOT_FOUND           | A specified resource is not found.                                                                                                                                                                                                                                  |
| 409  | ABORTED             | Concurrency conflict, such as read-modify-write conflict.                                                                                                                                                                                                           |
| 409  | ALREADY_EXISTS      | The resource that a client tried to create already exists.                                                                                                                                                                                                          |
| 499  | CANCELLED           | Request cancelled by the client.                                                                                                                                                                                                                                    |
| 500  | DATA_LOSS           | Unrecoverable data loss or data corruption. The client should report the error to the user.                                                                                                                                                                         |
| 500  | UNKNOWN             | Unknown server error. Typically a server bug.                                                                                                                                                                                                                       |
| 500  | INTERNAL            | Internal server error. Typically a server bug.                                                                                                                                                                                                                      |
| 501  | NOT_IMPLEMENTED     | API method not implemented by the server.                                                                                                                                                                                                                           |
| 502  | N/A                 | Network error occurred before reaching the server. Typically a network outage or misconfiguration.                                                                                                                                                                  |
| 503  | UNAVAILABLE         | Service unavailable. Typically the server is down.                                                                                                                                                                                                                  |
| 504  | DEADLINE_EXCEEDED   | Request deadline exceeded. This will happen only if the caller sets a deadline that is shorter than the method's default deadline (i.e. requested deadline is not enough for the server to process the request) and the request did not finish within the deadline. |


!!! warning
    Alinea APIs may concurrently check multiple preconditions for an API request. Returning one error code does not imply other preconditions are satisfied. Application code **must not** depend on the ordering of precondition checks.

### Retrying Errors

Clients **may** retry on 503 UNAVAILABLE errors with exponential backoff. The minimum delay should be 1s unless it is documented otherwise. The default retry repetition should be once unless it is documented otherwise.

For 429 RESOURCE_EXHAUSTED errors, the client may retry at the higher level with minimum 30s delay. Such retries are only useful for long running background jobs.

For all other errors, retry may not be applicable. First ensure your request is idempotent, and see [RetryInfo]() for guidance.

### Propagating Errors

If your API service depends on other services, you should not blindly propagate errors from those services to your clients. When translating errors, we suggest the following:

- Hide implementation details and confidential information.
- Adjust the party responsible for the error. For example, a server that receives an `INVALID_ARGUMENT` error from another service should propagate an `INTERNAL` to its own caller.

### Reproducing Errors

If you cannot resolve errors through analysis of logs and monitoring, you should try to reproduce the errors with a simple and repeatable test. You can use the test to collect more information for troubleshooting, which you can provide when contacting technical support.

We recommend you use [oauth2l](https://github.com/google/oauth2l) and [curl -v](https://curl.se/docs/manpage.html) and [System Parameters]() to reproduce errors with Alinea APIs. Together they can reproduce almost all Alinea API requests, and provide you verbose debug information. For more information, see the respective documentation pages for the API you are calling.

!!! note
    For troubleshooting, you should include **&$.xgafv=2** in your request URLs to select error format v2. Some Alinea APIs use error format v1 by default for compatibility reasons.

## Generating Errors

If you are a server developer, you should generate errors with enough information to help client developers understand and resolve the problem. At the same time, you must be aware of the security and privacy of the user data, and avoid disclosing sensitive information in the error message and error details, since errors are often logged and may be accessible by others. For example, an error message like "Client IP address is not on allowlist 128.0.0.0/8" exposes information about the server-side policy, which may not be accessible to the user who has access to the logs.

To generate proper errors, you first need to be familiar with [Code]() to choose the most suitable error code for each error condition. A server application may check multiple error conditions in parallel, and return the first one.

The following table lists each error code and an example of a good error message.

| HTTP | gRPC                | Example Error Message                                           |
| ---- | ------------------- | --------------------------------------------------------------- |
| 400  | INVALID_ARGUMENT    | Request field x.y.z is xxx, expected one of [yyy, zzz].         |
| 400  | FAILED_PRECONDITION | Resource xxx is a non-empty directory, so it cannot be deleted. |
| 400  | OUT_OF_RANGE        | Parameter 'age' is out of range [0, 125].                       |
| 401  | UNAUTHENTICATED     | Invalid authentication credentials.                             |
| 403  | PERMISSION_DENIED   | Permission 'xxx' denied on resource 'yyy'.                      |
| 404  | NOT_FOUND           | Resource 'xxx' not found.                                       |
| 409  | ABORTED             | Couldn’t acquire lock on resource ‘xxx’.                        |
| 409  | ALREADY_EXISTS      | Resource 'xxx' already exists.                                  |
| 499  | CANCELLED           | Request cancelled by the client.                                |
| 500  | DATA_LOSS           | See note.                                                       |
| 500  | UNKNOWN             | See note.                                                       |
| 500  | INTERNAL            | See note.                                                       |
| 501  | NOT_IMPLEMENTED     | Method 'xxx' not implemented.                                   |
| 503  | UNAVAILABLE         | See note.                                                       |
| 504  | DEADLINE_EXCEEDED   | See note.                                                       |

!!! note
    Since the client cannot fix the server error, it is not useful to generate additional error details. To avoid leaking sensitive information under error conditions, it is recommended not to generate any error message and only generate **DebugInfo** error details. The **DebugInfo** is specially designed only for server-side logging, and **must** not be sent to client.

### Error Payloads

The `alinea-core` package defines a set of standard error payloads, which are preferred to custom error payloads. The following table lists each error code and its matching standard error payload, if applicable. We recommend advanced applications look for these error payloads in `Status` when they handle errors.

| HTTP | gRPC                | Recommended Error Detail       |
| ---- | ------------------- | ------------------------------ |
| 400  | INVALID_ARGUMENT    | google.rpc.BadRequest          |
| 400  | FAILED_PRECONDITION | google.rpc.PreconditionFailure |
| 400  | OUT_OF_RANGE        | google.rpc.BadRequest          |
| 401  | UNAUTHENTICATED     | google.rpc.ErrorInfo           |
| 403  | PERMISSION_DENIED   | google.rpc.ErrorInfo           |
| 404  | NOT_FOUND           | google.rpc.ResourceInfo        |
| 409  | ABORTED             | google.rpc.ErrorInfo           |
| 409  | ALREADY_EXISTS      | google.rpc.ResourceInfo        |
| 499  | CANCELLED           |                                |
| 500  | DATA_LOSS           | google.rpc.DebugInfo           |
| 500  | UNKNOWN             | google.rpc.DebugInfo           |
| 500  | INTERNAL            | google.rpc.DebugInfo           |
| 501  | NOT_IMPLEMENTED     |                                |
| 503  | UNAVAILABLE         | google.rpc.DebugInfo           |
| 504  | DEADLINE_EXCEEDED   | google.rpc.DebugInfo           |
