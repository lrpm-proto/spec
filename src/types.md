{% import "macros.tera" as macros %}

# Types

## Basic Types

Any encoding method used must support these basic types.
A basic mapped type uses the most specific type in its respective encoding.
Types that incorrectly map from a less specific type to a specific type 
should cause a fatal error, destroying the session.

| Type                    | Mapping               |
| ----------------------- | --------------------- |
{%- for basic_type in basic_types %}
| `{{ basic_type.name }}` | {{ basic_type.desc }} |
{%- endfor %}


## Special Types

Special types build upon the basic types.

{% for special_type in special_types %}

### `{{ special_type.name }}` (extends {{ macros::inline_code_list(items=special_type.extends) }})

{{ special_type.desc }}

{% if special_type.constraints is defined %}

{% for constraint in special_type.constraints %}
- {{ constraint }}
{% endfor %}

{% endif %}

{% endfor %}
