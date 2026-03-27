# 03-BUILD-WEBSITE.md — Build The Complete Website
> Load after 00-CORE + 01-SETUP + 02-DESIGN-SYSTEM. Covers every component, section, page pattern, and blog system.

---

## 🧠 BUILD PHILOSOPHY

Build in this exact order:
1. `variables.css` + `base.css` — design tokens and utilities
2. `global/header.html` + `global/footer.html` — layout shell
3. `scripts/global-scripts.js` — all reusable JS behaviors
4. Page CSS files, then HTML files — assign scroll + spatial pattern BEFORE coding each section
5. Blog pages last (most complex)

**Before coding each section:** Look at 02-DESIGN-SYSTEM Pattern Selection Table. Assign the scroll pattern and spatial pattern first. Never code a section without knowing which pattern it uses.

---

## 🔘 REUSABLE HTML COMPONENTS

### Scroll Reveal — Any Section
```html
<!-- Simple reveal -->
<section class="section reveal">
  <div class="container">...</div>
</section>

<!-- Stagger children -->
<div class="grid-3col">
  <div class="card reveal delay-1">Card 1</div>
  <div class="card reveal delay-2">Card 2</div>
  <div class="card reveal delay-3">Card 3</div>
</div>
```
Handled automatically by `global-scripts.js` IntersectionObserver. No extra JS needed per page.

---

### Animated Counter
```html
<div class="stat-item reveal">
  <span class="stat-number">
    <span class="counter" data-target="250" data-suffix="+">0</span>
  </span>
  <p class="stat-label">Clients Served</p>
</div>
```
Handled automatically by `global-scripts.js`.

---

### Button Variants
```html
<!-- Primary CTA -->
<a href="/contact" class="btn btn-primary">Get a Free Quote</a>

<!-- Secondary -->
<a href="/services" class="btn btn-secondary">View Services</a>

<!-- Outline -->
<a href="/portfolio" class="btn btn-outline">See Our Work</a>

<!-- Ghost (text only) -->
<a href="/blog" class="btn btn-ghost">Read the Blog →</a>

<!-- Large -->
<a href="/contact" class="btn btn-primary btn-lg">Start Your Project</a>
```

---

### Card
```html
<div class="card">
  <div class="service-icon" aria-hidden="true">
    <svg>...</svg>
  </div>
  <h3 class="service-title">Service Name</h3>
  <p class="service-desc">Short description of what this service delivers.</p>
  <a href="/services/slug" class="card-link">
    Learn more
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/></svg>
  </a>
</div>
```

---

### Badge
```html
<span class="badge">Web Design</span>
```

---

### Breadcrumb (all inner pages)
```html
<nav class="breadcrumb" aria-label="Breadcrumb">
  <a href="/">Home</a>
  <span class="sep" aria-hidden="true">›</span>
  <a href="/services">Services</a>
  <span class="sep" aria-hidden="true">›</span>
  <span class="current" aria-current="page">Web Design</span>
</nav>
```

---

### Form Inputs
```html
<div class="form-group">
  <label class="form-label" for="name">
    Full Name <span class="required" aria-hidden="true">*</span>
  </label>
  <input class="form-input" type="text" id="name" name="name"
    placeholder="Your full name" required autocomplete="name">
  <p class="form-error" id="name-error" role="alert" style="display:none">
    ⚠ Please enter your name.
  </p>
</div>

<div class="form-group">
  <label class="form-label" for="message">Message <span class="required">*</span></label>
  <textarea class="form-textarea" id="message" name="message"
    placeholder="Tell us about your project..." required rows="5"></textarea>
</div>
```

---

### WhatsApp Button
Already included in `footer.html`. Replace `YOURPHONENUMBER` with international format (e.g. `919876543210` for India).

---

## 🧭 NAVBAR — header.html

Already self-contained in `global/header.html`. Includes:
- Sticky + blur-on-scroll
- Active link detection (matches `window.location.pathname`)
- Mobile hamburger drawer
- Theme toggle (dark/light, persists via `localStorage`)
- Skip nav link
- Scroll progress bar (`#scroll-progress`)

