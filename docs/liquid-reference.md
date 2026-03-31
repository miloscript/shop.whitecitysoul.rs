# Liquid Reference

## Liquid delimiters

- **`{{ ... }}`**: Output – prints a value.
- **`{{- ... -}}`**: Output, trims whitespace around the value.
- **`{% ... %}`**: Logic/control tag (if, for, assign, etc.), does not print anything, no whitespace trim.
- **`{%- ... -%}`**: Logic/control tag, trims whitespace around the tag.

**Tip:**
Adding a dash (`-`) after `{%`/`{{` or before `%}`/`}}` trims spaces or newlines next to the tag.

**Examples:**
- `{{- product.title -}}` → print value, remove surrounding spaces or lines.
- `{%- if available -%}In stock{%- endif -%}` → logic, removes extra spaces/lines.

## Liquid operators

**Comparison operators:**
- ==
- !=
- >
- <
- >=
- <=

**Logical operators:**
- `or`
- `and`
- `contains` - checks if a string contains a substring, or if an array contains a string

### Comparison and comparison tags

**Key condition principles:**
- For simplificity, ALWAYS use nested `if` conditions when the logic requires more than one logical operator
- Parentheses are not supported in Liquid
- Ternary conditionals are not supported in Liquid, so always use `{% if cond %}`

**Basic comparison example:**
```liquid
{% if product.title == "Awesome Shoes" %}
  These shoes are awesome!
{% endif %}
```

**Multiple Conditions:**
```liquid
{% if product.type == "Shirt" or product.type == "Shoes" %}
  This is a shirt or a pair of shoes.
{% endif %}
```

**Contains Usage:**
- For strings: `{% if product.title contains "Pack" %}`
- For arrays: `{% if product.tags contains "Hello" %}`
- Note: `contains` only works with strings, not objects in arrays

**{% elsif %} (used inside if/unless only)**
```liquid
{% if a %}
  ...
{% elsif b %}
  ...
{% endif %}
```

**{% unless %}**
```liquid
{% unless condition %}
  ...
{% endunless %}
```

**{% case %}**
```liquid
{% case variable %}
  {% when 'a' %}
    a
  {% when 'b' %}
    b
  {% else %}
    other
{% endcase %}
```

**{% else %} (used inside if, unless, case, or for)**
```liquid
{% if product.available %}
  In stock
{% else %}
  Sold out
{% endif %}
```
_or inside a for loop:_
```liquid
{% for item in collection.products %}
  {{ item.title }}
{% else %}
  No products found.
{% endfor %}
```

### Variables and variable tags

```liquid
{% assign my_variable = 'value' %}

{% capture my_variable %}
  Contents of variable
{% endcapture %}

{% increment counter %}
{% decrement counter %}
```

## Liquid filters

You can chain filters in Liquid, passing the result of one filter as the input to the next.

See these filters:

- `upcase`: `{{ string | upcase }}` returns a **string**
- `split`: `{{ string | split: string }}` returns an **array** (as we may notice in the docs, `split` receives a string as its argument)
- `last`: `{{ array | last }}` returns **untyped**

Each filter can pass its return value to the next filter as long as the types match.

For example, `upcase` returns a string, which is suitable input for `split`, which then produces an array for `last` to use.

Here's how the filters are executed step by step to eventually return `"WORLD"`:

```liquid
{{ "hello world" | upcase | split: " " | last }}
```

- First, `"hello world"` is converted to uppercase: `"HELLO WORLD"`, which is a string
- Next, `split` can act on strings, so it splits the value by space into an array: `["HELLO", "WORLD"]`
- Finally, the `last` filter work with array, so `"WORLD"` is returned

