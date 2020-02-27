# Session

## States

| State              | Description      |
| ------------------ | ---------------- |
{%- for stage in session.stages %}
| `{{ stage.name }}` | {{ stage.desc }} |
{%- endfor %}
