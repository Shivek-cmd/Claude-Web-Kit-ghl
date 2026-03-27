# 05-SEO.md — Complete SEO System
> Load after 04-CONTENT.md. SEO wraps around content — not the other way around. Every rule here assumes the content is already written correctly.

---

## 🧠 SEO PHILOSOPHY

SEO in 2025+ is not about tricking algorithms. It's about:
- Creating the most helpful, specific, authoritative content on a topic
- Making that content technically discoverable and crawlable
- Building structured data that makes Google understand your content
- Ensuring every social share looks professional and drives clicks

**Never skip**: metadata, JSON-LD, OG images, canonical URLs, sitemap, breadcrumbs. All of these together — not individually.

---

## 🔍 METADATA SYSTEM

### Every Page `<head>` — Complete Meta Block

Paste this into every page's `<head>`. Fill every placeholder.

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- Primary SEO -->
  <title>[Page Title — 50-60 chars] | Business Name</title>
  <meta name="description" content="[150-160 char unique description. Keyword-rich. Ends with a subtle action.]">
  <meta name="robots" content="index, follow, max-image-preview:large, max-snippet:-1">
  <meta name="author" content="Business Name">
  <link rel="canonical" href="https://yoursite.com/[page-slug]">

  <!-- Open Graph — Facebook, WhatsApp, LinkedIn -->
  <meta property="og:type"         content="website">
  <meta property="og:site_name"    content="Business Name">
  <meta property="og:locale"       content="en_US">
  <meta property="og:title"        content="[Page Title] | Business Name">
  <meta property="og:description"  content="[OG description — can match meta description]">
  <meta property="og:url"          content="https://yoursite.com/[page-slug]">
  <meta property="og:image"        content="https://yoursite.com/assets/og/og-[page].jpg">
  <meta property="og:image:width"  content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:image:type"   content="image/jpeg">
  <meta property="og:image:alt"    content="[Descriptive alt for OG image]">

  <!-- Twitter / X Card -->
  <meta name="twitter:card"        content="summary_large_image">
  <meta name="twitter:site"        content="@yourtwitterhandle">
  <meta name="twitter:creator"     content="@yourtwitterhandle">
  <meta name="twitter:title"       content="[Page Title] | Business Name">
  <meta name="twitter:description" content="[Twitter description]">
  <meta name="twitter:image"       content="https://yoursite.com/assets/og/og-[page].jpg">

  <!-- JSON-LD Structured Data (page-specific — see below) -->
  <script type="application/ld+json">{ ... }</script>
</head>
```

### Blog Post — Additional Meta Tags
```html
<!-- Add these for blog posts only -->
<meta property="og:type"              content="article">
<meta property="article:published_time" content="2025-03-20T09:00:00Z">
<meta property="article:modified_time"  content="2025-03-20T09:00:00Z">
<meta property="article:author"       content="https://yoursite.com/about">
<meta property="article:section"      content="SEO">
<meta property="article:tag"          content="web design, seo, ghl">
```

---

## 📊 JSON-LD STRUCTURED DATA — Every Page, No Exceptions

Place the appropriate JSON-LD block inside a `<script type="application/ld+json">` tag in the `<head>` of every page. Use `window.addSchema()` from `global-scripts.js` as an alternative for inline page scripts.

### Organization Schema — Homepage + About Page
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Business Name",
  "url": "https://yoursite.com",
  "logo": "https://yoursite.com/assets/icons/android-chrome-512x512.png",
  "description": "Your 150-character business description.",
  "sameAs": [
    "https://twitter.com/handle",
    "https://linkedin.com/company/name",
    "https://instagram.com/handle",
    "https://facebook.com/page"
  ],
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "+1-555-000-0000",
    "contactType": "customer service",
    "availableLanguage": "English"
  }
}
</script>
```

