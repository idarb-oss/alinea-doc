---
title: Naming Conventions
summary: The standarisation of API creation.
authors:
  - Idar Bergli
date: 2021-04-06
---

In order to provide consistent developer experience across many APIs and over a long period of time, all names used by an API **should** be:

- simple
- intuitive
- consistent

This includes names of interfaces, resources, collections, methods, and messages.

Since many developers are not native English speakers, one goal of these naming conventions is to ensure that the majority of developers can easily understand an API. It does this by encouraging the use of a simple, consistent, and small vocabulary when naming methods and resources.


- Names used in APIs **should** be in correct American English. For example, license (instead of licence), color (instead of colour).
- Commonly accepted short forms or abbreviations of long words **may** be used for brevity. For example, API is preferred over Application Programming Interface.
- Use intuitive, familiar terminology where possible. For example, when describing removing (and destroying) a resource, delete is preferred over erase.
- Use the same name or term for the same concept, including for concepts shared across APIs.
- Avoid name overloading. Use different names for different concepts.
- Avoid overly general names that are ambiguous within the context of the API and the larger ecosystem of Alinea APIs. They can lead to misunderstanding of API concepts. Rather, choose specific names that accurately describe the API concept. This is particularly important for names that define first-order API elements, such as resources. There is no definitive list of names to avoid, as every name must be evaluated in the context of other names. Instance, info, and service are examples of names that have been problematic in the past. Names chosen should describe the API concept clearly (for example: instance of what?) and distinguish it from other relevant concepts (for example: does "alert" mean the rule, the signal, or the notification?).
- Carefully consider use of names that may conflict with keywords in common programming languages. Such names **may** be used but will likely trigger additional scrutiny during API review. Use them judiciously and sparingly.

## Product names

Product names refer to the product marketing names of APIs, such as Alinea Raw API. Product names **must** be consistently used by APIs, UIs, documentation, etc. Alinea APIs **must** use product names approved by the product teams.

The table below shows examples of all related API names and their consistency. See further below on this page for more details on the respective names and their conventions.

| API Name         | Example                            |
| ---------------- | ---------------------------------- |
| Product Name     | Alinea Raw API                     |
| Service Name     | raw.alinea.io                      |
| Package Name     | alinea.raw.v3                      |
| Interface Name   | alinea.raw.v3.RawService           |
| Source Directory | //alinea/raw/v3                    |
| API Name         | raw                                |

## Service names