**To customise:**
- Change logo text inside `.nav-logo`
- Change nav links inside `.nav-links` (desktop) and `.mobile-menu` (mobile)
- Change CTA button text and href

---

## 🏠 HOMEPAGE — sections in order

### 1. Hero Section
```html
<section class="hero section" id="hero">
  <!-- Desktop rings decoration -->
  <div class="hero-rings" aria-hidden="true">
    <svg viewBox="0 0 900 900" xmlns="http://www.w3.org/2000/svg" width="900" height="900">
      <circle cx="450" cy="450" r="200" fill="none" stroke="currentColor" stroke-width="1"/>
      <circle cx="450" cy="450" r="320" fill="none" stroke="currentColor" stroke-width="1"/>
      <circle cx="450" cy="450" r="420" fill="none" stroke="currentColor" stroke-width="1" stroke-dasharray="4 8"/>
    </svg>
  </div>

  <!-- Ambient glow -->
  <div class="hero-glow" aria-hidden="true"></div>

  <div class="container">
    <div class="hero-content">
      <!-- Overline pill -->
      <div class="hero-label">
        <span class="dot"></span>
        Now accepting new clients for 2025
      </div>

      <!-- H1 — one per page, primary keyword first -->
      <h1>
        Premium Web Design<br>
        for <span class="text-shimmer">Ambitious</span> Brands
      </h1>

      <p class="hero-sub">
        We build conversion-focused websites that rank on Google and turn visitors into customers.
        Trusted by 200+ businesses worldwide.
      </p>

      <div class="hero-cta">
        <a href="/contact" class="btn btn-primary btn-lg">Start Your Project</a>
        <a href="/portfolio" class="btn btn-secondary btn-lg">View Our Work</a>
      </div>
    </div>
  </div>

  <!-- Scroll indicator -->
  <div class="scroll-indicator" aria-hidden="true">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <polyline points="6 9 12 15 18 9"/>
    </svg>
    Scroll
  </div>
</section>
```

---

### 2. Logo Marquee Strip
```html
<section class="marquee-section" aria-label="Trusted by leading brands">
  <p class="marquee-label">Trusted by</p>
  <div class="marquee-wrapper">
    <div class="marquee-track">
      <!-- Duplicate all items for seamless loop -->
      <span class="marquee-item">Acme Corp<span class="marquee-dot"></span></span>
      <span class="marquee-item">GlobalTech<span class="marquee-dot"></span></span>
      <span class="marquee-item">Nexus Group<span class="marquee-dot"></span></span>
      <span class="marquee-item">Verilink<span class="marquee-dot"></span></span>
      <span class="marquee-item">Stratera<span class="marquee-dot"></span></span>
      <!-- Duplicate start -->
      <span class="marquee-item">Acme Corp<span class="marquee-dot"></span></span>
      <span class="marquee-item">GlobalTech<span class="marquee-dot"></span></span>
      <span class="marquee-item">Nexus Group<span class="marquee-dot"></span></span>
      <span class="marquee-item">Verilink<span class="marquee-dot"></span></span>
      <span class="marquee-item">Stratera<span class="marquee-dot"></span></span>
    </div>
  </div>
</section>
```

---

### 3. Text Island (Mission)
```html
<section class="section text-island">
  <div class="container" style="max-width:800px">
    <span class="section-label reveal">Who We Are</span>
    <p class="text-island-text reveal delay-1">
      We are a boutique digital agency obsessed with one thing —
      <em>building websites that actually grow your business.</em>
      Not just beautiful. Fast, found, and converting.
    </p>
  </div>
</section>
```

---

