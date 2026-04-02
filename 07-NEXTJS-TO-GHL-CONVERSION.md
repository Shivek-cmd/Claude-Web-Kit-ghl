# 07-NEXTJS-TO-GHL-CONVERSION.md — Convert Next.js Website to GHL (Raw HTML/CSS/JS)
> Load this when you have an existing Next.js website and need to convert it to the raw HTML + CSS + Vanilla JS folder structure defined in this repo. Every design detail, SEO tag, animation, and interaction must survive the conversion. Nothing is lost — only the delivery mechanism changes.

---

## 🧠 CONVERSION PHILOSOPHY

You are NOT rebuilding the website from scratch. You are **extracting** the existing Next.js site into a framework-free, GHL-compatible format. The visual output in the browser must be **pixel-identical** before and after conversion.

**Core rules:**
- The Next.js site is the **source of truth** for design, content, SEO, and interactions
- Every page, every section, every animation, every meta tag must carry over exactly
- This repo's folder structure (00-CORE.md, 01-SETUP.md) is the **target format**
- If the Next.js site does something that conflicts with this repo's rules, the repo's rules win for structure — but the **visual output** of the Next.js site wins for design
- Never add, remove, or change content during conversion — that's a separate task

**Before you start, answer these:**
- Have I loaded 00-CORE.md through 06-ADDITIONAL.md? (mandatory — they define the target)
- Do I have the live Next.js site URL or a local dev server running?
- Do I have access to the Next.js source code repo?
- Have I taken screenshots of every page (desktop + mobile) as the reference?

---

## 📁 FOLDER STRUCTURE MAPPING

### Next.js Structure → GHL Structure

```
NEXT.JS (source)                        GHL / THIS REPO (target)
─────────────────                        ─────────────────────────
app/layout.tsx                     →     global/header.html + global/footer.html
app/globals.css                    →     global/variables.css + global/base.css
app/page.tsx                       →     pages/home/index.html + pages/home/home.css
app/about/page.tsx                 →     pages/about/index.html + pages/about/about.css
app/services/page.tsx              →     pages/services/index.html + pages/services/services.css
app/services/[slug]/page.tsx       →     pages/services/[slug]/index.html + service-page.css
app/blog/page.tsx                  →     pages/blog/index.html + pages/blog/blog.css
app/blog/[slug]/page.tsx           →     pages/blog/[slug]/index.html + blog-post.css
app/contact/page.tsx               →     pages/contact/index.html + pages/contact/contact.css
app/not-found.tsx                  →     pages/404/index.html + pages/404/404.css
public/images/                     →     assets/images/
public/og/                         →     assets/og/
public/favicon.ico                 →     assets/icons/favicon.ico
components/Header.tsx              →     global/header.html (self-contained: HTML + CSS + JS)
components/Footer.tsx              →     global/footer.html (self-contained: HTML + CSS + JS)
lib/utils.ts                       →     scripts/global-scripts.js (relevant utils only)
next.config.js                     →     vercel.json (if deploying to Vercel)
public/robots.txt                  →     robots.txt (root level)
public/sitemap.xml                 →     sitemap.xml (root level)
tailwind.config.ts                 →     global/variables.css (extract all design tokens)
```

### Pages Router (older Next.js) → GHL Structure

```
NEXT.JS PAGES ROUTER               →     GHL / THIS REPO
──────────────────                        ─────────────────
pages/_app.tsx                     →     global/header.html + footer.html + base.css
pages/_document.tsx                →     HTML boilerplate in each page's index.html
pages/index.tsx                    →     pages/home/index.html
pages/about.tsx                    →     pages/about/index.html
pages/blog/[slug].tsx              →     pages/blog/[slug]/index.html
styles/globals.css                 →     global/variables.css + global/base.css
styles/Home.module.css             →     pages/home/home.css
```

---

## 🔄 CONVERSION PROCESS — STEP BY STEP

