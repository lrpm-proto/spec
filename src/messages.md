{% import "macros.tera" as macros %}

# Messages

| Group                 | Description         |
| --------------------- | ------------------- |
{%- for msg_type in message_type %}
| `{{ msg_type.name }}` | {{ msg_type.desc }} |
{%- endfor %}

- Message fields are ordered as defined below.
- Messages are defined below with the format of `<str-kind> (<u8-kind>) Message`.

{% for msg in message %}
##  `{{ msg.name }}` (`{{ msg.code }}`) Message

| Kind code        | Kind name        | Message type     | Valid session stage(s)                          |
| ---------------- | ---------------- | ---------------- | ----------------------------------------------- |
| `{{ msg.code }}` | `{{ msg.name }}` | `{{ msg.type }}` | {{ macros::inline_code_list(items=msg.stages)}} |


{{ msg.desc }} <br />

| Field              | Type                | Description      |
| ------------------ | ------------------- | ---------------- |
{%- for field in msg.field %}
| `{{ field.name }}` | `{{ field.type }}`  | {{ field.desc }} |
{%- endfor %}
{% endfor %}