### 4. Stats Section
```html
<section class="section stats-section">
  <div class="container">
    <div class="stats-grid">
      <div class="stat-item reveal delay-1">
        <span class="stat-number">
          <span class="counter" data-target="247" data-suffix="+">0</span>
        </span>
        <p class="stat-label">Projects delivered</p>
      </div>
      <div class="stat-item reveal delay-2">
        <span class="stat-number">
          <span class="counter" data-target="98" data-suffix="%">0</span>
        </span>
        <p class="stat-label">Client satisfaction rate</p>
      </div>
      <div class="stat-item reveal delay-3">
        <span class="stat-number">
          <span class="counter" data-target="12" data-suffix="">0</span>
        </span>
        <p class="stat-label">Years in business</p>
      </div>
      <div class="stat-item reveal delay-4">
        <span class="stat-number">
          <span class="counter" data-target="50" data-suffix="+">0</span>
        </span>
        <p class="stat-label">Countries served</p>
      </div>
    </div>
  </div>
</section>
```

---

### 5. Services Section
```html
<section class="section services-section" id="services">
  <div class="container">
    <div class="section-header reveal">
      <span class="section-label">What We Do</span>
      <h2>Services built to grow your business</h2>
      <p class="section-sub">
        Every service is designed with one goal: measurable results for your business.
      </p>
    </div>

    <div class="services-grid">
      <a href="/services/web-design" class="card service-card reveal delay-1">
        <div class="service-icon" aria-hidden="true">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="2" y="3" width="20" height="14" rx="2"/><line x1="8" y1="21" x2="16" y2="21"/><line x1="12" y1="17" x2="12" y2="21"/></svg>
        </div>
        <h3 class="service-title">Web Design</h3>
        <p class="service-desc">Pixel-perfect websites that convert visitors into leads. Built for speed, SEO, and conversion.</p>
        <span class="card-link">
          Learn more
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true"><line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/></svg>
        </span>
      </a>

      <a href="/services/seo" class="card service-card reveal delay-2">
        <div class="service-icon" aria-hidden="true">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
        </div>
        <h3 class="service-title">SEO</h3>
        <p class="service-desc">Rank on page 1. We combine technical SEO, content strategy, and link building to grow organic traffic.</p>
        <span class="card-link">
          Learn more
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true"><line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/></svg>
        </span>
      </a>

      <!-- Add more service cards following the same pattern -->
    </div>
  </div>
</section>
```

---

### 6. Testimonials Section (JS Slider)
```html
<section class="section testimonials-section" id="testimonials">
  <div class="container">
    <div class="section-header reveal">
      <span class="section-label">Client Results</span>
      <h2>What our clients say</h2>
    </div>

    <div class="testimonial-slider reveal delay-1">
      <div class="testimonial-track">

        <div class="testimonial-slide">
          <div class="testimonial-stars" aria-label="5 out of 5 stars">
            <!-- 5 star SVGs -->
            <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
            <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
            <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
            <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
            <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
          </div>
          <blockquote class="testimonial-quote">
            &ldquo;Our organic traffic tripled in 6 months. The team delivered exactly what they promised — a site that actually ranks and converts.&rdquo;
          </blockquote>
          <div class="testimonial-author">
            <img src="/assets/images/avatar-sarah.jpg" alt="Sarah Johnson, CEO of Acme Corp" width="48" height="48" class="testimonial-avatar" loading="lazy">
            <div>
              <p class="testimonial-name">Sarah Johnson</p>
              <p class="testimonial-role">CEO, Acme Corp</p>
            </div>
          </div>
        </div>

        <!-- Add more .testimonial-slide divs -->

      </div>

      <!-- Slider controls (wired up by global-scripts.js) -->
      <div class="slider-controls">
        <button class="slider-prev" aria-label="Previous testimonial">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true"><polyline points="15 18 9 12 15 6"/></svg>
        </button>
        <div class="slider-dots" role="tablist" aria-label="Testimonial navigation"></div>
        <button class="slider-next" aria-label="Next testimonial">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true"><polyline points="9 18 15 12 9 6"/></svg>
        </button>
      </div>
    </div>
  </div>
</section>
```

---

