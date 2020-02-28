# Messages

## Groups

| Group                 | Description         |
| --------------------- | ------------------- |
{%- for msg_type in message.types %}
| `{{ msg_type.name }}` | {{ msg_type.desc }} |
{%- endfor %}

## Kind Ranges

| Range       | Description          |
| ----------- | -------------------- |
| `001 - 019` | Session messages     |
| `020 - 039` | Generic messages     |
| `040 - 059` | RPC messages         |
| `060 - 079` | PubSub messages      |
| `080 - 126` | Reserved             |
| `127 - 255` | Application specific |