### WebSite Schema — Homepage only
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "Business Name",
  "url": "https://yoursite.com",
  "potentialAction": {
    "@type": "SearchAction",
    "target": {
      "@type": "EntryPoint",
      "urlTemplate": "https://yoursite.com/search?q={search_term_string}"
    },
    "query-input": "required name=search_term_string"
  }
}
</script>
```

### Article Schema — Every Blog Post
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Blog Post Title Here",
  "description": "Post excerpt / meta description.",
  "datePublished": "2025-03-20T09:00:00Z",
  "dateModified": "2025-03-20T09:00:00Z",
  "author": {
    "@type": "Person",
    "name": "Author Full Name",
    "url": "https://yoursite.com/about"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Business Name",
    "logo": {
      "@type": "ImageObject",
      "url": "https://yoursite.com/assets/icons/android-chrome-512x512.png"
    }
  },
  "image": {
    "@type": "ImageObject",
    "url": "https://yoursite.com/assets/og/og-post-slug.jpg",
    "width": 1200,
    "height": 630
  },
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://yoursite.com/blog/post-slug"
  }
}
</script>
```

### FAQPage Schema — FAQ Section on Any Page
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "Question text exactly as shown on page?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Answer text exactly as shown on page. 40-60 words."
      }
    },
    {
      "@type": "Question",
      "name": "Second question?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Second answer text."
      }
    }
  ]
}
</script>
```

### BreadcrumbList Schema — All Inner Pages
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://yoursite.com"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Services",
      "item": "https://yoursite.com/services"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "Web Design",
      "item": "https://yoursite.com/services/web-design"
    }
  ]
}
</script>
```

### Service Schema — Each Service Page
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Service",
  "name": "Web Design Services",
  "description": "Premium web design services for businesses. Conversion-focused, SEO-optimised, built for speed.",
  "url": "https://yoursite.com/services/web-design",
  "provider": {
    "@type": "Organization",
    "name": "Business Name",
    "url": "https://yoursite.com"
  },
  "areaServed": "Worldwide",
  "serviceType": "Web Design"
}
</script>
```

### LocalBusiness Schema — Physical/Local Businesses
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Business Name",
  "url": "https://yoursite.com",
  "telephone": "+1-555-000-0000",
  "email": "hello@yoursite.com",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main Street",
    "addressLocality": "City",
    "addressRegion": "State",
    "postalCode": "00000",
    "addressCountry": "US"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 40.7128,
    "longitude": -74.0060
  },
  "openingHours": "Mo-Fr 09:00-18:00",
  "priceRange": "$$"
}
</script>
```

### Person Schema — Blog Authors
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Author Full Name",
  "url": "https://yoursite.com/about",
  "image": "https://yoursite.com/assets/images/author-photo.jpg",
  "jobTitle": "Job Title",
  "worksFor": {
    "@type": "Organization",
    "name": "Business Name"
  },
  "sameAs": [
    "https://twitter.com/handle",
    "https://linkedin.com/in/handle"
  ]
}
</script>
```

### AggregateRating Schema — Testimonials Section
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Business Name",
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.9",
    "reviewCount": "87",
    "bestRating": "5",
    "worstRating": "1"
  }
}
</script>
```

---

## 🗺️ SITEMAP — sitemap.xml

GHL auto-generates a sitemap. Verify it at `https://yoursite.com/sitemap.xml`. If it doesn't include all pages, submit a manual sitemap to Google Search Console.

Manual `sitemap.xml` template:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://yoursite.com/</loc>
    <lastmod>2025-03-01</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://yoursite.com/about</loc>
    <lastmod>2025-03-01</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://yoursite.com/services</loc>
    <lastmod>2025-03-01</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://yoursite.com/blog</loc>
    <lastmod>2025-03-20</lastmod>
    <changefreq>daily</changefreq>
    <priority>0.9</priority>
  </url>
  <!-- Add each blog post and page -->