### Phase 1: Extract Design Tokens

**Goal:** Turn the Next.js design system into `global/variables.css`

1. **Open the Next.js source code.** Find all design values:
   - `tailwind.config.ts` / `tailwind.config.js` — colors, fonts, spacing, breakpoints
   - `globals.css` — any CSS custom properties already defined
   - `theme.ts` or any theme config file
   - Component-level hardcoded values (search for hex codes, `px`, `rem` values)

2. **Map every value to a CSS variable** following 02-DESIGN-SYSTEM.md token names:
   ```
   Tailwind: colors.primary → --color-primary
   Tailwind: colors.gray.900 → --color-bg (if used as background)
   Tailwind: fontFamily.display → --font-display
   Tailwind: spacing.8 → --space-8
   ```

3. **Create `global/variables.css`** using the exact template from 02-DESIGN-SYSTEM.md. Fill every token with the values extracted from the Next.js site.

4. **Extract Google Fonts.** Find the font imports in:
   - `app/layout.tsx` (Next.js `next/font` imports)
   - `_document.tsx` (Pages Router)
   - `globals.css` (`@import` or `@font-face`)
   
   Convert to the Google Fonts `<link>` preload pattern from 02-DESIGN-SYSTEM.md. Place in `gtm-head.html`.

**Verification:** Compare the extracted `variables.css` token values against the live Next.js site's computed styles in DevTools. Every color, font, and spacing must match.

---

### Phase 2: Extract Global Layout (Header + Footer)

**Goal:** Turn `layout.tsx` + header/footer components into self-contained HTML files

1. **Render the Next.js site** in a browser. Inspect the `<header>` and `<footer>` elements.

2. **Copy the rendered HTML** from DevTools (right-click element → Copy → Copy outerHTML). This gives you the final HTML without JSX/React wrappers.

3. **Extract all CSS** that applies to header/footer elements. Methods:
   - In DevTools, check Computed styles for each element
   - Search the Next.js codebase for header/footer CSS (Tailwind classes, CSS modules, styled-components)
   - Convert Tailwind classes to plain CSS properties
   - Convert CSS Modules classes to regular class names

4. **Extract all JS behavior:**
   - Mobile menu toggle → vanilla JS `addEventListener`
   - Scroll-based navbar styling → vanilla `scroll` event listener
   - Active link detection → `window.location.pathname` check
   - Theme toggle → `localStorage` + `data-theme` attribute
   - Any dropdown/mega-menu logic

5. **Assemble into self-contained files** following the pattern in 03-BUILD-WEBSITE.md:
   ```html
   <!-- global/header.html -->
   <style>
     /* ALL header CSS here — scoped, self-contained */
   </style>
   <header class="site-header">
     <!-- All header HTML here -->
   </header>
   <script>
     // ALL header JS here — self-contained
   </script>
   ```

6. **Do the same for `global/footer.html`** — including WhatsApp FAB if the Next.js site has one.

**Verification:** Open the assembled header.html in a browser. It must look and behave identically to the Next.js site's header — sticky behavior, mobile hamburger, active states, everything.

---

### Phase 3: Convert Each Page

**Goal:** Turn every Next.js page/route into a standalone HTML + CSS file pair

**For each page, follow this exact sequence:**

#### Step 3a: Create the HTML file

1. **Open the Next.js page component** (e.g., `app/about/page.tsx`).

2. **Identify all child components** used in the page. For each component:
   - Open its source file
   - Flatten the JSX into plain HTML
   - Replace React props/state with static content
   - Replace `className={styles.xyz}` with `class="xyz"`
   - Replace `className={cn(...)}` or Tailwind utility classes with semantic class names
   - Replace `<Image>` (next/image) with `<img>` — add `width`, `height`, `alt`, `loading="lazy"`
   - Replace `<Link href="...">` with `<a href="...">`
   - Replace `{variable}` interpolations with the actual content values
   - Remove all React hooks, state, effects — convert to vanilla JS where behavior is needed

