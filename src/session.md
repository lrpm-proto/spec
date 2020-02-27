# Session

## States

| State              | Description      |
| ------------------ | ---------------- |
{%- for stage in stage %}
| `{{ stage.name }}` | {{ stage.desc }} |
{%- endfor %}