### 7. FAQ Section (JS Accordion + JSON-LD)
```html
<section class="section faq-section" id="faq">
  <div class="container" style="max-width:800px">
    <div class="section-header reveal">
      <span class="section-label">FAQ</span>
      <h2>Frequently asked questions</h2>
    </div>

    <div class="faq-list reveal delay-1">
      <div class="faq-item">
        <button class="faq-trigger" aria-expanded="false">
          How long does a website project take?
          <svg class="faq-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true"><polyline points="6 9 12 15 18 9"/></svg>
        </button>
        <div class="faq-body">
          <p>Most projects take 4–8 weeks from kickoff to launch, depending on scope. A standard 5-page business website typically takes 4 weeks. Complex sites with custom features take 8–12 weeks.</p>
        </div>
      </div>

      <div class="faq-item">
        <button class="faq-trigger" aria-expanded="false">
          Do you work with GoHighLevel?
          <svg class="faq-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true"><polyline points="6 9 12 15 18 9"/></svg>
        </button>
        <div class="faq-body">
          <p>Yes. All our websites are built in clean HTML, CSS, and JavaScript — fully compatible with GoHighLevel. We also set up CRM, automations, and landing pages inside GHL.</p>
        </div>
      </div>

      <!-- Add more .faq-item blocks -->
    </div>
  </div>
</section>

<!-- FAQ JSON-LD — placed right after the FAQ section -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "How long does a website project take?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Most projects take 4–8 weeks from kickoff to launch, depending on scope."
      }
    },
    {
      "@type": "Question",
      "name": "Do you work with GoHighLevel?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Yes. All our websites are built in clean HTML, CSS, and JavaScript — fully compatible with GoHighLevel."
      }
    }
  ]
}
</script>
```

---

### 8. CTA Banner
```html
<section class="section cta-section" id="cta">
  <!-- Concentric rings — desktop only -->
  <div class="cta-rings" aria-hidden="true">
    <svg viewBox="0 0 700 700" xmlns="http://www.w3.org/2000/svg">
      <circle cx="350" cy="350" r="150" fill="none" stroke="currentColor" stroke-width="1"/>
      <circle cx="350" cy="350" r="250" fill="none" stroke="currentColor" stroke-width="1"/>
      <circle cx="350" cy="350" r="330" fill="none" stroke="currentColor" stroke-width="1" stroke-dasharray="4 8"/>
    </svg>
  </div>

  <div class="container">
    <div class="cta-content reveal">
      <span class="section-label">Ready to grow?</span>
      <h2 class="cta-heading">Let&rsquo;s build something remarkable</h2>
      <p class="cta-sub">
        We take on 4 new projects per month. Spots fill fast — secure yours now.
      </p>
      <div class="cta-buttons">
        <a href="/contact" class="btn btn-primary btn-lg">Start Your Project</a>
        <a href="tel:+YOURPHONENUMBER" class="btn btn-secondary btn-lg">Call Us Now</a>
      </div>
    </div>
  </div>
</section>
```

---

## 📝 BLOG SYSTEM

No MDX, no build step. Blog posts are plain HTML pages.

### Blog Listing Page
```html
<section class="section blog-listing">
  <div class="container">
    <h1 class="reveal">Blog & Insights</h1>

    <div class="blog-grid">
      <article class="blog-card reveal delay-1">
        <a href="/blog/post-slug">
          <div class="blog-card-image">
            <img src="/assets/images/blog-post-name.jpg"
              alt="Descriptive alt text with keyword"
              width="800" height="500" loading="lazy">
          </div>
          <div class="blog-card-body">
            <div class="blog-card-meta">
              <span class="badge">SEO</span>
              <span class="blog-date">March 20, 2025</span>
              <span class="blog-read-time">5 min read</span>
            </div>
            <h2 class="blog-card-title">Blog Post Title Here — Keyword First</h2>
            <p class="blog-card-excerpt">
              Short excerpt summarising the post. 2-3 sentences. Compelling, specific.
            </p>
            <span class="card-link">
              Read the article
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true"><line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/></svg>
            </span>
          </div>
        </a>
      </article>
      <!-- Repeat .blog-card for each post -->
    </div>
  </div>
</section>
```