### Array
- `compact`: `{{ array | compact }}` returns `array`
- `concat`: `{{ array | concat: array }}` returns `array`
- `find`: `{{ array | find: string, string }}` returns `untyped`
- `find_index`: `{{ array | find_index: string, string }}` returns `number`
- `first`: `{{ array | first }}` returns `untyped`
- `has`: `{{ array | has: string, string }}` returns `boolean`
- `join`: `{{ array | join }}` returns `string`
- `last`: `{{ array | last }}` returns `untyped`
- `map`: `{{ array | map: string }}` returns `array`
- `reject`: `{{ array | reject: string, string }}` returns `array`
- `reverse`: `{{ array | reverse }}` returns `array`
- `size`: `{{ variable | size }}` returns `number`
- `sort`: `{{ array | sort }}` returns `array`
- `sort_natural`: `{{ array | sort_natural }}` returns `array`
- `sum`: `{{ array | sum }}` returns `number`
- `uniq`: `{{ array | uniq }}` returns `array`
- `where`: `{{ array | where: string, string }}` returns `array`

### Cart
- `item_count_for_variant`: `{{ cart | item_count_for_variant: {variant_id} }}` returns `number`
- `line_items_for`: `{{ cart | line_items_for: object }}` returns `array`

### Collection
- `link_to_type`: `{{ string | link_to_type }}` returns `string`
- `link_to_vendor`: `{{ string | link_to_vendor }}` returns `string`
- `sort_by`: `{{ string | sort_by: string }}` returns `string`
- `url_for_type`: `{{ string | url_for_type }}` returns `string`
- `url_for_vendor`: `{{ string | url_for_vendor }}` returns `string`
- `within`: `{{ string | within: collection }}` returns `string`
- `highlight_active_tag`: `{{ string | highlight_active_tag }}` returns `string`

### Color
- `brightness_difference`: `{{ string | brightness_difference: string }}` returns `number`
- `color_brightness`: `{{ string | color_brightness }}` returns `number`
- `color_contrast`: `{{ string | color_contrast: string }}` returns `number`
- `color_darken`: `{{ string | color_darken: number }}` returns `string`
- `color_desaturate`: `{{ string | color_desaturate: number }}` returns `string`
- `color_difference`: `{{ string | color_difference: string }}` returns `number`
- `color_extract`: `{{ string | color_extract: string }}` returns `number`
- `color_lighten`: `{{ string | color_lighten: number }}` returns `string`
- `color_mix`: `{{ string | color_mix: string, number }}` returns `string`
- `color_modify`: `{{ string | color_modify: string, number }}` returns `string`
- `color_saturate`: `{{ string | color_saturate: number }}` returns `string`
- `color_to_hex`: `{{ string | color_to_hex }}` returns `string`
- `color_to_hsl`: `{{ string | color_to_hsl }}` returns `string`
- `color_to_oklch`: `{{ string | color_to_oklch }}` returns `string`
- `color_to_rgb`: `{{ string | color_to_rgb }}` returns `string`
- `hex_to_rgba`: `{{ string | hex_to_rgba }}` returns `string`

### Customer
- `customer_login_link`: `{{ string | customer_login_link }}` returns `string`
- `customer_logout_link`: `{{ string | customer_logout_link }}` returns `string`
- `customer_register_link`: `{{ string | customer_register_link }}` returns `string`
- `avatar`: `{{ customer | avatar }}` returns `string`
- `login_button`: `{{ shop | login_button }}` returns `string`

### Date
- `date`: `{{ date | date: string }}` returns `string`

### Default
- `default_errors`: `{{ string | default_errors }}` returns `string`
- `default`: `{{ variable | default: variable }}` returns `untyped`
- `default_pagination`: `{{ paginate | default_pagination }}` returns `string`

### Font
- `font_face`: `{{ font | font_face }}` returns `string`
- `font_modify`: `{{ font | font_modify: string, string }}` returns `font`
- `font_url`: `{{ font | font_url }}` returns `string`

### Format
- `date`: `{{ string | date: string }}` returns `string`
- `json`: `{{ variable | json }}` returns `string`
- `structured_data`: `{{ variable | structured_data }}` returns `string`
- `unit_price_with_measurement`: `{{ number | unit_price_with_measurement: unit_price_measurement }}` returns `string`
- `weight_with_unit`: `{{ number | weight_with_unit }}` returns `string`

