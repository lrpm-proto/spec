# Types

## Basic Types

Any encoding method used must support these basic types.
A basic mapped type uses the most specific type in its respective encoding.
Types that incorrectly map from a less specific type to a specific type 
should cause a fatal error, destroying the session.

| Type                    | Mapping               |
| ----------------------- | --------------------- |
{%- for basic_type in basic_type %}
| `{{ basic_type.name }}` | {{ basic_type.desc }} |
{%- endfor %}


## Special Types

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