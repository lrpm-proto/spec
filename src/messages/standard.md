{% import "macros.tera" as macros %}

# Standard Messages

- Message fields are ordered as defined below.
- Messages are defined below with the format of `<str-kind> (<u8-kind>) Message`.

{% for msg in messages %}
##  `{{ msg.name }}` (`{{ msg.code }}`) Message

| Kind code        | Kind name        | Message type     | Valid session stage(s)                           |
| ---------------- | ---------------- | ---------------- | ------------------------------------------------ |
| `{{ msg.code }}` | `{{ msg.name }}` | `{{ msg.type }}` | {{ macros::inline_code_list(items=msg.stages) }} |


{{ msg.desc }} <br />

| Field              | Type                | Description      |
| ------------------ | ------------------- | ---------------- |
{%- for field in msg.fields %}
| `{{ field.name }}` | `{{ field.type }}`  | {{ field.desc }} |
{%- endfor %}
{% endfor %}