</urlset>
```

## 🤖 ROBOTS.TXT

In GHL, add a Custom Value or use a Custom File. Content:
```
User-agent: *
Allow: /
Disallow: /admin/
Disallow: /api/
Sitemap: https://yoursite.com/sitemap.xml
```

---

## 🖼️ OG IMAGE SYSTEM

### Requirements
- Size: exactly **1200 × 630px**
- Format: JPEG (smaller file size than PNG for photos)
- Max file size: 5MB (platforms skip silently above this)
- Keep all content within 100px of edges (gets cropped on some platforms)
- Always use **full absolute URL** — never a relative path
- One unique OG image per main page

### File naming in `/assets/og/`
```
og-default.jpg    ← fallback for any page without a specific image
og-home.jpg
og-about.jpg
og-services.jpg
og-portfolio.jpg
og-blog.jpg
og-contact.jpg
og-[post-slug].jpg   ← one per blog post
```

### Testing Social Preview Cards — Always Before Launch
| Tool | Platform | URL |
|------|----------|-----|
| Facebook Sharing Debugger | Facebook, WhatsApp | developers.facebook.com/tools/debug |
| X Card Validator | Twitter/X | cards-dev.twitter.com/validator |
| LinkedIn Post Inspector | LinkedIn | linkedin.com/post-inspector |
| OpenGraph.xyz | All platforms | opengraph.xyz |
| metatags.io | Visual preview | metatags.io |

**Testing checklist:**
- [ ] Facebook Sharing Debugger: correct title, description, image
- [ ] Twitter Card Validator: shows `summary_large_image`
- [ ] LinkedIn Post Inspector: correct 1.91:1 image
- [ ] WhatsApp: actually send in a chat to test
- [ ] iMessage: send link to your own phone

---

## 📐 HEADING HIERARCHY — SEO CRITICAL

Google uses headings to understand page structure. Never violate this:

- **H1**: Exactly ONE per page. Primary keyword ideally in first 3 words. Never in navbar/footer.
- **H2**: Main section headings. Each naturally contains a secondary keyword.
- **H3**: Sub-sections within H2 only. FAQs, feature sub-points.
- **Never skip levels** — never jump H1 → H3.

```html
<!-- Homepage -->
<h1>Web Design Agency for Fast-Growing Businesses</h1>
<h2>Our Services</h2>
  <h3>Web Design</h3>
  <h3>SEO</h3>
<h2>What Our Clients Say</h2>
<h2>Frequently Asked Questions</h2>
```

---

## 🔗 URL SLUG RULES

- Always lowercase
- Hyphens only — no underscores
- Keywords in URL — not IDs
- Short: max 4-5 words
- No stop words (a, the, and, of)
- No dates in page URLs
- Blog slugs: primary keyword at the start

---

## 🔗 INTERNAL LINKING — SEO LEVEL

1. Navbar → all main pages
2. Footer → full sitemap + 3 recent blog posts
3. Homepage → services detail, latest 3 blog posts, about, contact
4. Blog posts → 3 related posts + 2-4 contextual links in body
5. Services pages → each links to 1 relevant blog post
6. Every section CTA → `/contact` with descriptive anchor text
7. Breadcrumbs on all inner pages with `BreadcrumbList` JSON-LD

**Anchor text — always descriptive:**
- ✅ "learn about our gold export compliance process"
- ❌ "click here" / "read more"

---

## 🔍 GOOGLE SEARCH CONSOLE

Add the verification tag to every page's `<head>` (in `gtm-head.html`):
```html
<meta name="google-site-verification" content="your-verification-code">
```
Or upload the HTML verification file to GHL's assets.

---

## 🚫 SEO — NEVER DO THESE

- **NEVER** use relative path for `og:image` — WhatsApp won't load it (use full `https://` URL)
- **NEVER** use same `og:image` on every page — unique images perform better
- **NEVER** skip `twitter:card = summary_large_image` — shows tiny thumbnail
- **NEVER** use OG image over 5MB — platforms skip it silently
- **NEVER** put content within 100px of OG image edges — gets cropped
- **NEVER** skip testing in Facebook Debugger + X Card Validator before launch
- **NEVER** skip `<link rel="canonical">` — duplicate content confusion
- **NEVER** use duplicate meta titles across pages
- **NEVER** write meta description longer than 160 characters
- **NEVER** skip the meta description
- **NEVER** skip JSON-LD on any page
- **NEVER** skip breadcrumbs on inner pages
- **NEVER** keyword stuff — Google penalises
- **NEVER** target same keyword on two pages (cannibalism)
- **NEVER** ship without sitemap.xml and robots.txt
- **NEVER** skip `<link rel="canonical">` on blog category/tag/pagination pages
