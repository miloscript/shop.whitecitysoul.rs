# AGENTS.md

MANDATORY: YOU MUST CALL "learn_shopify_api" ONCE WHEN WORKING WITH LIQUID THEMES.

## Reference docs (read when needed)

- **[docs/liquid-reference.md](docs/liquid-reference.md)** — Liquid delimiters, operators, filters, objects, and tags
- **[docs/localization-standards.md](docs/localization-standards.md)** — Locale file structure, schema locale files, key organization
- **[docs/component-examples.md](docs/component-examples.md)** — Full examples of snippets, blocks, and sections

## Theme Architecture

**Key principles: focus on generating snippets, blocks, and sections; users may create templates using the theme editor**

### Directory structure

```
.
├── assets          # Static assets (CSS, JS, images, fonts). Only critical.css and globally-needed files here.
├── blocks          # Reusable, nestable, customizable components ({% schema %} required)
├── config          # settings_schema.json (schema) + settings_data.json (data)
├── layout          # Top-level HTML wrappers. Must include {{ content_for_header }} and {{ content_for_layout }}
├── locales         # Translation files (en.default.json, etc.)
├── sections        # Modular full-width page components ({% schema %} required)
├── snippets        # Reusable Liquid fragments via {% render %} ({% doc %} required)
└── templates       # JSON files defining page structure
```

**Blocks** must have `{% doc %}` when statically rendered via `{% content_for 'block', id: '42', type: 'block_name' %}`.
**Snippets** must always have `{% doc %}`.

Validate schemas: `schemas/section.json` for sections, `schemas/theme_block.json` for blocks, `schemas/theme_settings.json` for config, `schemas/translations.json` for locales.

### CSS & JavaScript

- Use `{% stylesheet %}` and `{% javascript %}` tags per component (supported in snippets, blocks, sections)
- Liquid is NOT rendered inside these tags

### LiquidDoc

```liquid
{% doc %}
  Renders a responsive image that might be wrapped in a link.

  @param {image} image - The image to be rendered
  @param {string} [url] - An optional destination URL for the image

  @example
  {% render 'image', image: product.featured_image %}
{% enddoc %}
```

## Schema best practices

**Single CSS property** — use CSS variables:
```liquid
<div class="collection" style="--gap: {{ block.settings.gap }}px">
```

**Multiple CSS properties** — use CSS classes:
```liquid
<div class="collection {{ block.settings.layout }}">
```

**Mobile columns** — use a select input with values `1` and `"2"`.

## Liquid key rules

- ALWAYS use nested `if` when logic requires more than one operator (no parentheses in Liquid)
- No ternary conditionals — use `{% if %}`
- `contains` only works with strings, not objects in arrays
- `for` loops max 50 iterations — use `{% paginate %}` for more
- One `{% javascript %}` and one `{% stylesheet %}` per file
- Snippets can't access variables created outside — pass as parameters

## Translation standards

- **Every user-facing text** must use `{{ 'key' | t }}`
- **Update `locales/en.default.json`** with all new keys
- Use descriptive, hierarchical keys (max 3 levels deep, snake_case)
- Only add English text; translators handle other languages
- **Use sentence case** for all user-facing text
- Use interpolation for dynamic content: `{{ 'key' | t: var: value }}`

```liquid
<!-- Good -->
<h2>{{ 'sections.featured_collection.title' | t }}</h2>

<!-- Bad -->
<h2>Featured Collection</h2>
```
