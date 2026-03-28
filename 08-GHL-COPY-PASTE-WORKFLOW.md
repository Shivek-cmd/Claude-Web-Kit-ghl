# 08-GHL-COPY-PASTE-WORKFLOW.md — How to Deploy Your Raw HTML/CSS/JS Site into GoHighLevel
> Load this after you have a completed raw HTML/CSS/JS website (built following this repo's guides 00–07). This file tells you exactly what to copy, where to paste it, and in what order inside GHL's page builder. GHL does NOT support npm, build tools, or global CSS injection — every page must be self-contained. Follow this guide exactly or styles will break.

---

## 🧠 HOW GHL WORKS — KEY CONSTRAINTS

Before you copy a single line, understand GHL's limitations:

1. **No global CSS.** There is no single place to add CSS that applies to every page. You must add `variables.css` + `base.css` content to **every single page** via the Custom CSS component.
2. **No file linking.** You cannot use `<link rel="stylesheet" href="/global/variables.css">` — GHL does not serve static files from a folder structure. Everything is pasted inline.
3. **Global scripts only via Site Settings.** The only way to share JS across all pages is through the **Header/Footer Tracking Code** in GHL's Site Settings.
4. **Custom Code component = HTML block.** Each page can have one or more Custom Code components where you paste raw HTML (with inline `<style>` and `<script>` if needed).
5. **Custom CSS component = page-level CSS.** Each page has a Custom CSS field where you paste CSS. This CSS is scoped to that page only — it does NOT carry to other pages.
6. **No folder structure.** GHL pages are flat — there's no `/pages/about/` folder. Each page is just a GHL page with a URL slug.

---

## 📁 REPO STRUCTURE → GHL MAPPING

Here is how each file/folder in your repo maps to a location inside GHL:

```
REPO FILE / FOLDER                    WHERE IT GOES IN GHL
──────────────────                    ────────────────────
global/variables.css            →     Custom CSS of EVERY page (paste at the top)
global/base.css                 →     Custom CSS of EVERY page (paste after variables.css)
global/header.html              →     Custom Code component at TOP of EVERY page
global/footer.html              →     Custom Code component at BOTTOM of EVERY page
global/gtm-head.html            →     Site Settings → Header Tracking Code
global/gtm-body.html            →     Site Settings → Footer Tracking Code
scripts/global-scripts.js       →     Site Settings → Footer Tracking Code (wrap in <script>)
pages/home/home.css             →     Custom CSS of the Homepage (paste AFTER base.css)
pages/home/index.html           →     Custom Code component(s) on Homepage (the <main> content only)
pages/about/about.css           →     Custom CSS of the About page (paste AFTER base.css)
pages/about/index.html          →     Custom Code component(s) on About page (<main> content only)
pages/blog/blog.css             →     Custom CSS of the Blog Listing page
pages/blog/[slug]/index.html    →     Custom Code component(s) on each Blog Post page
pages/blog/blog-post.css        →     Custom CSS of each Blog Post page
assets/images/*                 →     Upload to GHL Media Library, then update src URLs
assets/og/*                     →     Upload to GHL Media Library or external CDN
assets/icons/*                  →     Upload favicon to GHL Site Settings → Favicon
robots.txt                      →     GHL Site Settings → Custom Code (or domain-level config)
sitemap.xml                     →     Host externally or generate via GHL settings
```

---

## 🔧 STEP-BY-STEP: DEPLOY TO GHL

### Step 0: Pre-Flight — Fix the base.css Visibility Problem

**CRITICAL:** The `base.css` reset sets `body { color: var(--body); }` and headings to `color: var(--foreground);`. If `variables.css` is not loaded on a page, these CSS variables resolve to nothing — making **all text invisible** (transparent color).

**The fix:** When pasting `variables.css` and `base.css` into GHL's Custom CSS, they must ALWAYS be pasted together, with `variables.css` content FIRST. Never paste `base.css` alone.

Additionally, add fallback values to the critical color declarations in base.css before pasting. Here are the lines that cause invisible text and their fixes:

```css
/* ── ORIGINAL (breaks if variables.css is missing) ── */
body {
  font-family: var(--font-body);
  color: var(--body);           /* ← invisible if --body undefined */
  background: var(--bg);        /* ← transparent if --bg undefined */
}
h1, h2, h3, h4, h5, h6 {
  color: var(--foreground);     /* ← invisible if --foreground undefined */
}
a { color: inherit; }           /* ← inherits invisible color */

/* ── FIXED (safe for GHL — add fallbacks) ── */
body {
  font-family: var(--font-body, 'Inter', system-ui, sans-serif);
  color: var(--body, #4A4458);
  background: var(--bg, #F7F6FA);
}
h1, h2, h3, h4, h5, h6 {
  color: var(--foreground, #1A1625);
}
```

**Rule:** When writing base.css for any website that will be deployed to GHL, ALWAYS include a hardcoded fallback value for every `var()` that affects text color, background, or font-family. The fallback should match the value defined in `variables.css`. This protects against GHL pages where the CSS might load partially or out of order.

**Other base.css rules that can cause GHL conflicts:**

| Rule | Problem in GHL | Fix |
|---|---|---|
| `*, *::before, *::after { margin: 0; padding: 0; }` | Removes all spacing from GHL's own elements (section titles, buttons, form fields injected by GHL) | Keep this rule — but be aware GHL builder components may look different in the editor vs published page |
| `ul, ol { list-style: none; }` | Removes bullet points from any GHL text blocks with lists | If you need default list styles inside GHL text blocks, add `.ghl-content ul { list-style: disc; }` override |
| `a { color: inherit; text-decoration: none; }` | Links become invisible if parent text color is undefined | Keep — but ensure `variables.css` is always loaded first |
| `img, video, svg { display: block; max-width: 100%; }` | May affect GHL's own UI images in the builder | Safe in published mode — only affects your custom code components |

---

### Step 1: Site Settings — Global Scripts & Tracking

**Location in GHL:** Sites/Funnels → Your Site → Settings (gear icon) → Tracking Code

#### Header Tracking Code
Paste the contents of `global/gtm-head.html` here. This includes:
- Google Tag Manager `<script>` snippet (if configured)
- Google Fonts `<link>` preload tags
- Favicon `<link>` tags

```html
<!-- PASTE THIS INTO: Site Settings → Header Tracking Code -->

<!-- Google Tag Manager (replace GTM-XXXXXXX with your actual ID) -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXXX');</script>

<!-- Fonts (update the family names for your project) -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=YOUR+DISPLAY+FONT&family=YOUR+BODY+FONT&display=swap" rel="stylesheet">

<!-- Favicon -->
<link rel="icon" href="YOUR_FAVICON_URL" type="image/x-icon">
<link rel="apple-touch-icon" href="YOUR_APPLE_TOUCH_ICON_URL">
```

**Important:** Since GHL cannot serve files from `/assets/icons/`, you must either:
- Upload favicon files to GHL's Media Library and use those URLs
- Host them externally (CDN, Cloudflare R2, S3) and use absolute URLs
- Use GHL's built-in Favicon setting (Settings → General → Favicon)

#### Footer Tracking Code
Paste the contents of `global/gtm-body.html` here, PLUS the entire `global-scripts.js` wrapped in a `<script>` tag:

```html
<!-- PASTE THIS INTO: Site Settings → Footer Tracking Code -->

<!-- GTM noscript (replace GTM-XXXXXXX with your actual ID) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-XXXXXXX"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>

<!-- Global Scripts (paste entire contents of scripts/global-scripts.js inside this tag) -->
<script>
// ========== PASTE THE ENTIRE CONTENTS OF scripts/global-scripts.js HERE ==========
document.addEventListener('DOMContentLoaded', () => {
  // ... all your JS code ...
});
</script>
```

**Why Footer Tracking Code?** Scripts placed here load on EVERY page in the site — this is the only way to share JavaScript globally in GHL. The `defer` attribute is not needed since footer code already loads at the bottom.

---

### Step 2: Upload All Images to GHL Media Library

Before building pages, upload all images so you have GHL-hosted URLs ready.

1. Go to **Sites/Funnels → Your Site → Media Library** (or use the global Media Library)
2. Upload all files from these repo folders:
   - `assets/images/*` — logos, team photos, project images
   - `assets/og/*` — Open Graph images (1200×630)
   - Any page-specific images referenced in HTML

3. **Record the new URLs.** GHL gives each uploaded image a URL like:
   ```
   https://storage.googleapis.com/msgsndr/XXXXX/media/YYYYY.webp
   ```

4. **Create a URL mapping document** (keep this for your reference):
   ```
   /assets/images/logo-circle.png     →  https://storage.googleapis.com/msgsndr/xxx/media/logo-circle.png
   /assets/images/hero-1.webp         →  https://storage.googleapis.com/msgsndr/xxx/media/hero-1.webp
   /assets/og/og-home.jpg             →  https://storage.googleapis.com/msgsndr/xxx/media/og-home.jpg
   ```

5. **You will need to find-and-replace** all `/assets/...` paths in your HTML with the new GHL media URLs before pasting.

---

### Step 3: Build Each Page in GHL

For EVERY page in your site, follow this exact sequence:

#### 3a. Create the Page in GHL
1. Go to **Sites/Funnels → Your Site → Pages**
2. Click **+ New Page** (or **+ New Step** in funnels)
3. Choose **Blank Template**
4. Set the page name and URL slug to match your repo's structure:
   ```
   pages/home/         →  Homepage (set as default/index)
   pages/about/        →  /about
   pages/services/     →  /services
   pages/contact/      →  /contact
   pages/blog/         →  /blog
   pages/blog/[slug]/  →  /blog/[slug]
   pages/privacy-policy/ → /privacy-policy
   pages/terms-of-service/ → /terms-of-service
   pages/404/          →  /404 (set as custom 404 in site settings)
   ```

#### 3b. Add Page SEO Settings
1. Click the **page settings** (gear icon for that page)
2. Go to **SEO Meta Data** section
3. Fill in from the `<head>` of that page's `index.html`:
   - **Title** → value from `<title>` tag
   - **Description** → value from `<meta name="description">`
   - **OG Image** → upload the page's OG image from `assets/og/`
   - **Social Share Image** → same OG image
   - **Canonical URL** → if different from default

4. **For JSON-LD structured data:** You need to add it via Custom Code (see Step 3d) or the Header Tracking Code. GHL doesn't have a native JSON-LD field.

#### 3c. Add Custom CSS (variables + base + page-specific)

**Location:** In the page builder, click **Page Settings** (or the CSS icon) → **Custom CSS**

Paste CSS in this **exact order** — the order matters because later rules override earlier ones:

```css
/* ================================================
   SECTION 1: DESIGN TOKENS (from global/variables.css)
   Copy-paste the ENTIRE contents of variables.css here
   ================================================ */
:root {
  --bg:            #F7F6FA;
  --bg-secondary:  #FCFBFF;
  --bg-depth:      #F0ECF8;
  --foreground:    #1A1625;
  --body:          #4A4458;
  --muted:         #7A748C;
  --border:        #E6E1F0;
  --accent:        #7C3AED;
  /* ... paste ALL variables ... */
}

/* ================================================
   SECTION 2: BASE STYLES (from global/base.css)
   Copy-paste the ENTIRE contents of base.css here
   Make sure fallback values are added (see Step 0)
   ================================================ */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
/* ... paste ALL base.css ... */

/* ================================================
   SECTION 3: PAGE-SPECIFIC CSS (from pages/[page]/[page].css)
   Copy-paste the page's own CSS file here
   e.g., for Homepage: paste pages/home/home.css
   e.g., for About: paste pages/about/about.css
   ================================================ */
/* ... paste page CSS here ... */
```

**YOU MUST DO THIS FOR EVERY PAGE.** There is no shortcut. If you skip `variables.css` on any page, text will be invisible.

#### 3d. Add Custom Code Components (HTML)

Now add the actual page content using GHL's Custom Code components.

**For each page, you need 3 Custom Code components in this order:**

**Component 1: Header**
1. In the page builder, add a **Custom Code** (or "Code" / "HTML") element
2. Drag it to the very TOP of the page
3. Paste the entire contents of `global/header.html`
   - This includes the `<header>` HTML, inline `<style>`, and inline `<script>`
   - The header file is self-contained — it has its own CSS and JS inside it
4. **Replace image paths** with GHL media URLs:
   ```html
   <!-- BEFORE -->
   <img src="/assets/images/logo-circle.png" ...>
   <!-- AFTER -->
   <img src="https://storage.googleapis.com/msgsndr/xxx/media/logo-circle.png" ...>
   ```

**Component 2: Main Content**
1. Add another **Custom Code** element below the header
2. From the page's `index.html`, copy ONLY the `<main>` section (everything between `<main>` and `</main>`)
   - Do NOT copy the `<!DOCTYPE>`, `<html>`, `<head>`, or `<body>` tags — GHL provides these
   - Do NOT copy the header or footer HTML — those are separate components
3. **Replace all image paths** with GHL media URLs
4. **Replace all internal links** — GHL may use different URL patterns:
   ```html
   <!-- BEFORE (repo structure) -->
   <a href="/about">About</a>
   <a href="/blog/my-post">My Post</a>
   
   <!-- AFTER (GHL structure — depends on your site/funnel setup) -->
   <a href="/about">About</a>        <!-- same if slug matches -->
   <a href="/blog/my-post">My Post</a>  <!-- same if slug matches -->
   ```
5. If the page has page-specific `<script>` tags at the bottom of the `<main>`, include them in this component

**Component 3: Footer**
1. Add another **Custom Code** element at the very BOTTOM of the page
2. Paste the entire contents of `global/footer.html`
   - Like the header, this is self-contained with its own `<style>` inside
3. **Replace image paths** with GHL media URLs

#### 3e. Add JSON-LD Structured Data

For each page's JSON-LD (the `<script type="application/ld+json">` block from the `<head>` of each `index.html`):

1. Add a **Custom Code** element at the top of the page (before or after the header component)
2. Paste the JSON-LD script tag:
   ```html
   <script type="application/ld+json">
   {
     "@context": "https://schema.org",
     "@graph": [
       {
         "@type": "Organization",
         "name": "Your Business",
         ...
       }
     ]
   }
   </script>
   ```
3. **Update all URLs** in the JSON-LD to match your live GHL domain

---

### Step 4: Page-by-Page Checklist

Use this checklist for EACH page before moving to the next:

```
Page: _____________ (e.g., Homepage, About, Blog, etc.)

CUSTOM CSS (Page Settings → Custom CSS):
  [ ] variables.css content pasted (Section 1)
  [ ] base.css content pasted with fallbacks (Section 2)
  [ ] Page-specific CSS pasted (Section 3)

CUSTOM CODE COMPONENTS:
  [ ] Header component (global/header.html) — at top
  [ ] Main content component (<main> from page's index.html) — in middle
  [ ] Footer component (global/footer.html) — at bottom
  [ ] JSON-LD component (if page has structured data) — at top

IMAGE URLS:
  [ ] All /assets/images/ paths replaced with GHL media URLs
  [ ] All /assets/og/ paths replaced with GHL media URLs
  [ ] Logo URLs updated in header and footer

INTERNAL LINKS:
  [ ] All href paths verified against GHL page slugs
  [ ] Nav links in header point to correct GHL pages
  [ ] Footer links point to correct GHL pages
  [ ] CTA buttons point to correct GHL pages

SEO:
  [ ] Page title set in GHL page settings
  [ ] Meta description set in GHL page settings
  [ ] OG image uploaded and set
  [ ] Canonical URL set (if needed)

VERIFY:
  [ ] Preview page — text is visible (not invisible/transparent)
  [ ] Preview page — header displays correctly with logo
  [ ] Preview page — mobile menu opens and closes
  [ ] Preview page — scroll animations trigger
  [ ] Preview page — footer displays correctly
  [ ] Published page — check actual URL in incognito browser
```

---

### Step 5: Handle Blog Posts

Blog posts require extra attention because each post is a separate GHL page.

**For each blog post:**
1. Create a new GHL page with slug matching the blog post slug:
   ```
   pages/blog/my-article-title/index.html → /blog/my-article-title
   ```

2. In **Custom CSS**, paste:
   - `variables.css` (same as every other page)
   - `base.css` (same as every other page)
   - `blog-post.css` (the shared blog post styles from `pages/blog/blog-post.css`)

3. Add Custom Code components:
   - Header (same `header.html` as all pages)
   - Blog post `<main>` content (from `pages/blog/[slug]/index.html`)
   - Footer (same `footer.html` as all pages)
   - JSON-LD for the article (Article schema from the post's `<head>`)

4. **Update related posts links** to point to the correct GHL blog post pages

**If you have many blog posts (10+),** consider creating a GHL "template" approach:
- Build the first blog post page completely
- Clone it for each subsequent post
- Only replace the `<main>` content and SEO settings
- This saves time since variables.css, base.css, header, footer, and blog-post.css stay the same

---

### Step 6: Final Site-Wide Verification

After all pages are built:

```
SITE SETTINGS:
  [ ] Header Tracking Code has GTM + fonts + favicon links
  [ ] Footer Tracking Code has GTM noscript + global-scripts.js
  [ ] Favicon is set in site settings
  [ ] Custom domain is connected (if applicable)
  [ ] SSL is enabled

EVERY PAGE (spot-check at minimum):
  [ ] Text is visible — no invisible/transparent text
  [ ] Colors match the original design (compare screenshots)
  [ ] Fonts are loading (check Network tab in DevTools for Google Fonts)
  [ ] Header renders correctly — desktop nav, mobile hamburger
  [ ] Footer renders correctly — links, social icons
  [ ] Scroll animations trigger (IntersectionObserver from global-scripts.js)
  [ ] Counter animations work (if page has counters)
  [ ] FAQ accordion opens/closes (if page has FAQ)
  [ ] Testimonial slider auto-rotates (if page has testimonials)
  [ ] Internal links navigate to correct pages
  [ ] Mobile responsive — check at 768px and 375px widths

SEO VERIFICATION:
  [ ] View page source — confirm JSON-LD is present
  [ ] Run Facebook Debugger (https://developers.facebook.com/tools/debug/)
  [ ] Run X Card Validator (https://cards-dev.twitter.com/validator)
  [ ] Check Google Rich Results Test (https://search.google.com/test/rich-results)
```

---

## 🚫 GHL COPY-PASTE — NEVER DO THESE

- **NEVER** paste `<link rel="stylesheet" href="/global/...">` into GHL — GHL doesn't serve files, paste the CSS content directly
- **NEVER** paste `<script src="/scripts/...">` into page components — put global JS in Footer Tracking Code only
- **NEVER** paste the full `<!DOCTYPE html>...<body>` wrapper — GHL provides its own, only paste inner content
- **NEVER** skip `variables.css` on any page — it will make text invisible
- **NEVER** assume CSS from one GHL page carries to another — every page is isolated
- **NEVER** paste header/footer CSS in the page's Custom CSS — header.html and footer.html are self-contained (they have their own `<style>` tags inside)
- **NEVER** use relative paths like `../../global/` or `/assets/` — replace ALL paths with absolute GHL media URLs
- **NEVER** copy-paste everything at once — follow the component order (header → main → footer) to debug issues section by section
- **NEVER** forget to update URLs inside JSON-LD to match the live GHL domain

---

## 📋 QUICK REFERENCE: WHAT GOES WHERE

| Repo File | GHL Location | How Often |
|---|---|---|
| `variables.css` | Custom CSS (top) | Every page |
| `base.css` | Custom CSS (after variables) | Every page |
| `[page].css` | Custom CSS (after base) | Once per page |
| `header.html` | Custom Code component (top) | Every page |
| `footer.html` | Custom Code component (bottom) | Every page |
| `<main>` content | Custom Code component (middle) | Once per page |
| `JSON-LD` from `<head>` | Custom Code component (top) | Once per page |
| `gtm-head.html` | Site Settings → Header Tracking | Once for whole site |
| `global-scripts.js` | Site Settings → Footer Tracking (in `<script>`) | Once for whole site |
| Images | GHL Media Library → get new URLs | Upload once, reference everywhere |
| Favicon | Site Settings → Favicon (or Header Tracking) | Once for whole site |

---

## 🔧 TROUBLESHOOTING: COMMON GHL ISSUES

### Text is invisible after pasting
**Cause:** `variables.css` is missing from that page's Custom CSS, so `var(--body)` and `var(--foreground)` resolve to nothing.
**Fix:** Paste `variables.css` content at the TOP of the page's Custom CSS. Always paste variables first, base second.

### Fonts not loading
**Cause:** Google Fonts `<link>` tag is missing from Site Settings Header Tracking Code.
**Fix:** Add the font `<link>` tags to Header Tracking Code. Verify in DevTools Network tab that the font files are downloading.

### Header/footer looks broken but page content is fine
**Cause:** Header/footer CSS is self-contained inside the `<style>` tag within `header.html`/`footer.html`, but GHL may strip `<style>` tags from Custom Code components in some configurations.
**Fix:** If `<style>` tags are stripped, move the header/footer CSS into the page's Custom CSS field (after base.css, before page-specific CSS). Then remove the `<style>` block from the HTML component, keeping only the markup and `<script>`.

### Scroll animations not working
**Cause:** `global-scripts.js` is not in the Footer Tracking Code, or it was pasted without the wrapping `<script>` tag.
**Fix:** Ensure the JS is wrapped in `<script>...</script>` in the Footer Tracking Code. Check browser console for errors.

### GHL builder shows broken layout but published page is fine
**Cause:** GHL's editor injects its own CSS that conflicts with the reset in `base.css`. This is normal.
**Fix:** Always verify on the **published page** (or preview mode), not the builder view. The builder view is not accurate for custom-coded pages.

### Margins/padding missing on GHL-native elements
**Cause:** The `* { margin: 0; padding: 0; }` reset in `base.css` strips default spacing from GHL's own components (if you mix custom code with GHL drag-and-drop elements).
**Fix:** Don't mix GHL's native drag-and-drop components with custom code components on the same page. Use either 100% custom code OR 100% GHL native — not both.

### Images not loading
**Cause:** HTML still has repo paths (`/assets/images/...`) instead of GHL media URLs.
**Fix:** Upload images to GHL Media Library and replace all paths. Use browser DevTools → Network tab to find 404s.

---

## 📝 COPY-PASTE ORDER — DO IN THIS EXACT SEQUENCE

For the entire site, follow this master sequence:

```
1.  Upload ALL images to GHL Media Library
2.  Create URL mapping document (old paths → new GHL URLs)
3.  Site Settings → Header Tracking Code (gtm-head.html contents)
4.  Site Settings → Footer Tracking Code (gtm-body.html + global-scripts.js in <script>)
5.  Site Settings → Favicon
6.  Create Homepage in GHL
7.  Homepage → Custom CSS (variables.css + base.css + home.css)
8.  Homepage → Custom Code: Header
9.  Homepage → Custom Code: Main content
10. Homepage → Custom Code: Footer
11. Homepage → Custom Code: JSON-LD
12. Homepage → SEO settings (title, description, OG image)
13. VERIFY Homepage (preview + publish + check text visibility)
14. Create About page → repeat steps 7-13 with about.css
15. Create Services page(s) → repeat
16. Create Contact page → repeat
17. Create Blog listing page → repeat
18. Create each Blog Post page → repeat (using blog-post.css)
19. Create Privacy Policy page → repeat
20. Create Terms of Service page → repeat
21. Create 404 page → repeat (set as custom 404 in site settings)
22. Site-wide verification (Step 6 checklist above)
23. SEO verification (Facebook Debugger, X Card Validator, Rich Results Test)
24. Domain connection + SSL
25. Final published site walkthrough
```
