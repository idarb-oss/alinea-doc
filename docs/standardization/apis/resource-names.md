---
title: Resource Names
summary: The standarisation of API creation.
authors:
  - Idar Bergli
date: 2021-04-06
---

In _resource-oriented_ APIs, _resources_ are named _entities_ and _resource names_ are their identifiers. Each resource **must** have its own unique resource name. The resource name is made up of the ID of the resource itself, the IDs of any parent resources, and its API service name. We'll look at resource IDs and how a resource name is constructed below.

APIs should use scheme-less [URIs](http://tools.ietf.org/html/rfc3986) for resource names. They generally follow the REST URL conventions and behave much like network file paths. They can be easily mapped to REST URLs: see the [Standard Methods](standard-methods.md) section for details.

A _collection_ is a special kind of resource that contains a list of sub-resources of identical type. For example, a table is a collection of row resources. The resource ID for a collection is called collection ID.

The resource name is organized hierarchically using collection IDs and resource IDs, separated by forward slashes. If a resource contains a sub-resource, the sub-resource's name is formed by specifying the parent resource name followed by the sub-resource's ID - again, separated by forward slashes.

Example: A raw service has a collection of `databases`, where each database has a collection of `tables`:

| API Service Name  | Collection ID | Resource ID  | Collection ID | Resource ID |
| ----------------- | ------------- | ------------ | ------------- | ----------- |
| //raw.alinea.com/ | /databases    | /database-id | /tables       | /table-id   |

An API producer can choose any acceptable value for resource and collection IDs as long as they are unique within the resource hierarchy. You can find more guidelines for choosing appropriate resource and collection IDs below.

## Full Resource Name

A scheme-less [URI](http://tools.ietf.org/html/rfc3986) consisting of a [DNS-compatible](http://tools.ietf.org/html/rfc1035) API service name and a resource path. The resource path is also known as relative resource name. For example:

```
"//raw.alinea.com/databases/database1/tables/table2"
```

The API service name is for clients to locate the API service endpoint; it **may** be a fake DNS name for internal-only services. If the API service name is obvious from the context, relative resource names are often used.

## Relative Resource Name

A URI path ([path-noscheme](http://tools.ietf.org/html/rfc3986#appendix-A)) without the leading "/". It identifies a resource within the API service. For example:

```
"databases/database1/tables/table2"
```

## Resource ID

 resource ID typically consists of one or more non-empty URI segments ([segment-nz-nc](http://tools.ietf.org/html/rfc3986#appendix-A)) that identify the resource within its parent resource, see above examples. The non-trailing resource ID in a resource name must have exactly one URL segment, while the trailing resource ID in a resource name **may** have more than one URI segment. For example:

 | Collection ID | Resource ID         |
 | ------------- | ------------------- |
 | files         | source/py/parser.py |

 API services **should** use URL-friendly resource IDs when feasible. Resource IDs **must** be clearly documented whether they are assigned by the client, the server, or either. For example, file names are typically assigned by clients, while asset IDs are typically assigned by servers.

## Collection ID

A non-empty URI segment ([segment-nz-nc](http://tools.ietf.org/html/rfc3986#appendix-A)) identifying the collection resource within its parent resource, see above examples.

Because collection IDs often appear in the generated client libraries, they must conform to the following requirements:

- Must be valid C/C++ identifiers.
- Must be in plural form with lowerCamel case. If the term doesn't have suitable plural form, such as "evidence" and "weather", the singular form should be used.
- Must use clear and concise English terms.
- Overly general terms should be avoided or qualified. For example, rowValues is preferred to values. The following terms should be avoided without qualification:
    - elements
    - entries
    - instances
    - items
    - objects
    - resources
    - types
    - values

## Resource Name vs URL

While full resource names resemble normal URLs, they are not the same thing. A single resource can be exposed by different API versions, API protocols, or API network endpoints. The full resource name does not specify such information, so it must be mapped to a specific API version and API protocol for actual use.

To use a full resource name via REST APIs, it must be converted to a REST URL by adding the HTTPS scheme before the service name, adding the API major version before the resource path, and URL-escaping the resource path. For example:

```
// This is a database table resource name.
"//raw.alinea.com/projects/project1/databases/database1/tables/table123"

// This is the corresponding HTTP URL.
"https://raw.alinea.com/v2/projects/project1/databases/database1/tables/table123"
```

## Resource Name as String

Alinea APIs **must** represent resource names using plain strings, unless backward compatibility is an issue. Resource names **should** be handled like normal file paths. When a resource name is passed between different components, it must be treated as an atomic value and must not have any data loss.

For resource definitions, the first field **should** be a string field for the resource name.

For example:

```
GET projects/project1/databases/database1

response:
{
  dislay_name: database1
}
```

## Questions

### Q: Why not use resource IDs to identify a resource?

For any large system, there are many kinds of resources. To use resource IDs to identify a resource, we actually use a resource-specific tuple to identify a resource, such as `(bucket, object)` or `(user, album, photo)`. It creates several major problems:

- Developers have to understand and remember such anonymous tuples.
- Passing tuples is generally harder than passing strings.
- Centralized infrastructures, such as logging and access control systems, don't understand specialized tuples.
- Specialized tuples limit API design flexibility, such as providing reusable API interfaces. For example, [Long Running Operations](https://github.com/googleapis/googleapis/tree/master/google/longrunning) can work with many other API interfaces because they use flexible resource names.