Service names **should** be syntactically valid DNS names (as per [RFC 1035](http://www.ietf.org/rfc/rfc1035.txt)) which can be resolved to one or more network addresses. The service names of public Alinea APIs follow the pattern: `xxx.alinea.io`. For example, the service name of the Alinea Raw is `raw.alinea.io`. If an API is composed of several services they **should** be named in a way to help discoverability. One way to do this is for the Service Names to share a common prefix.

## Package names

Package names declared in the API files **should** be consistent with Product Names and Service Names. Package names **should** use singular component names to avoid mixed singular and plural component names. Package names **must not** use underscores. Package names for versioned APIs **must** end with the version. For example:

```
// Alinea Raw API
package alinea.raw.v3;
```

An abstract API that isn't directly associated with a service, such as Alinea Watcher API, **should** use package names consistent with the Product name:

```
// Alinea Watcher API
package alinea.watcher.v1;
```

## Collection IDs

[Collection IDs](resource-names.md) should use plural form and `lowerCamelCase`, and American English spelling and semantics. For example: `events`, `children`, or `deletedEvents`.

## Interface names

To avoid confusion with [Service Names](naming-conventions.md) such as `raw.alinea.io`, the term interface name refers to the name used when defining a `service`.

You can think of the service name as a reference to the actual implementation of a set of APIs, while the interface name refers to the abstract definition of an API.

An interface name **should** use an intuitive noun such as Raw or Blob. The name **should not** conflict with any well-established concepts in programming languages and their runtime libraries (for example, File).

In the rare case where an interface name would conflict with another name within the API, a suffix (for example `Api` or `Service`) **should** be used to disambiguate.

## Message names

Message names **should** be short and concise. Avoid unnecessary or redundant words. Adjectives can often be omitted if there is no corresponding message without the adjective. For example, the `Shared` in `SharedProxySettings` is unnecessary if there are no unshared proxy settings.

Message names **should not** include prepositions (e.g. "With", "For"). Generally, message names with prepositions are better represented with optional fields on the message.

### Request and response messages

The request and response messages for RPC methods **should** be named after the method names with the suffix `Request` and `Response`, respectively, unless the method request or response type is:

- an empty message (use `Empty`),
- a resource type, or
- a resource representing an operation

This typically applies to requests or responses used in standard methods `Get`, `Create`, `Update`, or `Delete`.

## Enum names

Enum types **must** use UpperCamelCase names.

Enum values **must** use CAPITALIZED_NAMES_WITH_UNDERSCORES.

```
class FooBar(Enum):
    // The first value represents the default and must be == 0.
    FOO_BAR_UNSPECIFIED = 0;
    FIRST_VALUE = 1;
    SECOND_VALUE = 2;
```

## Field names

Field definitions **must** use lower_case_underscore_separated_names.

Field names **should not** include prepositions (e.g. "for", "during", "at"), for example:

- `reason_for_error` should instead be `error_reason`
- `cpu_usage_at_time_of_failure` should instead be `failure_time_cpu_usage`

Field names **should not** use postpositive adjectives (modifiers placed after the noun), for example:

- `items_collected` should instead be `collected_items`
- `objects_imported` should instead be `imported_objects`

### Repeated field names

Repeated fields in APIs **must** use proper plural forms. This matches the convention of existing Alinea APIs, and the common expectation of external developers.

### Time and Duration

To represent a point in time independent of any time zone or calendar, `ISO 8601` **should** be used, and the field name **should** end with `time`, such as `start_time` and `end_time`.

If the time refers to an activity, the field name **should** have the form of `verb_time`, such as `create_time`, `update_time`. Avoid using past tense for the verb, such as `created_time` or `last_updated_time`.

To represent a span of time between two points in time independent of any calendar and concepts like "day" or "month", `Duration` **should** be used.

If you have to represent time-related fields using an integer type for legacy or compatibility reasons, including wall-clock time, duration, delay and latency, the field names **must** have the following form:

```
xxx_{time|duration|delay|latency}_{seconds|millis|micros|nanos}
```

```
send_time_millis: int
receive_time_millis: int
```

If you have to represent timestamp using string type for legacy or compatibility reasons, the field names **should not** include any unit suffix. The string representation **should** use RFC 3339 format, e.g. "2014-07-30T10:43:17Z".

### Date and Time of Day

For dates that are independent of time zone and time of day, `Date` **should** be used and it **should** have the suffix `_date`. If a date must be represented as a string, it should be in the ISO 8601 date format YYYY-MM-DD, e.g. 2014-07-30.

For times of day that are independent of time zone and date, `TimeOfDay` **should** be used and should have the suffix `_time`. If a time of day must be represented as a string, it should be in the ISO 8601 24-hour time format HH:MM:SS[.FFF], e.g. 14:55:01.672.

### Quantities

Quantities represented by an integer type **must** include the unit of measurement.
```
xxx_{bytes|width_pixels|meters}
```
If the quantity is a number of items, then the field **should** have the suffix `_count`, for example `node_count`.

### List filter field

If an API supports filtering of resources returned by the `List` method, the field containing the filter expression **should** be named `filter`. For example:

```
class ListBooksRequest:
  // The parent resource name.
  parent: str

  // The filter expression.
  filter: str
```

### List response

The name of the field in the `List` method's response message, which contains the list of resources **must** be a plural form of the resource name itself.

## Name abbreviation

For well known name abbreviations among software developers, such as `config` and `spec`, the abbreviations **should** be used in API definitions instead of the full spelling. This will make the source code easy to read and write. In formal documentations, the full spelling **should** be used. Examples:

- config (configuration)
- id (identifier)
- spec (specification)
- stats (statistics)