### Individual Blog Post Page
```html
<!-- In <head>: add Article JSON-LD (see 05-SEO.md) -->

<section class="blog-post-hero section">
  <div class="container" style="max-width:900px">
    <nav class="breadcrumb" aria-label="Breadcrumb">
      <a href="/">Home</a>
      <span class="sep" aria-hidden="true">›</span>
      <a href="/blog">Blog</a>
      <span class="sep" aria-hidden="true">›</span>
      <span class="current" aria-current="page">Post Title</span>
    </nav>
    <span class="badge">SEO</span>
    <h1 class="reveal">Blog Post Title — Primary Keyword First</h1>
    <div class="post-meta reveal delay-1">
      <img src="/assets/images/author-photo.jpg" alt="Author Name" width="40" height="40" class="author-avatar" loading="lazy">
      <div>
        <p class="author-name">Author Name</p>
        <p class="post-date">March 20, 2025 · 8 min read</p>
      </div>
    </div>
    <img src="/assets/og/og-post-name.jpg" alt="Post hero image description" width="1200" height="630" class="post-hero-img" loading="eager">
  </div>
</section>

<!-- Layout: TOC sidebar + article body -->
<div class="blog-post-layout container" style="max-width:1100px">

  <!-- Table of Contents (auto-built by global-scripts.js) -->
  <aside class="toc-sidebar" id="toc-container" aria-label="Table of contents">
    <p class="toc-title">On This Page</p>
    <!-- JS fills this in -->
  </aside>

  <!-- Article body -->
  <article class="post-body" id="post-body">
    <h2 id="section-one">First H2 Section</h2>
    <p>Content here...</p>

    <h2 id="section-two">Second H2 Section</h2>
    <p>Content here...</p>

    <!-- Author bio at end -->
    <div class="author-bio">
      <img src="/assets/images/author-photo.jpg" alt="Author Name" width="64" height="64" loading="lazy">
      <div>
        <p class="author-bio-name">Author Name</p>
        <p class="author-bio-title">Job Title, Business Name</p>
        <p class="author-bio-text">Short bio — 50-100 words. Who they are, years of experience, specific expertise area.</p>
      </div>
    </div>
  </article>

</div>

<!-- Related Posts -->
<section class="section related-posts">
  <div class="container">
    <h2 class="reveal">Related Articles</h2>
    <div class="blog-grid">
      <!-- 3 related blog cards -->
    </div>
  </div>
</section>
```

---

## 📄 ADDITIONAL PAGE TEMPLATES

### 404 Page
```html
<main id="main">
  <section class="section not-found-section">
    <div class="container" style="text-align:center; max-width:560px">
      <p class="not-found-number" aria-hidden="true">404</p>
      <h1>Page Not Found</h1>
      <p class="not-found-sub">
        The page you&rsquo;re looking for doesn&rsquo;t exist or has been moved.
      </p>
      <div class="not-found-cta">
        <a href="/" class="btn btn-primary">Go Home</a>
        <a href="/contact" class="btn btn-ghost">Contact Us</a>
      </div>
    </div>
  </section>
</main>
```

---

## 📐 SEO RULES — ALWAYS IN EVERY PAGE

- **H1**: Exactly ONE per page. Primary keyword ideally in first 3 words.
- **H2**: Main section headings. Each naturally contains a secondary keyword.
- **H3**: Sub-sections within H2 only. FAQs, feature sub-points.
- **Never skip levels** — never jump H1 → H3.
- Every page: unique `<title>` + `<meta name="description">` + `<link rel="canonical">`
- JSON-LD `<script type="application/ld+json">` on every page
- Breadcrumbs on all inner pages
- All internal links: descriptive anchor text, never "click here"

---

## 📱 WHATSAPP BUTTON

Already in `footer.html`. Update the phone number:
```html
href="https://wa.me/YOURPHONENUMBER?text=Hi%21%20I%20found%20you%20through%20your%20website."
```
Use full international format without `+` or spaces: e.g. `919876543210`
