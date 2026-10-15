# Go Directory schema

This directory stores all of the [Go Directory Project](https://github.com/go-directory)-authored schema efinitions. Though perfectly legal per the syntax defined throughout [§ 4 of RFC 4512](https://datatracker.ietf.org/doc/html/rfc4512#section-4), they are not really intended for use elsewhere and are designed specifically to suit the objectives of the [Go Directory Project](https://github.com/go-directory).

## Custom eXtensions

This section contains information regarding the proprietary extensions used within the [Go Directory Project](https://github.com/go-directory) schema definitions.

### X-DO-NOT-ADVERTISE

 - Format: 'qdstrings' (single value only)
 - Value:  `BOOLEAN`

Example:

```
  X-DO-NOT-ADVERTISE 'TRUE'
```

Purpose:

According to [§ 4.4 of RFC 4512](https://datatracker.ietf.org/doc/html/rfc4512#section-4.4):

> Clients SHOULD NOT assume that a published subschema is complete, that the server supports all of the schema elements it publishes, or that the server does not support an unpublished element.

One take-away from this statement is that there may be portions of the schema not visible to those who are querying the subschemaSubentry. Indeed, many directory products have purely "internal" schema definitions that serve some kind of operational purpose _within_ the directory system, but are not really meant for end-user consumption.

But there is no formal means to "classifying" a particular definition as "hidden" or "internal". Of course, maintainers can just facilitate this classification privately within the code base, but this may be difficult to maintain.

As such, the [Go Directory Project](https://github.com/go-directory) chose to implement its own EXPERIMENTAL extension clause, `X-DO-NOT-ADVERTISE`, for assignment to so-called private definitions.

Definitions which bear this extension -- _and_ with a single value of `'TRUE'` -- will NOT be returned when the subschemaSubentry is targeted in a search request.

The definition, however, _will_ continue to function normally.

This means that, for example, an unadvertised attribute type can still be specified within the AVA of a search filter (or compare request), _and_ it can still be requested via attribute selection. Furthermore, if it is meant to be modified by the end-user, it can be. Nothing changes as a result of this feature, functionally speaking.

The benefits of using this extension are simply:

  - Keeping the advertised subschemaSubentry as uncluttered and "user-relevant" as possible
  - Helping [Go Directory Project](https://github.com/go-directory) maintainers more effectively keep track of special definition classifications
