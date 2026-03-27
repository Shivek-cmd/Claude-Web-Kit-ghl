# 01-SETUP.md — Project Setup & Architecture
> Load after 00-CORE.md. Use at the START of every new project before writing any page code.

---

## 📁 FOLDER STRUCTURE — ALWAYS USE THIS EXACTLY

```
claude-web-kit-ghl/
│
├── global/
│   ├── variables.css          # :root design tokens — single source of truth
│   ├── base.css               # reset, base styles, utility classes
│   ├── header.html            # sticky navbar (HTML + CSS + JS — self-contained)
│   ├── footer.html            # footer + WhatsApp FAB (HTML + CSS + JS — self-contained)
│   ├── gtm-head.html          # paste into GHL → Tracking → Head
│   └── gtm-body.html          # paste into GHL → Tracking → Body
│
├── scripts/
│   └── global-scripts.js      # IntersectionObserver, counters, FAQ, slider, filter, TOC
│
├── pages/
│   ├── home/
│   │   ├── index.html         # Homepage HTML (no inline CSS)
│   │   └── home.css           # Homepage styles
│   ├── about/
│   │   ├── about.html
│   │   └── about.css
│   ├── services/
│   │   ├── services.html
│   │   └── services.css
│   │   └── [service-slug]/
│   │       ├── service-page.html
│   │       └── service-page.css
│   ├── portfolio/
│   │   ├── portfolio.html
│   │   └── portfolio.css
│   ├── blog/
│   │   ├── blog.html          # Blog listing page
│   │   ├── blog.css
│   │   └── [post-slug]/
│   │       ├── post.html      # Individual blog post
│   │       └── post.css
│   ├── contact/
│   │   ├── contact.html
│   │   └── contact.css
│   └── 404/
│       ├── 404.html
│       └── 404.css
│
└── assets/
    ├── images/                # All page images (descriptive filenames, e.g. web-design-hero.jpg)
    ├── og/                    # OG images — 1200×630px per page
    │   ├── og-default.jpg
    │   ├── og-home.jpg
    │   ├── og-about.jpg
    │   ├── og-services.jpg
    │   └── og-blog.jpg
    └── icons/                 # Favicon set
        ├── favicon.ico
        ├── favicon-16x16.png
        ├── favicon-32x32.png
        ├── apple-touch-icon.png
        ├── android-chrome-192x192.png
        ├── android-chrome-512x512.png
        └── site.webmanifest
```

---

## 🗂️ KEY FILES EXPLAINED

| File | Purpose |
|------|---------|
| `global/variables.css` | Every design token. Colors, fonts, spacing, shadows, radii. Edit ONLY here — never hardcode. |
| `global/base.css` | CSS reset, base element styles, typography scale, utility classes. |
| `global/header.html` | Self-contained — HTML + scoped CSS + JS for navbar. Paste into GHL header tracking code. |
| `global/footer.html` | Self-contained — HTML + scoped CSS + JS for footer + WhatsApp button. GHL footer tracking code. |
| `global/gtm-head.html` | GTM snippet + Google Fonts + favicon links + OG fallbacks. GHL head tracking code. |
| `global/gtm-body.html` | GTM noscript + global-scripts.js link. GHL body tracking code. |
| `scripts/global-scripts.js` | All reusable JS: reveal animations, counters, FAQ accordion, slider, portfolio filter, TOC. |
| `pages/[page]/[page].html` | HTML structure only. No inline CSS except GHL-specific inline overrides if needed. |
| `pages/[page]/[page].css` | All CSS for that page (on top of variables + base). |

---

## 🔧 HTML PAGE TEMPLATE — EVERY PAGE STARTS WITH THIS

Copy this shell for every new page. Replace the `[placeholders]`.

```html
<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- SEO Meta -->
  <title>[Page Title — 50-60 chars] | Business Name</title>
  <meta name="description" content="[150-160 char unique description. Include primary keyword.]">
  <meta name="robots" content="index, follow">
  <link rel="canonical" href="https://yoursite.com/[page-slug]">

  <!-- Open Graph -->
  <meta property="og:title"       content="[Page Title] | Business Name">
  <meta property="og:description" content="[OG description]">
  <meta property="og:url"         content="https://yoursite.com/[page-slug]">
  <meta property="og:image"       content="https://yoursite.com/assets/og/og-[page].jpg">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:type"        content="website">
  <meta property="og:site_name"   content="Business Name">

  <!-- Twitter Card -->
  <meta name="twitter:card"        content="summary_large_image">
  <meta name="twitter:title"       content="[Page Title] | Business Name">
  <meta name="twitter:description" content="[Twitter description]">
  <meta name="twitter:image"       content="https://yoursite.com/assets/og/og-[page].jpg">

  <!-- CSS: always this order -->
  <link rel="stylesheet" href="../../global/variables.css">
  <link rel="stylesheet" href="../../global/base.css">
  <link rel="stylesheet" href="./[page].css">

  <!-- JSON-LD (page-specific schema — see 05-SEO.md) -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "WebPage",
    "name": "[Page Title]",
    "url": "https://yoursite.com/[page-slug]"
  }
  </script>
</head>

<body class="grain page-grid">

  <!-- Skip navigation -->
  <a href="#main" class="skip-nav">Skip to main content</a>

  <!-- Scroll progress bar -->
  <div id="scroll-progress" aria-hidden="true"></div>

  <!-- ═══════════════════════════════════════════════════
       HEADER: In GHL this is injected globally.
       For LOCAL TESTING only — paste header.html content here:
  ════════════════════════════════════════════════════════ -->
  <!-- [header.html content here for local preview] -->

  <main id="main">

    <!-- ─── YOUR PAGE SECTIONS GO HERE ─── -->

  </main>

  <!-- ═══════════════════════════════════════════════════
       FOOTER: In GHL this is injected globally.
       For LOCAL TESTING only — paste footer.html content here:
  ════════════════════════════════════════════════════════ -->
  <!-- [footer.html content here for local preview] -->

  <!-- Global scripts -->
  <script src="../../scripts/global-scripts.js" defer></script>

  <!-- Page-specific inline JS (if any) -->
  <script>
    // Page-specific logic here
  </script>

</body>
</html>
```

