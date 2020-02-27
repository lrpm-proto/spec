# JSON, CBOR Message Frame

For JSON and supersets, messages are encoded as an array.

The first element is the message kind, and the following elements are the message fields.

## JSON Example

```json
["CALL", 13, "helloworld", "payload", {}]
```

**OR**

```json
[40, 13, "helloworld", "payload", {}]
```

## Basic Type Mappings

A basic mapped type uses the most specific type in its respective encoding.
Types that incorrectly map from a less specific type to a specific type should error.

For example in JSON, a `u8` and `u64` will map to a JSON number, while in CBOR
a `u8` will map to a `u8` and a `u64` to a `u64`.

### JSON Mapping

| Type   | Mapping      |
| ------ | ------------ |
| `u8`   | JSON number  |
| `u64`  | JSON number  |
| `str`  | JSON string  |
| `map`  | JSON object  |
| `val`  | JSON value   |

### CBOR Mapping

| Type   | Mapping                         |
| ------ | ------------------------------- |
| `u8`   | CBOR u8                         |
| `u64`  | CBOR u64                        |
| `str`  | CBOR string                     |
| `map`  | CBOR object (with `ASCII` keys) |
| `val`  | CBOR data item                  |
