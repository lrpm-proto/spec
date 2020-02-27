# Defined URIs

| Defined URI             | Description            |
| ----------------------- | ---------------------- |
{%- for defined_uri in uri_definitions %}
| `{{ defined_uri.uri }}` | {{ defined_uri.desc }} |
{%- endfor %}