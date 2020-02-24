# Lightweight RPC and PubSub Message Protocol (LRPM Protocol)

The LRPM Protocol is inspired by WAMP (Web Application Messaging Protocol),
however is heavily stripped down to be more generic with more application 
specific details.

## Version

The current version is undefined as this is still a draft.

[Semantic versioning](https://semver.org) will be used.

## Session states

| Stage  | Description               |
| ------ | ------------------------- |
| `INIT` | Session is initialising   |
| `AUTH` | Session is authenticating |
| `OKAY` | Session is established    |

## Message Definitions

### URI definitions

| URI              | Description             |
| ---------------- | ----------------------- |
| `.err.auth`      | Bad auth error occurred |
| `.err.protocol`  | Protocol error occurred |
| `.err.cancelled` | Request was cancelled   |
| `.bye.normal`    | Normal closing event    |

### Basic Types

Any encoding method used must support these basic types.
A basic mapped type uses the most specific type in its respective encoding.
Types that incorrectly map from a less specific type to a specific type 
should cause a fatal error, destroying the session.

| Type   | Mapping                              |
| ------ | ------------------------------------ |
| `U8`   | An unsigned 8-bit integer.           |
| `U64`  | An unsigned 64-bit integer.          |
| `Str`  | An ASCII encoded string               |
| `Map`  | A `Str` key to `Val` structure       |
| `Val`  | A generic value tagged with its type |

### Special Types

Special types build upon the basic types.

#### `Id` (`U64`)

An `Id` represents a single request unique within a session.

#### `Uri` (`Str`)

A `Uri` represents a resource unique across all sessions.

- `Uri`s that start with a `.` are reserved for LRPMP use.
- `Uri`s are limited to the characters `_`, `.`, `*`, `a-z` and `0-9`.
- `Uri`s can not have either `*` or `.` in a sequence of more than one (eg `**`, `..`).

| Character         | Usage                                         |
| ----------------- | --------------------------------------------- |
| `.`               | Represents a namespace seperator              |
| `*`               | Represents a wildcard (only valid for topics) |
| `_`, `a-z`, `0-9` | Valid characters for identifiers              |

#### `Kind` (`U8` or `Str`)

A `Kind` represents a message kind (eg, `CALL`, `40`).

- `Kind`s in `Str` form are limited to the characters `A-Z` and `_`.

| Range       | Description          |
| ----------- | -------------------- |
| `001 - 019` | Session messages     |
| `020 - 039` | Generic messages     |
| `040 - 059` | RPC messages         |
| `060 - 079` | PubSub messages      |
| `080 - 126` | Reserved             |
| `127 - 255` | Application specific |

#### `Meta` (`Map`)

A `Meta` structure is arbitrary map of optional additional information.

#### `Body` (`Val`)

A `Body` is an application specific value.

### Messages

Messages are seperated into four groups:

| Group      | Description                                                        |
| ---------- | ------------------------------------------------------------------ |
| `INIT`     | Messages used to establish a session                               |
| `SESSION`  | Messages specific to a session, but not tied to a request          |
| `REQUEST`  | Messages with a request ID that require a response                 |
| `RESPONSE` | Messages responding to a request                                   |

- Message fields are ordered as defined below.
- Messages defined only as `INIT` must not be sent when a session is in the `OKAY` state.
- Messages defined only as `SESSION`, `REQUEST` or `RESPONSE` must not be sent when the 
  session is in the `INIT` or `AUTH` state.

Messages are defined below with the format of:

```plain
(<str-kind>) Message (<u8-kind>, <message-group>)
```

#### `GOODBYE` Message (`01`, `INIT` or `SESSION`)

`GOODBYE` messages can be sent in both the `INIT` and `SESSION` 
stages of a connection.

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `reason`          | `Uri`  | A URI uniquely describing the close reason         |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `HELLO` Message (`02`, `INIT`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `body`            | `Body` | The body of the message                            |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `PROVE` Message (`03`, `INIT`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `body`            | `Body` | The body of the message                            |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `PROOF` Message (`04`, `INIT`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `body`            | `Body` | The body of the message                            |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `ERROR` Message (`20`, `RESPONSE`)

Error messages are a generic response to a request.

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `request_kind`    | `Kind` | The request kind that triggered the error          | 
| `request_id`      | `Id`   | The request ID that triggered the error            |
| `error`           | `Uri`  | A URI uniquely describing the error                |
| `body`            | `Body` | Body of the error message                          |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `CANCEL` Message (`21`, `REQUEST`)

Cancel messages will trigger an `ERROR` response if successful.

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `request_id`      | `Id`   | The ID of the request we want to cancel            |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `CALL` Message (`40`, `REQUEST`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `request_id`      | `Id`   | An ID uniquely describing the call request         |
| `procedure`       | `Uri`  | A URI uniquely describing the procedure            |
| `body`            | `Body` | The body of the message                            |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `RESULT` Message (`41`, `RESPONSE`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `request_id`      | `Id`   | The ID of the call we are responding to            |
| `body`            | `Body` | The successful body result from the call           |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `EVENT` Message (`60`, `SESSION`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `publication_id`  | `Id`   | The publication ID                                 |
| `subscription_id` | `Id`   | The subscription ID                                |
| `body`            | `Body` | The body of the event                              |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `PUBLISH` Message (`61`, `REQUEST`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `request_id`      | `Id`   | An ID uniquely describing the publication request  |
| `topic`           | `Uri`  | A URI describing the topic we are publishing to    |
| `body`            | `Body` | The body of the event being published              |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `PUBLISHED` Message (`62`, `RESPONSE`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `request_id`      | `Id`   | The ID of the publication request                  |
| `publication_id`  | `Id`   | An ID uniquely describing the publication          |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `SUBSCRIBE` Message (`63`, `REQUEST`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `request_id`      | `Id`   | An ID uniquely describing the subscription request |
| `topic`           | `Uri`  | A URI describing the topic we are subscribing to   |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `SUBSCRIBED` Message (`64`, `RESPONSE`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `request_id`      | `Id`   | The ID of the subscription request                 |
| `subscription_id` | `Id`   | An ID uniquely describing the subscription         |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `UNSUBSCRIBE` Message (`65`, `REQUEST`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `request_id`      | `Id`   | An ID uniquely describing the unsubscribe request  |
| `subscription_id` | `Id`   | The subscription ID we are unsubscribing from      |
| `meta`            | `Meta` | Optional meta information on this message          |

#### `UNSUBSCRIBED` Message (`66`, `RESPONSE`)

| Field             | Type   | Description                                        |
| ----------------- | ------ | -------------------------------------------------- |
| `request_id`      | `Id`   | The ID of the unsubscribe request                  |
| `meta`            | `Meta` | Optional meta information on this message          |

## JSON, CBOR Message Frame

For JSON and supersets, messages are encoded as an array.

The first element is the message kind, and the following elements are the message fields.

### Example (JSON):

```json
["CALL", 13, "helloworld", "payload", {}]
```

**OR**

```json
[40, 13, "helloworld", "payload", {}]
```

### Basic Type Mappings

A basic mapped type uses the most specific type in its respective encoding.
Types that incorrectly map from a less specific type to a specific type should error.

For example in JSON, a `U8` and `U64` will map to a JSON number, while in CBOR
a `u8` will map to a `u8` and a `u64` to a `u64`.

#### JSON Mapping

| Type   | Mapping      |
| ------ | ------------ |
| `U8`   | JSON number  |
| `U64`  | JSON number  |
| `Str`  | JSON string  |
| `Map`  | JSON object  |
| `Seq`  | JSON array   |
| `Val`  | JSON value   |

#### CBOR Mapping

| Type   | Mapping                         |
| ------ | ------------------------------- |
| `U8`   | CBOR u8                         |
| `U64`  | CBOR u64                        |
| `Str`  | CBOR string                     |
| `Map`  | CBOR object (with `ASCII` keys) |
| `Seq`  | CBOR array                      |
| `Val`  | CBOR data item                  |
