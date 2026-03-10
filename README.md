# shop.whitecitysoul.rs — Shopify Theme

A customized Shopify storefront for [shop.whitecitysoul.rs](https://shop.whitecitysoul.rs), built on the **Horizon v3.4.0** base theme by Shopify.

---

## Overview

This is a production Shopify theme using the modular block/section architecture. It supports full customization through the Shopify Theme Editor with no code required for layout changes.

---

## Structure

```
├── assets/        # JS (74 files), CSS (3 files), SVG icons
├── blocks/        # 94 modular block components
├── sections/      # 41 page section templates
├── snippets/      # 95 reusable Liquid partials
├── templates/     # 13 page templates (JSON + Liquid)
├── layout/        # Master layout (theme.liquid)
├── locales/       # 44 language translation files
└── config/        # settings_schema.json, settings_data.json
```

---

## Templates

| Template | Description |
|---|---|
| `index.json` | Home page |
| `product.json` | Product detail page |
| `collection.json` | Collection listing |
| `list-collections.json` | All collections |
| `cart.json` | Shopping cart |
| `search.json` | Search results |
| `blog.json` | Blog listing |
| `article.json` | Blog post |
| `page.json` | Generic page |
| `page.contact.json` | Contact page |
| `404.json` | Not found |
| `password.json` | Password-protected store |
| `gift_card.liquid` | Gift card |

---

## Key Sections

- **Header** — Sticky, transparent, with mobile drawer and search modal
- **Hero** — Full-width banner
- **Slideshow / Layered Slideshow** — Single and multi-layer carousels
- **Featured Product** — Showcase a single product
- **Featured Collection** — Grid of products from a collection
- **Product List** — Configurable product grid
- **Collection List** — Collection cards grid
- **Product Hotspots** — Clickable image regions linking to products
- **Media with Content** — Side-by-side media and text
- **Marquee** — Scrolling text/logo bar
- **Featured Blog Posts** — Blog post grid
- **Footer** — Responsive multi-column footer
- **Announcement Bar** — Top-of-page messaging
- **Predictive Search** — Real-time search with suggestions
- **Quick Order List** — Bulk ordering interface
- **Custom Liquid** — Arbitrary Liquid/HTML injection

---

## Notable Features

### Commerce
- Quick add to cart with fly-to-cart animation
- Sticky add-to-cart button on product pages
- Cart drawer (slide-out)
- Variant picker with color/image swatches
- Volume/bulk pricing display
- Recently viewed products
- Local pickup support
- Gift card recipient forms
- Discount code handling in cart

### Media & Content
- Before/after comparison slider
- Video background sections
- Drag-to-zoom image viewer
- Image zoom dialog
- Jumbo text with animations

### Search & Filtering
- Predictive/autocomplete search
- Faceted product filtering
- Grid density toggle (compact/default)

### Performance
- Native [View Transitions API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API) for smooth page navigation
- Render blocking for optimized First Contentful Paint
- Low-power device detection (disables animations)
- Section hydration / lazy loading
- `requestIdleCallback` and main-thread yielding utilities
- Reduced motion media query support

### Accessibility
- Skip-to-content link
- ARIA labels throughout
- Full keyboard navigation
- Focus management utilities

---

## Theme Settings

Configured via `config/settings_schema.json` (2,287 lines). Key customization areas:

- **Logo & Favicon** — Desktop/mobile logo, inverse logo for transparent header, favicon
- **Color Schemes** — Full palette control: background, text, headings, borders, shadows, primary/secondary buttons, variant picker states
- **Typography** — 4 font families (body, subheading, heading, accent), responsive sizing (H1–H3), line height, letter spacing, text case
- **Layout** — Page width, card hover effects, spacing
- **Header** — Sticky behavior, transparent header per page type (home/product/collection), search toggle
- **Animations** — View transitions toggle, page transition effects

---

## Internationalization

44 languages supported including English, German, French, Italian, Spanish, Portuguese (BR/PT), Dutch, Polish, Swedish, Norwegian, Finnish, Danish, Czech, Slovak, Slovenian, Croatian, Hungarian, Romanian, Russian, Bulgarian, Serbian, Greek, Japanese, Korean, Chinese (Simplified/Traditional), Vietnamese, Thai, Turkish, and Indonesian.

---

## Development

This theme is managed via the [Shopify CLI](https://shopify.dev/docs/themes/tools/cli).

```bash
# Install Shopify CLI
npm install -g @shopify/cli @shopify/theme

# Authenticate
shopify auth login

# Start dev server (live preview)
shopify theme dev --store white-city-soul.myshopify.com

# List themes
shopify theme list

# Push to a specific theme slot
shopify theme push --theme THEME_ID

# Pull latest from store
shopify theme pull
```

### Theme Slots

| Theme | Role | ID |
|---|---|---|
| Copy of Horizon | **live** | 151321215130 |
| Horizon | unpublished | 151094886554 |
| Development | development | 151320789146 |

---

## Base Theme

Built on **Horizon** — Shopify's official free theme. Source: [github.com/Shopify/horizon](https://github.com/Shopify/horizon)