### Hosted_file
- `asset_img_url`: `{{ string | asset_img_url }}` returns `string`
- `asset_url`: `{{ string | asset_url }}` returns `string`
- `file_img_url`: `{{ string | file_img_url }}` returns `string`
- `file_url`: `{{ string | file_url }}` returns `string`
- `global_asset_url`: `{{ string | global_asset_url }}` returns `string`
- `shopify_asset_url`: `{{ string | shopify_asset_url }}` returns `string`

### Html
- `time_tag`: `{{ string | time_tag: string }}` returns `string`
- `inline_asset_content`: `{{ asset_name | inline_asset_content }}` returns `string`
- `highlight`: `{{ string | highlight: string }}` returns `string`
- `link_to`: `{{ string | link_to: string }}` returns `string`
- `placeholder_svg_tag`: `{{ string | placeholder_svg_tag }}` returns `string`
- `preload_tag`: `{{ string | preload_tag: as: string }}` returns `string`
- `script_tag`: `{{ string | script_tag }}` returns `string`
- `stylesheet_tag`: `{{ string | stylesheet_tag }}` returns `string`

### Localization
- `currency_selector`: `{{ form | currency_selector }}` returns `string`
- `translate`: `{{ string | t }}` returns `string`
- `format_address`: `{{ address | format_address }}` returns `string`

### Math
- `abs`: `{{ number | abs }}` returns `number`
- `at_least`: `{{ number | at_least }}` returns `number`
- `at_most`: `{{ number | at_most }}` returns `number`
- `ceil`: `{{ number | ceil }}` returns `number`
- `divided_by`: `{{ number | divided_by: number }}` returns `number`
- `floor`: `{{ number | floor }}` returns `number`
- `minus`: `{{ number | minus: number }}` returns `number`
- `modulo`: `{{ number | modulo: number }}` returns `number`
- `plus`: `{{ number | plus: number }}` returns `number`
- `round`: `{{ number | round }}` returns `number`
- `times`: `{{ number | times: number }}` returns `number`

### Media
- `external_video_tag`: `{{ variable | external_video_tag }}` returns `string`
- `external_video_url`: `{{ media | external_video_url: attribute: string }}` returns `string`
- `image_tag`: `{{ string | image_tag }}` returns `string`
- `media_tag`: `{{ media | media_tag }}` returns `string`
- `model_viewer_tag`: `{{ media | model_viewer_tag }}` returns `string`
- `video_tag`: `{{ media | video_tag }}` returns `string`
- `article_img_url`: `{{ variable | article_img_url }}` returns `string`
- `collection_img_url`: `{{ variable | collection_img_url }}` returns `string`
- `image_url`: `{{ variable | image_url: width: number, height: number }}` returns `string`
- `img_tag`: `{{ string | img_tag }}` returns `string`
- `img_url`: `{{ variable | img_url }}` returns `string`
- `product_img_url`: `{{ variable | product_img_url }}` returns `string`

### Metafield
- `metafield_tag`: `{{ metafield | metafield_tag }}` returns `string`
- `metafield_text`: `{{ metafield | metafield_text }}` returns `string`

### Money
- `money`: `{{ number | money }}` returns `string`
- `money_with_currency`: `{{ number | money_with_currency }}` returns `string`
- `money_without_currency`: `{{ number | money_without_currency }}` returns `string`
- `money_without_trailing_zeros`: `{{ number | money_without_trailing_zeros }}` returns `string`

### Payment
- `payment_button`: `{{ form | payment_button }}` returns `string`
- `payment_terms`: `{{ form | payment_terms }}` returns `string`
- `payment_type_img_url`: `{{ string | payment_type_img_url }}` returns `string`
- `payment_type_svg_tag`: `{{ string | payment_type_svg_tag }}` returns `string`