---

## 🧪 LOCAL TESTING SETUP

Since GHL injects header/footer globally, you need them inline for local preview:

1. Open `pages/home/index.html` in VS Code
2. Where the header comment is, paste the full content of `global/header.html`
3. Where the footer comment is, paste the full content of `global/footer.html`
4. Open with **Live Server** extension in VS Code (or just double-click to open in browser)

> **Tip**: Keep a `_local-preview/` folder with versions that have header/footer already included for quick testing. Never commit these — they're just for local use.

---

## 🌐 GHL DEPLOYMENT WORKFLOW

### One-time setup (do once per site):
1. Paste `gtm-head.html` → GHL Settings → Tracking Code → **Head**
2. Paste `gtm-body.html` → GHL Settings → Tracking Code → **Body**
3. Paste `header.html` content into GHL **Header** custom code section
4. Paste `footer.html` content into GHL **Footer** custom code section

### Per-page workflow (every update):
1. Edit `[page].html` + `[page].css` in VS Code
2. Test locally (open in browser / Live Server)
3. Copy `[page].css` contents → paste into GHL page **Custom CSS**
4. Copy `[page].html` body content → paste into GHL page **Custom HTML** block
5. Done — GHL handles the rest

---

## 🔗 URL SLUG RULES

- Always lowercase: `/web-design-services` not `/Web-Design-Services`
- Hyphens only: `/web-design` not `/web_design`
- Keywords in URL: `/blog/nextjs-seo-guide` not `/blog/post-123`
- Short and descriptive: max 4-5 words
- No stop words in slugs: avoid `a`, `the`, `and`, `of`
- No dates in page URLs (only blog slugs)
- Blog slugs: primary keyword at the start

```
✅ /services/web-design
✅ /blog/seo-guide-2025
✅ /about
❌ /services/our-amazing-web-design-services-page
❌ /blog/2024/01/15/post-1
❌ /p?id=123
```

---

## ♿ ACCESSIBILITY — ALWAYS

- All images: descriptive `alt` text (8-12 words, include keyword naturally)
- All icon-only buttons: `aria-label` attribute
- All forms: `<label for="id">` paired with every `<input>`
- Focus styles: `:focus-visible` ring on all interactive elements (in base.css)
- Semantic HTML: `<header>`, `<main>`, `<nav>`, `<section>`, `<article>`, `<footer>`
- Color contrast: 4.5:1 minimum for body text, 3:1 for large text
- Skip link at top of every page (in header.html)
- `role` and `aria-*` attributes on interactive components (sliders, accordions)
- Respect reduced motion:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## ⚡ PERFORMANCE — CORE WEB VITALS

### LCP < 2.5s
- `loading="eager"` on hero image (above fold — never lazy-load)
- `loading="lazy"` on all below-fold images
- Preload Google Fonts with `rel="preload"` + `onload` swap trick (in gtm-head.html)
- No render-blocking scripts — always use `defer` or `async`

### CLS < 0.1
- Always set explicit `width` and `height` on all `<img>` tags
- Use `aspect-ratio` CSS property to reserve image space before load
- Never inject content above existing content (header is position:fixed)

### INP < 200ms
- No heavy synchronous JS on page load
- Event listeners are passive where possible: `{ passive: true }`
- Defer all non-critical JS with `defer` attribute

---

## 🖼️ FAVICON SYSTEM

### Required files in `/assets/icons/`
```
favicon.ico                  # 16×16 + 32×32 multi-size
favicon-16x16.png
favicon-32x32.png
apple-touch-icon.png         # 180×180
android-chrome-192x192.png
android-chrome-512x512.png
site.webmanifest
```

### site.webmanifest
```json
{
  "name": "Business Name",
  "short_name": "BizName",
  "description": "Business description",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#0a0a0a",
  "theme_color": "#your-brand-color",
  "icons": [
    { "src": "/assets/icons/android-chrome-192x192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/assets/icons/android-chrome-512x512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

> **Always**: Generate real favicon files from your logo at **realfavicongenerator.net** — upload a 512×512 SVG/PNG, download the full package, place in `/assets/icons/`.
