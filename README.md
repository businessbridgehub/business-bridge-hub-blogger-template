# Business Bridge Hub — Blogger XML Theme

A production-grade, fully responsive Blogger XML theme for the **Business Bridge Hub** brand. Built with a SaaS-style UI, widget-driven architecture, PageSpeed 95+ performance targets, full SEO optimization, and mobile-first touch-optimized UX.

## Table of Contents

- [Features](#features)
- [Performance](#performance)
- [Architecture](#architecture)
  - [Sections](#sections)
  - [Widgets](#widgets)
- [SEO](#seo)
- [Design System](#design-system)
- [Layout Editor Guide](#layout-editor-guide)
  - [Navigation](#navigation)
  - [Hero Section](#hero-section)
  - [Features Section](#features-section)
  - [About Section](#about-section)
  - [Testimonials Section](#testimonials-section)
  - [Blog Posts](#blog-posts)
  - [Sidebar](#sidebar)
  - [Footer](#footer)
- [Installation](#installation)
- [Customization](#customization)
- [Browser Support](#browser-support)

## Features

- **Zero external dependencies** — no Google Fonts, no CDN scripts, no jQuery. System font stack only (no font-loading CLS).
- **All CSS inline** via `b:skin` (~22KB compressed) — no external CSS files, zero external HTTP requests.
- **Widget-driven architecture** — all content areas (navigation, footer, sidebar, CTAs) managed via Blogger Layout widgets. No XML editing needed to change content.
- **Mobile-first responsive** — off-canvas nav, collapsible sidebar, 44px+ touch targets, `touch-action: manipulation`, GPU-composited animations.
- **Off-canvas mobile navigation** — slide-in from right (`translateX`), dark overlay, body scroll lock, auto-close on link tap.
- **CSS-only hover dropdowns** (desktop) + JS tap-to-toggle (mobile) for Categories and nested LinkList items.
- **Fixed sticky navbar** with `.is-sticky` scroll shadow, hamburger → X morph animation, `contain:layout style paint` for compositing.
- **Hero section** with animated blob gradients, feature card grid, and stat counters.
- **6-card features/services section** with gradient accent bar on hover.
- **About section** with staggered stat grid (2×2).
- **Testimonials section** with blockquote cards and star ratings.
- **Card-based blog grid** (responsive 3→2→1 columns) with image aspect ratio, tag badge, 3-line clamp, and hover lift.
- **Comprehensive 4-column footer grid** with 7 editable sections: About, Quick Links, Social, Legal, Newsletter, Copyright, Bottom Links.
- **Modular sidebar** with 6 pre-loaded widgets across 8 slots: BlogSearch, PopularPosts (5 all-time), BlogArchive (recent posts), Label (cloud tags), CTA block, Custom HTML.
- **Collapsible sidebar on mobile** — widget titles become tap-toggle with chevron rotation, start collapsed, `max-height` animation.
- **Proper heading hierarchy** — post title is `<h1>` on item pages, `<h2>` on index pages; hero `<h1>` on homepage.
- **SEO complete** — canonical URLs, Open Graph, Twitter Cards, JSON-LD (WebSite, Organization, BreadcrumbList, BlogPosting), breadcrumb RDFa.
- **`loading="lazy"` on all images** injected via JS feature-detect.
- **`prefers-reduced-motion`** support — zero-duration override for all animations.
- **`prefers-reduced-data`** support — disables blob gradient animations on data-saver mode.
- **WCAG AA** contrast compliance (4.5:1+ body text).
- **`content-visibility: auto`** on below-fold sections with CSS fallback for older browsers.
- **`backdrop-filter`** desktop-only (`min-width:769px`) to avoid GPU jank on low-end Android.
- **All scroll listeners** use `{passive:true}` — no browser scroll blocking.
- **`contain:layout style paint`** on header — limits paint/composite scope for scroll performance.
- **`@media(hover:none)`** suppresses hover effects on touch devices — no sticky-hover state.

## Performance

| Metric          | Target |
|-----------------|--------|
| PageSpeed Score | 95+    |
| External Reqs   | 0      |
| Render-blocking | 0      |
| CLS             | ~0     |
| FCP             | <1.5s  |

Performance strategies employed:

- All CSS inlined in `<head>` via `b:skin` — zero network requests for styles.
- System font stack — no font loading, no FOUT/FOIT, no CLS from web fonts.
- `content-visibility: auto` on all below-fold sections (hero, features, about, testimonials, blog grid, sidebar, footer) — deferred rendering until near viewport.
- CSS fallback for browsers without `content-visibility` support (e.g., Firefox < 109) via `@supports not` rule.
- `aspect-ratio` on image containers — prevents layout shift as images load.
- `loading="lazy"` injected on every `<img>` without explicit `loading` attribute.
- GPU-composited animations — `transform` and `opacity` only; no layout or paint triggers.
- Passive scroll event listeners.
- `contain:layout style paint` on fixed header — browser doesn't repaint header when content scrolls.
- No `document.write` or synchronous scripts.
- JavaScript placed at end of `<body>` — non-render-blocking.

## Architecture

### Sections

The theme defines **17 `b:section` elements**, each a drop zone in the Blogger Layout editor:

| Section ID            | Location     | Widgets | Content                                   |
|----------------------|--------------|---------|-------------------------------------------|
| `nav-brand`          | Header left  | 1       | Site logo/brand name (Text)               |
| `nav-pages`          | Nav center   | 1       | Static pages list (PageList)              |
| `nav-labels`         | Nav center   | 1       | Categories dropdown (Label)               |
| `nav-extra`          | Nav right    | 1       | Custom quick links (LinkList)             |
| `hero`               | Above fold   | 1       | Hero with blobs, cards, stats (Text)      |
| `features`           | Content      | 1       | 6-card features grid (Text)               |
| `about`              | Content      | 1       | About with stat grid (Text)               |
| `testimonials`       | Content      | 1       | Testimonial cards (Text)                  |
| `main-posts`         | Main column  | 1       | Blog post feed (Blog)                     |
| `sidebar-widgets`    | Sidebar      | 8 max   | 6 pre-loaded widget types                 |
| `footer-about`       | Footer col 1 | 1       | About brand text (Text)                   |
| `footer-quick`       | Footer col 2 | 1       | Quick link list (LinkList)                |
| `footer-social-links`| Footer col 3 | 1       | Social media links (LinkList)             |
| `footer-legal`       | Footer col 4 | 1       | Legal/privacy links (LinkList)            |
| `footer-newsletter`  | Footer       | 1       | Email subscribe form (Subscribe)          |
| `footer-copyright`   | Footer bar   | 1       | Copyright notice (Text)                   |
| `footer-bottom-links`| Footer bar   | 1       | Bottom row links (LinkList)               |

### Widgets

The theme ships with **22 `b:widget` elements** (some locked, some user-configurable). Widget types used:

| Widget Type      | Used In                               | Purpose                          |
|------------------|---------------------------------------|----------------------------------|
| `Text`           | NavBrand, Hero, Features, About,<br>Testimonials, CTABlock, FooterAbout,<br>FooterCopyright | Rich text / HTML content         |
| `PageList`       | NavPages                              | Static page navigation           |
| `Label`          | NavLabels, Label (sidebar)            | Category/tag navigation          |
| `LinkList`       | NavExtra, FooterQuick,<br>FooterSocial, FooterLegal,<br>FooterBottomLinks | Custom link lists                |
| `Blog`           | main-posts                            | Blog post feed with includables  |
| `BlogSearch`     | sidebar-widgets                       | Search form                      |
| `PopularPosts`   | sidebar-widgets                       | Top 5 popular posts with thumbs  |
| `BlogArchive`    | sidebar-widgets                       | Recent posts archive list        |
| `HTML`           | sidebar-widgets                       | Custom HTML/JavaScript widget    |
| `Subscribe`      | footer-newsletter                     | Email subscription form          |

## SEO

### Heading Hierarchy

- **Homepage**: Hero `<h1>`, section headings `<h2>`, feature cards `<h3>`
- **Item pages** (single post): Post title `<h1>`, sidebar/related `<h2>`, card headings `<h3>`
- **Index pages** (archive/label/search): Post title `<h2>`, sidebar `<h2>`, card headings `<h3>`

Controlled via `postTitle` includable override in the Blog widget — renders `<h1>` when `data:view.isPost` is true, `<h2>` otherwise.

### Structured Data (JSON-LD)

- **WebSite** with SearchAction — all pages
- **Organization** — homepage only
- **BreadcrumbList** — item pages only (2-position: Home > Page)
- **BlogPosting** — per-post via `postFooter` includable (includes `datePublished`, `dateModified`, `author.name`)

### Open Graph

- `og:locale`, `og:site_name`, `og:title`, `og:type` (`article` on posts, `website` elsewhere), `og:url`, `og:image` (only when `postImageUrl` exists)

### Twitter Card

- `twitter:card` (`summary_large_image`), `twitter:url`, `twitter:title`, `twitter:image`

### Breadcrumbs

- HTML `<nav>` with RDFa (`vocab='https://schema.org/' typeof='BreadcrumbList'`)
- Hidden on homepage (`data:blog.url == data:blog.homepageUrl`)
- Uses Blogger `data:nav` variables for label breadcrumbs when available

### Other

- Single canonical URL (`data:blog.url`) in `<head>` — no duplicate content
- Proper `<title>` with `pageName` on item pages
- Meta description from Blogger settings

## Design System

Design tokens defined as CSS custom properties in `:root`:

| Token Category | Values                                |
|----------------|---------------------------------------|
| Spacing        | 19 steps: `--space-3xs` through `--space-3xl` (0.25rem–4rem) |
| Text sizes     | 7 steps: `--text-xs` through `--text-4xl` (0.75rem–2.5rem) |
| Shadows        | 7 levels: `--shadow-xs` through `--shadow-2xl` |
| Radii          | 7 steps: `--radius-sm` through `--radius-full` (4px–9999px) |
| Easing         | 3 curves: `--ease-out`, `--ease-in-out`, `--ease-spring` |
| Colors         | Primary (indigo 600/700), accent (emerald 500/600), surface (white/gray 50), text (gray 900/600/400) |

## Layout Editor Guide

### Navigation

The header contains four section drop zones:

1. **`nav-brand`** — Place a `Text` widget here for your site name/logo. Supports HTML for logo images.
2. **`nav-pages`** — Place a `PageList` widget. Pages auto-populate as left-aligned nav links. The home page link is always first.
3. **`nav-labels`** — Place a `Label` widget. Labels render as a "Categories" dropdown. On desktop: CSS hover dropdown. On mobile: tap to toggle.
4. **`nav-extra`** — Place a `LinkList` widget. Links appear as right-aligned nav items. Supports nested link groups for multi-level dropdowns.

> **Tip**: To reorder nav items, drag widgets within their section in Layout view, or reorder PageList/LinkList entries in the widget settings.

### Hero Section

The `hero` section uses a `Text` widget. Default content includes:

- Animated blob gradient background (CSS-only, no JS)
- Main headline and subtitle
- Two CTA buttons
- Feature stat cards (e.g., "Projects Delivered", "Client Satisfaction")
- Feature highlights grid

Edit the widget HTML in Layout view to customize all text, stats, and buttons.

> **Note**: On `prefers-reduced-data`, blob animations are replaced with a static gradient to save battery/bandwidth.

### Features Section

The `features` section uses a `Text` widget. Default layout is a 6-card grid (responsive 3→2→1 columns). Each card has:

- An icon or emoji
- A heading
- Description text
- Gradient accent bar on hover

Replace the HTML with your own services/features. Each card is a `<div class="feature-card">`. You can add or remove cards.

### About Section

The `about` section uses a `Text` widget. Default layout includes:

- Section heading with accent underline
- Company description paragraph
- 2×2 stat grid (Years, Projects, Clients, Awards)

Edit the HTML to update company info and stats.

### Testimonials Section

The `testimonials` section uses a `Text` widget. Default layout includes:

- Section heading
- 3 testimonial blockquote cards
- Star ratings (CSS-generated via `--rating` custom property)
- Reviewer name and title

Each testimonial is a `<div class="testimonial-card">`. Set the star count by changing the `--rating` inline style (value 1–5).

### Blog Posts

The `main-posts` section uses the built-in `Blog` widget. It auto-populates with your posts. The theme renders posts as:

- **Index pages**: Card grid (responsive 3→2→1 columns) with featured image, tag badge, date, title (2-line clamp), excerpt (3-line clamp), and "Read More" link.
- **Item pages**: Full post with `<h1>` title, metadata bar (date, author, comments), featured image, post body, labels, prev/next navigation, and comments section.

**Includable overrides** used:

- `postTitle` — renders heading with proper hierarchy (`<h1>` on item, `<h2>` on index)
- `postFooter` — injects `BlogPosting` JSON-LD per post
- `postComments` — styled comment section

### Sidebar

The `sidebar-widgets` section supports up to **8 widgets** (6 pre-loaded, 2 spare slots). Pre-loaded widgets:

| Widget       | Type         | Notes                                    |
|-------------|-------------|------------------------------------------|
| BlogSearch  | BlogSearch   | Search form with icon                    |
| PopularPosts| PopularPosts | 5 all-time popular posts with thumbnails |
| BlogArchive | BlogArchive  | Renders as recent posts list (flat)      |
| Label       | Label        | Cloud/tag style with post counts         |
| CTABlock    | Text         | CTA card with button (locked)            |
| CustomHTML  | HTML         | Empty slot for your custom code          |

**Mobile behavior**: Sidebar widgets are collapsible. On viewports < 768px, each widget title becomes a tap target that toggles its content via `max-height` animation. Widgets start collapsed. The chevron icon rotates on toggle.

> **Tip**: Add or remove widgets in Layout view. The sidebar is on the right on desktop, collapses below the blog feed on mobile.

### Footer

The footer is organized as a 4-column grid with additional full-width rows:

| Section           | Widget Type | Content Hint                          |
|------------------|-------------|---------------------------------------|
| `footer-about`   | Text        | Company description, ~2–3 paragraphs  |
| `footer-quick`   | LinkList    | Quick navigation links                |
| `footer-social-links` | LinkList | Social media links with icon labels   |
| `footer-legal`   | LinkList    | Privacy policy, terms, etc.           |
| `footer-newsletter` | Subscribe | Email subscription form (full width)  |
| `footer-copyright` | Text      | Copyright notice text                 |
| `footer-bottom-links` | LinkList | Bottom row links (privacy, terms)    |

All footer widgets are **unlocked** in Layout view — edit their content directly.

## Installation

1. In your Blogger dashboard, go to **Theme** → **Backup/Restore** → **Download full theme** (save a backup).
2. Click **Backup/Restore** → **Choose file** → select `business-bridge-hub.xml`.
3. Click **Upload**.
4. Go to **Layout** to arrange widgets in the provided sections.
5. Populate each section with the appropriate widget type (see [Layout Editor Guide](#layout-editor-guide)).

## Customization

### Colors

Edit the CSS custom properties in `b:skin` (`:root` block, ~line 89):

```css
--color-primary: #4f46e5;
--color-primary-dark: #4338ca;
--color-accent: #10b981;
--color-accent-dark: #059669;
/* ... etc */
```

### Typography

The theme uses the system font stack:

```css
--font-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
             Ubuntu, Cantarell, 'Helvetica Neue', Arial, sans-serif;
```

To change font sizes, adjust `--text-*` tokens.

### Spacing

Adjust `--space-*` tokens to modify layout spacing globally.

### Adding Custom CSS

Append additional CSS rules inside the `<b:skin><![CDATA[ ... ]]></b:skin>` block (~lines 83–460).

### Adding Custom JavaScript

Add scripts at the end of the `<body>` section, before the closing `</body>` tag (~line 922).

## Browser Support

- **Chrome** 90+
- **Firefox** 88+
- **Safari** 15+
- **Edge** 90+
- **Samsung Internet** 15+
- **iOS Safari** 15+
- **Opera** 76+

Graceful degradation for older browsers:

- `content-visibility` falls back to normal rendering via `@supports not (content-visibility: auto)`
- CSS custom properties have fallbacks in critical paths
- `backdrop-filter` only on desktop (progressive enhancement)
- No JavaScript errors in browsers without ES6 support (older fallbacks pass through silently)

---

**Version**: 1.0.4 — Built for Business Bridge Hub.