3. **Assemble the full page HTML** using the template from 01-SETUP.md:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <!-- Copy ALL meta tags from the Next.js page's <Head> or metadata export -->
     <!-- CSS links: variables.css → base.css → page.css -->
   </head>
   <body>
     <!-- header.html content (for local testing) -->
     <main id="main">
       <!-- Flattened page sections in order -->
     </main>
     <!-- footer.html content (for local testing) -->
     <script src="/scripts/global-scripts.js" defer></script>
   </body>
   </html>
   ```

4. **Add semantic HTML** following 00-CORE.md rules:
   - Wrap in `<section>`, `<article>`, `<nav>`, `<aside>` as appropriate
   - Add `class="section"` and `class="container"` wrappers per 03-BUILD-WEBSITE.md
   - Add `class="reveal"` and `class="delay-N"` for scroll animations
   - Add breadcrumbs on all inner pages (not homepage)

#### Step 3b: Create the CSS file

1. **Extract all styles** for the page's elements from:
   - Tailwind utility classes → convert each to a CSS rule
   - CSS Modules (`.module.css`) → rename classes, copy rules
   - Styled-components / Emotion → extract the CSS string literals
   - Inline styles → move to the CSS file
   - Global CSS that targets page-specific elements

2. **Replace all hardcoded values** with CSS variables from `variables.css`:
   ```css
   /* WRONG — hardcoded from Next.js */
   color: #0a0a0a;
   font-family: "DM Sans", sans-serif;
   padding: 2rem;
   
   /* CORRECT — uses design tokens */
   color: var(--color-text);
   font-family: var(--font-body);
   padding: var(--space-8);
   ```

3. **Follow the CSS rules** from 00-CORE.md:
   - No `!important` (except `.sr-only`)
   - No `px` for font sizes — use `rem` or `clamp()`
   - No hardcoded colors, fonts, spacing — always CSS variables
   - Responsive: use `@media` queries matching the Next.js breakpoints

#### Step 3c: Convert JavaScript Interactions

Convert React state/effects to vanilla JS. Common patterns:

| Next.js / React | Vanilla JS Equivalent |
|---|---|
| `useState` + toggle | `element.classList.toggle('active')` |
| `useEffect` + IntersectionObserver | Move to `global-scripts.js` (already handles `.reveal`) |
| `useState` for form values | Direct DOM: `input.value` |
| `fetch` in `useEffect` | Inline `<script>` with `fetch()` |
| `onClick={() => ...}` | `element.addEventListener('click', ...)` |
| `useState` for filter/tabs | DOM: `querySelectorAll` + `classList` toggle |
| `useRef` | `document.getElementById()` or `document.querySelector()` |
| `next/router` push/replace | `window.location.href = '...'` |
| Framer Motion animations | CSS transitions + IntersectionObserver (see 02-DESIGN-SYSTEM.md) |
| `react-intersection-observer` | Native `IntersectionObserver` (already in `global-scripts.js`) |
| Dynamic imports / lazy loading | `loading="lazy"` on images, `defer` on scripts |

**Rule:** If the behavior is reusable across pages (counters, FAQ accordion, scroll reveal, slider, portfolio filter), it goes in `global-scripts.js`. If it's page-specific, it goes in an inline `<script>` at the bottom of that page's HTML.

---

### Phase 4: Preserve SEO — Exact Match Required

**Goal:** Every SEO tag from the Next.js site must appear in the converted HTML. Follow 05-SEO.md as the compliance standard.

1. **Extract metadata from every Next.js page.** Check:
   - `export const metadata = { ... }` (App Router)
   - `<Head>` component (Pages Router)
   - `generateMetadata()` function (dynamic metadata)
   - `opengraph-image.tsx` or `twitter-image.tsx` (generated OG images)

2. **For each page, verify these exist in the converted HTML `<head>`:**

   ```
   ✅ <title> — exact same text, 50-60 chars
   ✅ <meta name="description"> — exact same text, 150-160 chars
   ✅ <meta name="robots"> — index, follow, max-image-preview:large, max-snippet:-1
   ✅ <meta name="author"> — business name
   ✅ <link rel="canonical"> — full absolute URL (https://domain.com/page)
   ✅ <meta property="og:type"> — website (or article for blog posts)
   ✅ <meta property="og:site_name"> — business name
   ✅ <meta property="og:locale"> — en_US or appropriate locale
   ✅ <meta property="og:title"> — matches or near-matches <title>
   ✅ <meta property="og:description"> — matches or near-matches meta description
   ✅ <meta property="og:url"> — full absolute URL
   ✅ <meta property="og:image"> — full absolute URL to 1200x630 JPEG
   ✅ <meta property="og:image:width"> — 1200
   ✅ <meta property="og:image:height"> — 630
   ✅ <meta property="og:image:type"> — image/jpeg
   ✅ <meta property="og:image:alt"> — descriptive alt text
   ✅ <meta name="twitter:card"> — summary_large_image
   ✅ <meta name="twitter:site"> — @handle
   ✅ <meta name="twitter:creator"> — @handle
   ✅ <meta name="twitter:title"> — matches title
   ✅ <meta name="twitter:description"> — matches description
   ✅ <meta name="twitter:image"> — full absolute URL (same as og:image)
   ```

3. **For blog posts, also verify:**
   ```
   ✅ <meta property="og:type" content="article">
   ✅ <meta property="article:published_time">
   ✅ <meta property="article:modified_time">
   ✅ <meta property="article:author">
   ✅ <meta property="article:section">
   ✅ <meta property="article:tag"> (one per tag)
   ```

4. **JSON-LD structured data** — extract from Next.js and place in `<head>`:
   - Organization schema (homepage + about)
   - WebSite schema (homepage)
   - Article schema (every blog post)
   - BreadcrumbList schema (all inner pages)
   - Service schema (service pages)
   - FAQPage schema (any page with FAQ section)
   - See 05-SEO.md for exact templates

5. **Copy `robots.txt` and `sitemap.xml`** from Next.js `public/` to root level. Update any Next.js-specific URLs if the domain changes.

6. **Heading hierarchy** — must be identical:
   - Exactly 1 `<h1>` per page
   - `<h2>` for main sections
   - `<h3>` for sub-sections within `<h2>`
   - Never skip levels (no `<h1>` → `<h3>`)

**Verification:** Use `view-source:` on both the Next.js site and converted HTML. Every meta tag, JSON-LD block, and heading level must match.

---

### Phase 5: Convert Images & Assets

1. **Download all images** from the Next.js `public/` folder.

2. **Rename to descriptive filenames** per 00-CORE.md:
   ```
   ❌ IMG_3847.jpg
   ❌ hero-image-1.webp
   ✅ gold-export-warehouse-colombia.jpg
   ✅ team-leadership-meeting.jpg
   ```

3. **Handle `next/image` optimizations:**
   - Next.js auto-converts to WebP and resizes — for GHL, use the original source images
   - Add explicit `width` and `height` attributes to every `<img>` tag
   - Add `loading="lazy"` to all below-fold images
   - Add `loading="eager"` to hero/above-fold images
   - Write descriptive `alt` text (8-12 words, include keyword naturally)

4. **OG images** — copy to `assets/og/`:
   - If Next.js generates OG images dynamically (`opengraph-image.tsx`), generate static versions at 1200x630px JPEG
   - One unique OG image per page — never reuse the same image across pages
   - File naming: `og-home.jpg`, `og-about.jpg`, `og-[post-slug].jpg`

5. **Favicon set** — copy to `assets/icons/`:
   - If Next.js only has `favicon.ico`, generate the full set at realfavicongenerator.net
   - Required: favicon.ico, favicon-16x16.png, favicon-32x32.png, apple-touch-icon.png, android-chrome-192x192.png, android-chrome-512x512.png, site.webmanifest

---

### Phase 6: Convert Animations & Interactions

| Next.js Animation | GHL Equivalent |
|---|---|
| Framer Motion `initial` + `animate` | `.reveal` class + IntersectionObserver |
| Framer Motion `variants` with stagger | `.reveal .delay-1`, `.delay-2`, etc. |
| Framer Motion `whileInView` | IntersectionObserver in `global-scripts.js` |
| Framer Motion `whileHover` | CSS `:hover` transition |
| `@react-spring/web` | CSS `transition` + `@keyframes` |
| GSAP ScrollTrigger | IntersectionObserver + CSS scroll-driven animations |
| CSS-in-JS (`motion.div`) | Plain CSS with `.reveal` + `.visible` class swap |
| Lottie animations | Replace with CSS animation or inline SVG animation |
| Counter/number animation | `.counter` + `data-target` (handled by `global-scripts.js`) |
| Parallax scroll | CSS `transform` + scroll event listener |
| Infinite marquee | CSS `@keyframes marquee-scroll` (see 02-DESIGN-SYSTEM.md Pattern 4) |

**Key rule:** All scroll-triggered reveal animations use the single IntersectionObserver in `global-scripts.js`. Do NOT create separate observers per page. Add `.reveal` class to elements, optionally with `.delay-N` for stagger.

---

### Phase 7: Handle Dynamic Content

Next.js often fetches data dynamically. In the GHL static site, all content is hardcoded.

| Next.js Dynamic Pattern | GHL Static Equivalent |
|---|---|
| `getStaticProps` + API fetch | Hardcode the data directly in HTML |
| `getServerSideProps` | Hardcode — or use client-side `fetch` in `<script>` |
| Dynamic routes `[slug]` | Create one HTML file per slug (e.g., `blog/post-name/index.html`) |
| `generateStaticParams` | Create all pages manually |
| CMS content (Sanity, Contentful, etc.) | Copy final rendered content into HTML files |
| Blog post markdown → HTML | Render to HTML once, paste into page HTML |
| Search/filter with API | Client-side JS filter on static DOM elements |
| Pagination | Either load all items + JS filter, or create paginated HTML pages |

**Important:** If the Next.js site loads blog posts from a CMS, render each post to its final HTML and create static pages. The GHL version has no server — all content lives in the HTML.

---

### Phase 8: Deployment Setup

1. **Create `vercel.json`** (if deploying to Vercel) with rewrites for clean URLs:
   ```json
   {
     "rewrites": [
       { "source": "/about", "destination": "/pages/about/index.html" },
       { "source": "/blog", "destination": "/pages/blog/index.html" },
       { "source": "/blog/:slug", "destination": "/pages/blog/:slug/index.html" }
     ]
   }
   ```
   Or create root-level route copies (see 01-SETUP.md).

2. **Create root-level route files** for static hosting:
   - Copy `pages/home/index.html` → root `index.html`
   - Copy `pages/404/index.html` → root `404.html`
   - Copy `pages/about/index.html` → `about/index.html`
   - Repeat for every route

3. **Ensure all paths in HTML are absolute** (start with `/`):
   ```html
   <!-- CORRECT -->
   <link rel="stylesheet" href="/global/variables.css">
   <script src="/scripts/global-scripts.js" defer></script>
   <img src="/assets/images/hero.jpg">
   
   <!-- WRONG — breaks when file is in root-level route copy -->
   <link rel="stylesheet" href="../../global/variables.css">
   ```

4. **Test locally** with a static server:
   ```bash
   npx serve .
   # Navigate to http://localhost:3000
   # Click through every page — verify routing works
   ```

---

## ✅ CONVERSION CHECKLIST — VERIFY BEFORE SHIPPING

### Design System
- [ ] `variables.css` has every color, font, spacing, shadow, radius token from the Next.js site
- [ ] No hardcoded values in any CSS file — all use CSS variables
- [ ] Google Fonts loaded via preload pattern in `gtm-head.html`
- [ ] Font sizes use `rem` or `clamp()` — never `px`
- [ ] Responsive breakpoints match the Next.js site

### Pages
- [ ] Every Next.js page/route has a corresponding HTML + CSS file pair
- [ ] Every page uses the HTML template from 01-SETUP.md
- [ ] Every page includes: `variables.css` → `base.css` → `page.css` in `<head>`
- [ ] Every page has `<main id="main">` with all content inside
- [ ] Homepage matches pixel-for-pixel (screenshot comparison)
- [ ] All inner pages match pixel-for-pixel

### Components
- [ ] Header: sticky, blur-on-scroll, mobile hamburger, active link detection — all working
- [ ] Footer: sitemap links, social icons, WhatsApp FAB — all present
- [ ] All cards, badges, buttons use the class names from 03-BUILD-WEBSITE.md
- [ ] All forms have `<label>` + `<input>` pairing with validation

### SEO (see 05-SEO.md for full spec)
- [ ] Every page has: title, description, robots, author, canonical
- [ ] Every page has: og:type, og:site_name, og:locale, og:title, og:description, og:url, og:image (with width/height/type/alt)
- [ ] Every page has: twitter:card, twitter:site, twitter:creator, twitter:title, twitter:description, twitter:image
- [ ] Every page has appropriate JSON-LD (Organization, WebSite, Article, BreadcrumbList, Service, FAQPage)
- [ ] Every inner page has breadcrumbs (HTML + BreadcrumbList JSON-LD)
- [ ] Blog posts have: article:published_time, article:modified_time, article:author, article:tag
- [ ] Heading hierarchy: exactly 1 H1 per page, no skipped levels
- [ ] `robots.txt` exists at root with sitemap reference
- [ ] `sitemap.xml` exists at root with all page URLs + lastmod + changefreq + priority
- [ ] All canonical URLs use full absolute `https://` URLs
- [ ] All og:image URLs use full absolute `https://` URLs
- [ ] No duplicate meta titles across pages
- [ ] No meta description longer than 160 characters

### Images & Assets
- [ ] All `<img>` tags have: `src`, `alt`, `width`, `height`, `loading`
- [ ] Hero/above-fold images: `loading="eager"`
- [ ] All other images: `loading="lazy"`
- [ ] OG images: 1200x630px, JPEG, one unique per page, in `assets/og/`
- [ ] Favicon set complete in `assets/icons/`
- [ ] `site.webmanifest` present and correct
- [ ] Icons use inline SVG — not `<img>` tags

### Animations & JS
- [ ] All scroll reveals use `.reveal` + IntersectionObserver from `global-scripts.js`
- [ ] Counter animations use `.counter` + `data-target` from `global-scripts.js`
- [ ] FAQ accordion works via `global-scripts.js`
- [ ] Testimonial slider works via `global-scripts.js`
- [ ] No React, no npm, no node_modules — pure vanilla JS
- [ ] All scripts use `defer` attribute
- [ ] No `document.write()`, no jQuery, no `var` (use `const`/`let`)

### Accessibility
- [ ] Skip nav link in header
- [ ] All icon-only buttons have `aria-label`
- [ ] All forms have `<label for="id">`
- [ ] Color contrast: 4.5:1 body text, 3:1 large text
- [ ] `prefers-reduced-motion` media query in `variables.css`

### Deployment
- [ ] Root-level route files exist for every page
- [ ] `vercel.json` (or equivalent) configured if using Vercel
- [ ] All HTML paths are absolute (start with `/`)
- [ ] Local test with `npx serve .` — every route loads
- [ ] Social preview tested in Facebook Debugger + X Card Validator

---

## 🚫 CONVERSION — NEVER DO THESE

- **NEVER** leave Tailwind utility classes in the HTML — convert all to semantic class names + CSS
- **NEVER** leave React/JSX syntax in HTML — no `className`, no `{}`, no `<Component />`
- **NEVER** leave `next/image` `<Image>` tags — convert to plain `<img>`
- **NEVER** leave `next/link` `<Link>` tags — convert to plain `<a>`
- **NEVER** import npm packages — everything is vanilla JS
- **NEVER** use CSS Modules syntax (`.module.css`) — use regular class names
- **NEVER** leave `getStaticProps` / `getServerSideProps` references — all content is static
- **NEVER** change the visual design during conversion — pixel-identical is the goal
- **NEVER** skip SEO tags — every meta tag from Next.js must appear in the converted HTML
- **NEVER** skip JSON-LD structured data — extract from Next.js and place in `<head>`
- **NEVER** use relative paths for CSS/JS/images — always absolute paths from root
- **NEVER** skip the checklist above — run it for every converted page before shipping

---

## 🔍 QUICK REFERENCE — JSX TO HTML

```jsx
// NEXT.JS JSX
<Link href="/about" className={styles.navLink}>
  About Us
</Link>

<Image
  src="/images/hero.jpg"
  alt="Hero image"
  width={1920}
  height={1080}
  priority
/>

<div className={cn("card", isActive && "card-active")}>
  {items.map(item => (
    <p key={item.id}>{item.title}</p>
  ))}
</div>

{showModal && <Modal onClose={() => setShowModal(false)} />}
```

```html
<!-- CONVERTED HTML -->
<a href="/about" class="nav-link">
  About Us
</a>

<img
  src="/assets/images/hero.jpg"
  alt="Hero image showing company headquarters"
  width="1920"
  height="1080"
  loading="eager"
>

<div class="card card-active">
  <p>Item 1 Title</p>
  <p>Item 2 Title</p>
  <p>Item 3 Title</p>
</div>

<!-- Modal: implement with vanilla JS toggle -->
<div class="modal" id="my-modal" style="display:none;">...</div>
<script>
  document.getElementById('modal-trigger').addEventListener('click', function() {
    document.getElementById('my-modal').style.display = 'flex';
  });
</script>
```

---

## 📋 CONVERSION ORDER — DO IN THIS SEQUENCE

1. **Extract design tokens** → `variables.css`
2. **Set up Google Fonts** → `gtm-head.html`
3. **Create `base.css`** → reset, utilities, button/card/badge styles
4. **Convert header** → `global/header.html` (self-contained)
5. **Convert footer** → `global/footer.html` (self-contained)
6. **Write `global-scripts.js`** → all shared behaviors (reveal, counter, FAQ, slider, filter)
7. **Convert homepage** → `pages/home/index.html` + `home.css`
8. **Convert inner pages** (about, services, portfolio, contact, etc.)
9. **Convert blog listing** → `pages/blog/index.html` + `blog.css`
10. **Convert each blog post** → `pages/blog/[slug]/index.html` + `blog-post.css`
11. **Create 404 page** → `pages/404/index.html` + `404.css`
12. **Create privacy policy / terms** pages
13. **Copy all images** to `assets/images/` with descriptive filenames
14. **Generate OG images** (1200x630 JPEG each) → `assets/og/`
15. **Generate favicon set** → `assets/icons/`
16. **Create `robots.txt`** at root
17. **Create `sitemap.xml`** at root
18. **Create root-level route copies** for static hosting
19. **Create `vercel.json`** (if using Vercel)
20. **Local test** — `npx serve .` — click through every page
21. **Screenshot comparison** — Next.js vs converted, every page, desktop + mobile
22. **Run full checklist** above — every box must be checked