### String
- `blake3`: `{{ string | blake3 }}` returns `string`
- `hmac_sha1`: `{{ string | hmac_sha1: string }}` returns `string`
- `hmac_sha256`: `{{ string | hmac_sha256: string }}` returns `string`
- `md5`: `{{ string | md5 }}` returns `string`
- `sha1`: `{{ string | sha1: string }}` returns `string`
- `sha256`: `{{ string | sha256: string }}` returns `string`
- `append`: `{{ string | append: string }}` returns `string`
- `base64_decode`: `{{ string | base64_decode }}` returns `string`
- `base64_encode`: `{{ string | base64_encode }}` returns `string`
- `base64_url_safe_decode`: `{{ string | base64_url_safe_decode }}` returns `string`
- `base64_url_safe_encode`: `{{ string | base64_url_safe_encode }}` returns `string`
- `capitalize`: `{{ string | capitalize }}` returns `string`
- `downcase`: `{{ string | downcase }}` returns `string`
- `escape`: `{{ string | escape }}` returns `string`
- `escape_once`: `{{ string | escape_once }}` returns `string`
- `lstrip`: `{{ string | lstrip }}` returns `string`
- `newline_to_br`: `{{ string | newline_to_br }}` returns `string`
- `prepend`: `{{ string | prepend: string }}` returns `string`
- `remove`: `{{ string | remove: string }}` returns `string`
- `remove_first`: `{{ string | remove_first: string }}` returns `string`
- `remove_last`: `{{ string | remove_last: string }}` returns `string`
- `replace`: `{{ string | replace: string, string }}` returns `string`
- `replace_first`: `{{ string | replace_first: string, string }}` returns `string`
- `replace_last`: `{{ string | replace_last: string, string }}` returns `string`
- `rstrip`: `{{ string | rstrip }}` returns `string`
- `slice`: `{{ string | slice }}` returns `string`
- `split`: `{{ string | split: string }}` returns `array`
- `strip`: `{{ string | strip }}` returns `string`
- `strip_html`: `{{ string | strip_html }}` returns `string`
- `strip_newlines`: `{{ string | strip_newlines }}` returns `string`
- `truncate`: `{{ string | truncate: number }}` returns `string`
- `truncatewords`: `{{ string | truncatewords: number }}` returns `string`
- `upcase`: `{{ string | upcase }}` returns `string`
- `url_decode`: `{{ string | url_decode }}` returns `string`
- `url_encode`: `{{ string | url_encode }}` returns `string`
- `camelize`: `{{ string | camelize }}` returns `string`
- `handleize`: `{{ string | handleize }}` returns `string`
- `url_escape`: `{{ string | url_escape }}` returns `string`
- `url_param_escape`: `{{ string | url_param_escape }}` returns `string`
- `pluralize`: `{{ number | pluralize: string, string }}` returns `string`

### Tag
- `link_to_add_tag`: `{{ string | link_to_add_tag }}` returns `string`
- `link_to_remove_tag`: `{{ string | link_to_remove_tag }}` returns `string`
- `link_to_tag`: `{{ string | link_to_tag }}` returns `string`

## Liquid objects

### Global objects
- `collections`, `pages`, `all_products`, `articles`, `blogs`, `cart`, `closest`, `content_for_header`, `customer`, `images`, `linklists`, `localization`, `metaobjects`, `request`, `routes`, `shop`, `theme`, `settings`, `template`, `additional_checkout_buttons`, `all_country_option_tags`, `canonical_url`, `content_for_additional_checkout_buttons`, `content_for_index`, `content_for_layout`, `country_option_tags`, `current_page`, `handle`, `page_description`, `page_image`, `page_title`, `powered_by_link`, `scripts`

### Page-specific objects
- `/article`: `article`, `blog`
- `/blog`: `blog`, `current_tags`
- `/cart`: `cart`
- `/checkout`: `checkout`
- `/collection`: `collection`, `current_tags`
- `/customers/account`: `customer`
- `/customers/addresses`: `customer`
- `/customers/order`: `customer`, `order`
- `/gift_card.liquid`: `gift_card`, `recipient`
- `/metaobject`: `metaobject`
- `/page`: `page`
- `/product`: `product`, `remote_product`
- `/robots.txt.liquid`: `robots`
- `/search`: `search`

## Liquid tags

### content_for
The `content_for` tag requires a type parameter to differentiate between rendering a number of theme blocks (`'blocks'`) and a single static block (`'block'`).

