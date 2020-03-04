# JSON, CBOR Message Encoding

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

For example:

- With JSON, `u8` and `u64` will be encoded to a JSON number.
- With JSON, a JSON number will attempt to decode into a `u8` if it fits or a `u64`.
- With CBOR, a `u8` will be encoded to a `u8`, respectively with `u64` to a `u64`.
- With CBOR, decoding a `u8` and `u64` map the same as with the encoding.

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
| `map`  | CBOR object (with `UTF-8` keys) |
| `val`  | CBOR data item                  |