```
{% content_for 'blocks' %}
{% content_for 'block', type: "slide", id: "slide-1" %}
```

### form
Because there are many different form types available in Shopify themes, the `form` tag requires a type. Depending on the form type, an additional parameter might be required. You can specify the following form types:

- `activate_customer_password`, `cart`, `contact`, `create_customer`, `currency`, `customer`, `customer_address`, `customer_login`, `guest_login`, `localization`, `new_comment`, `product`, `recover_customer_password`, `reset_customer_password`, `storefront_password`

```
{% form 'form_type' %}
  content
{% endform %}
```

### layout

```
{% layout name %}
```

### assign
You can create variables of any basic type, object, or object property.

> Caution: Predefined Liquid objects can be overridden by variables with the same name.

```
{% assign variable_name = value %}
```

### break

```
{% break %}
```

### capture
You can create complex strings with Liquid logic and variables.

> Caution: Predefined Liquid objects can be overridden by variables with the same name.

```
{% capture variable %}
  value
{% endcapture %}
```

### case

```
{% case variable %}
  {% when first_value %}
    first_expression
  {% when second_value %}
    second_expression
  {% else %}
    third_expression
{% endcase %}
```

### comment
Any text inside `comment` tags won't be output, and any Liquid code will be parsed, but not executed.

```
{% comment %}
  content
{% endcomment %}
```

### continue

```
{% continue %}
```

### cycle
The `cycle` tag must be used inside a `for` loop. Use it to output text in a predictable pattern (e.g., odd/even classes).

```
{% cycle string, string, ... %}
```

### decrement
Variables declared with `decrement` are unique to their layout/template/section file but shared across snippets. Independent from `assign`/`capture` but shared with `increment`.

```
{% decrement variable_name %}
```

### doc
The `doc` tag allows developers to include documentation within Liquid templates. Content inside is not rendered.

```
{% doc %}
  Renders a message.

  @param {string} foo - A string value.
  @param {string} [bar] - An optional string value.

  @example
  {% render 'message', foo: 'Hello', bar: 'World' %}
{% enddoc %}
```

### echo
Same as `{{ }}` but usable inside `liquid` tags.

```
{% liquid
  echo expression
%}
```

### for
Maximum 50 iterations. Use `paginate` tag for larger arrays. Every `for` loop has an associated `forloop` object.

```
{% for variable in array %}
  expression
{% endfor %}
```

### if

```
{% if condition %}
  expression
{% endif %}
```

### increment
Same scoping rules as `decrement`.

```
{% increment variable_name %}
```

### raw

```
{% raw %}
  expression
{% endraw %}
```

### render
Inside snippets/app blocks, you can't directly access variables created outside — pass them as parameters. You can access global objects and template-specific objects.

```
{% render 'filename' %}
```

### tablerow
Must be wrapped in HTML `<table>` tags. Has associated `tablerowloop` object.

```
{% tablerow variable in array %}
  expression
{% endtablerow %}
```

### unless

```
{% unless condition %}
  expression
{% endunless %}
```

### paginate
Required for arrays with 50+ items. Paginatable arrays: `article.comments`, `blog.articles`, `collections`, `collection.products`, `customer.addresses`, `customer.orders`, `metaobject_definition.values`, `pages`, `product.variants`, `search.results`, and list settings.

```
{% paginate array by page_size %}
  {% for item in array %}
    forloop_content
  {% endfor %}
{% endpaginate %}
```

### javascript
One per section/block/snippet. Liquid is NOT rendered inside.

```
{% javascript %}
  javascript_code
{% endjavascript %}
```

### section
Renders a section statically.

```
{% section 'name' %}
```

### stylesheet
One per section/block/snippet. Liquid is NOT rendered inside.

```
{% stylesheet %}
  css_styles
{% endstylesheet %}
```

### sections
Renders section groups in layouts.

```
{% sections 'name' %}
```

### style
Color settings referenced inside update in real-time in the theme editor.

```
{% style %}
  CSS_rules
{% endstyle %}
```

### liquid
Each tag must be on its own line. Use `echo` for output.

```
{% liquid
  expression
%}
```
