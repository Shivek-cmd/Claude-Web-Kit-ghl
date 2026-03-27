# 06-ADDITIONAL.md — Advanced Patterns, Performance & Customizations
> Load this when needed for specific advanced requirements. Not needed on every project.

---

## ⚡ PERFORMANCE — ADVANCED PATTERNS

### Core Web Vitals Targets
- LCP (Largest Contentful Paint): < 2.5s
- CLS (Cumulative Layout Shift): < 0.1
- INP (Interaction to Next Paint): < 200ms

### Image Optimization
```html
<!-- Hero image — never lazy load above fold -->
<img
  src="/assets/images/web-design-agency-hero.jpg"
  alt="Web design agency team collaborating on a client project"
  width="1920"
  height="1080"
  loading="eager"
  fetchpriority="high"
>

<!-- All other images — always lazy load -->
<img
  src="/assets/images/service-web-design.jpg"
  alt="Web design process showing wireframes and mockups"
  width="800"
  height="600"
  loading="lazy"
>

<!-- Aspect ratio reserve — prevent CLS -->
<div style="aspect-ratio: 4/3; background: var(--color-bg-elevated); border-radius: var(--radius-xl); overflow: hidden;">
  <img src="..." alt="..." width="800" height="600" loading="lazy" style="width:100%; height:100%; object-fit:cover;">
</div>
```

### Font Loading — Prevent Flash
Always use the `preload` + `onload` swap trick (already in `gtm-head.html`):
```html
<link rel="preload"
  href="https://fonts.googleapis.com/css2?family=..."
  as="style"
  onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="..."></noscript>
```
Never use `@import` in CSS for fonts — it blocks rendering.

### Script Loading
```html
<!-- All non-critical scripts: defer -->
<script src="/scripts/global-scripts.js" defer></script>

<!-- Third-party scripts: async -->
<script src="https://cdn.example.com/widget.js" async></script>

<!-- Critical inline scripts (theme flash prevention): in <head>, synchronous -->
<script>
  (function(){ var t = localStorage.getItem("theme") || "dark"; document.documentElement.setAttribute("data-theme", t); })();
</script>
```

### CSS Performance
```css
/* Contain layout shifts from animations */
.reveal {
  will-change: opacity, transform; /* only on animated elements */
  contain: layout style;           /* isolate repaint */
}

/* Remove will-change after animation completes (via JS) */
```
```js
// In global-scripts.js observer:
entry.target.classList.add("visible");
setTimeout(() => { entry.target.style.willChange = "auto"; }, 800);
```

---

## 🎬 VIDEO BACKGROUND HERO — PATTERN

```html
<section class="hero section" style="position:relative; overflow:hidden;">
  <video
    autoplay muted loop playsinline
    poster="/assets/images/hero-poster.jpg"
    style="position:absolute; inset:0; width:100%; height:100%; object-fit:cover; opacity:0.3;"
  >
    <source src="/assets/videos/hero.webm" type="video/webm">
    <source src="/assets/videos/hero.mp4" type="video/mp4">
  </video>
  <div style="position:absolute; inset:0; background:linear-gradient(to bottom, rgba(10,10,10,0.7), rgba(10,10,10,0.9));"></div>
  <div class="container" style="position:relative; z-index:1;">
    <!-- hero content -->
  </div>
</section>
```
**Rules:**
- Always include `poster` attribute (shows while video loads — prevents CLS)
- Provide both `.webm` (smaller) and `.mp4` (fallback)
- Always `muted` — required for autoplay in all browsers
- Video opacity 0.2–0.4 — decorative, never distract from text

---

## 🎯 CONTACT FORM — GHL INTEGRATION

GHL provides its own form builder. Use GHL forms for actual submissions (connected to CRM).

For custom-styled forms that submit to GHL:

```html
<!-- Option A: Embed GHL form with custom CSS wrapper -->
<div class="ghl-form-wrapper">
  <!-- Paste GHL form embed code here -->
  <!-- GHL generates an <iframe> or JS embed -->
</div>

<!-- Option B: Custom HTML form posting to GHL webhook -->
<form id="contact-form" class="contact-form" novalidate>
  <div class="form-row">
    <div class="form-group">
      <label class="form-label" for="name">Full Name <span class="required">*</span></label>
      <input class="form-input" type="text" id="name" name="name" placeholder="Your full name" required autocomplete="name">
      <p class="form-error" id="name-error" role="alert" style="display:none">Please enter your name.</p>
    </div>
    <div class="form-group">
      <label class="form-label" for="email">Email <span class="required">*</span></label>
      <input class="form-input" type="email" id="email" name="email" placeholder="your@email.com" required autocomplete="email">
      <p class="form-error" id="email-error" role="alert" style="display:none">Please enter a valid email.</p>
    </div>
  </div>
  <div class="form-group">
    <label class="form-label" for="phone">Phone Number</label>
    <input class="form-input" type="tel" id="phone" name="phone" placeholder="+1 (555) 000-0000" autocomplete="tel">
  </div>
  <div class="form-group">
    <label class="form-label" for="message">Message <span class="required">*</span></label>
    <textarea class="form-textarea" id="message" name="message" rows="5" placeholder="Tell us about your project..." required></textarea>
  </div>
  <button type="submit" class="btn btn-primary btn-lg" id="form-submit">
    Send Message
    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true"><line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></svg>
  </button>
  <p class="form-success" id="form-success" role="status" aria-live="polite" style="display:none; color:var(--color-success); margin-top:1rem;">
    ✓ Thank you! We'll get back to you within 24 hours.
  </p>
</form>

<script>
(function() {
  var form = document.getElementById("contact-form");
  if (!form) return;

  form.addEventListener("submit", function(e) {
    e.preventDefault();
    var valid = true;

    // Validate name
    var name = document.getElementById("name");
    if (!name.value.trim()) {
      document.getElementById("name-error").style.display = "block";
      valid = false;
    } else {
      document.getElementById("name-error").style.display = "none";
    }

    // Validate email
    var email = document.getElementById("email");
    if (!email.value.match(/^[^\s@]+@[^\s@]+\.[^\s@]+$/)) {
      document.getElementById("email-error").style.display = "block";
      valid = false;
    } else {
      document.getElementById("email-error").style.display = "none";
    }

    if (!valid) return;

    // Submit to GHL webhook (replace URL)
    var btn = document.getElementById("form-submit");
    btn.textContent = "Sending...";
    btn.disabled = true;

    fetch("https://services.leadconnectorhq.com/hooks/YOUR-WEBHOOK-ID/webhook-trigger/YOUR-TRIGGER-ID", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        name: name.value.trim(),
        email: email.value.trim(),
        phone: document.getElementById("phone").value.trim(),
        message: document.getElementById("message").value.trim()
      })
    })
    .then(function() {
      form.style.display = "none";
      document.getElementById("form-success").style.display = "block";
    })
    .catch(function() {
      btn.textContent = "Send Message";
      btn.disabled = false;
      alert("Something went wrong. Please try again or email us directly.");
    });
  });
})();
</script>
```

---

## 💰 PRICING SECTION

```html
<section class="section pricing-section" id="pricing">
  <div class="container">
    <div class="section-header reveal">
      <span class="section-label">Pricing</span>
      <h2>Simple, transparent pricing</h2>
    </div>

    <div class="pricing-grid">

      <!-- Standard card -->
      <div class="pricing-card reveal delay-1">
        <p class="pricing-name">Starter</p>
        <div class="pricing-price">
          <span class="pricing-amount">$999</span>
          <span class="pricing-period">one-time</span>
        </div>
        <p class="pricing-desc">For small businesses getting started online.</p>
        <ul class="pricing-features">
          <li>
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" aria-hidden="true"><polyline points="20 6 9 17 4 12"/></svg>
            5-page website
          </li>
          <li>
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" aria-hidden="true"><polyline points="20 6 9 17 4 12"/></svg>
            Mobile responsive
          </li>
          <li>
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" aria-hidden="true"><polyline points="20 6 9 17 4 12"/></svg>
            Basic SEO setup
          </li>
        </ul>
        <a href="/contact" class="btn btn-secondary" style="width:100%">Get Started</a>
      </div>

      <!-- Highlighted card (most popular) -->
      <div class="pricing-card pricing-card--highlighted reveal delay-2">
        <span class="pricing-badge">Most Popular</span>
        <p class="pricing-name">Professional</p>
        <div class="pricing-price">
          <span class="pricing-amount">$2,499</span>
          <span class="pricing-period">one-time</span>
        </div>
        <p class="pricing-desc">For growing businesses that want to rank and convert.</p>
        <ul class="pricing-features">
          <li>
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" aria-hidden="true"><polyline points="20 6 9 17 4 12"/></svg>
            10-page website
          </li>
          <li>
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" aria-hidden="true"><polyline points="20 6 9 17 4 12"/></svg>
            Full SEO + blog setup
          </li>
          <li>
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" aria-hidden="true"><polyline points="20 6 9 17 4 12"/></svg>
            Priority support
          </li>
        </ul>
        <a href="/contact" class="btn btn-primary" style="width:100%">Get Started</a>
      </div>

    </div>
  </div>
</section>
```

```css
/* Pricing CSS — add to the relevant page CSS */
.pricing-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-6);
  margin-top: var(--space-12);
  align-items: start;
}

.pricing-card {
  background: var(--color-card);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-xl);
  padding: var(--space-8);
  display: flex;
  flex-direction: column;
  gap: var(--space-4);
  position: relative;
}

.pricing-card--highlighted {
  border-color: var(--color-primary);
  background: color-mix(in srgb, var(--color-primary) 6%, var(--color-card));
  transform: scale(1.02);
  box-shadow: var(--shadow-lg);
}

.pricing-badge {
  position: absolute;
  top: -12px;
  left: 50%;
  transform: translateX(-50%);
  background: var(--color-primary);
  color: #fff;
  font-size: var(--text-xs);
  font-weight: 600;
  letter-spacing: var(--tracking-wider);
  text-transform: uppercase;
  padding: 0.2rem 0.9rem;
  border-radius: var(--radius-full);
  white-space: nowrap;
}

.pricing-amount {
  font-family: var(--font-display);
  font-size: var(--text-3xl);
  font-weight: 700;
  color: var(--color-text);
}
.pricing-period {
  font-size: var(--text-sm);
  color: var(--color-text-muted);
  margin-left: var(--space-1);
}

.pricing-features {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
  flex: 1;
}
.pricing-features li {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  font-size: var(--text-sm);
  color: var(--color-text-muted);
}
.pricing-features svg { color: var(--color-success); flex-shrink: 0; }
```

---

## 🔔 TOAST NOTIFICATIONS

Simple toast without external library:

```html
<!-- Add once to page, hidden by default -->
<div id="toast-container" style="
  position: fixed;
  bottom: 5rem;
  right: 1.5rem;
  z-index: var(--z-toast);
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  pointer-events: none;
" aria-live="polite"></div>
```

```js
function showToast(message, type) {
  type = type || "success"; // "success" | "error" | "info"
  var colors = { success: "var(--color-success)", error: "var(--color-error)", info: "var(--color-info)" };
  var container = document.getElementById("toast-container");
  var toast = document.createElement("div");
  toast.style.cssText = "background: var(--color-bg-elevated); border: 1px solid var(--color-border); border-left: 3px solid " + colors[type] + "; color: var(--color-text); padding: 0.85rem 1.25rem; border-radius: var(--radius-md); font-size: var(--text-sm); box-shadow: var(--shadow-lg); pointer-events: all; opacity:0; transform: translateX(20px); transition: all 0.3s var(--ease-out); max-width: 320px;";
  toast.textContent = message;
  container.appendChild(toast);
  requestAnimationFrame(function() { toast.style.opacity = "1"; toast.style.transform = "translateX(0)"; });
  setTimeout(function() {
    toast.style.opacity = "0";
    toast.style.transform = "translateX(20px)";
    setTimeout(function() { container.removeChild(toast); }, 400);
  }, 4000);
}

// Usage:
// showToast("Form submitted successfully!", "success");
// showToast("Something went wrong.", "error");
```

---

## 📱 PORTFOLIO FILTER

The filter is already wired up in `global-scripts.js`. CSS needed:

```html
<div class="filter-bar">
  <button class="filter-btn active" data-filter="all">All</button>
  <button class="filter-btn" data-filter="web">Web</button>
  <button class="filter-btn" data-filter="branding">Branding</button>
  <button class="filter-btn" data-filter="seo">SEO</button>
</div>

<div class="portfolio-grid" id="portfolio-grid">
  <div class="portfolio-item" data-category="web">...</div>
  <div class="portfolio-item" data-category="branding">...</div>
</div>
```

```css
.filter-bar {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-2);
  margin-bottom: var(--space-10);
}
.filter-btn {
  padding: 0.4rem 1.1rem;
  font-size: var(--text-sm);
  font-weight: 500;
  font-family: var(--font-body);
  border: 1px solid var(--color-border);
  background: var(--color-card);
  color: var(--color-text-muted);
  border-radius: var(--radius-full);
  cursor: pointer;
  transition: all var(--duration-fast) var(--ease-out);
}
.filter-btn:hover { border-color: var(--color-border-hover); color: var(--color-text); }
.filter-btn.active { background: var(--color-primary); border-color: var(--color-primary); color: #fff; }

.portfolio-item {
  transition: opacity var(--duration-slow) var(--ease-out), transform var(--duration-slow) var(--ease-out);
}
```

---

## 🌍 GHL-SPECIFIC TIPS

### Using Custom Values (GHL)
Set site-wide values like phone number, address, GTM ID as GHL Custom Values. Reference them in templates as `{{custom_values.phone}}`. This way you only update in one place.

### GHL Tracking Code Injection Order
1. **Head** → `gtm-head.html` (GTM snippet, fonts, favicons, CSS links)
2. **Body** → `gtm-body.html` (GTM noscript, global-scripts.js)
3. **Header** → `header.html` (navbar component)
4. **Footer** → `footer.html` (footer + WhatsApp button)

### GHL Custom CSS Box
Paste CSS in this order per page:
```
1. variables.css contents  (or link the file if hosting)
2. base.css contents       (or link the file if hosting)
3. [page].css contents
```

### CSS Hosting Options in GHL
- **Option A**: Upload CSS files to GHL → Media Library → paste public URL in `<link rel="stylesheet">`
- **Option B**: Copy-paste full CSS content into GHL's Custom CSS box for each page
- **Option C**: Host on GitHub Pages or a CDN, link from GHL

---

## 📋 PRE-LAUNCH CHECKLIST

Run this before any site goes live:

### Technical
- [ ] All pages open without errors locally (Live Server in VS Code)
- [ ] All pages render correctly at 375px (mobile), 768px (tablet), 1440px (desktop)
- [ ] Dark mode and light mode tested on every page
- [ ] All images have `alt`, `width`, `height` attributes
- [ ] All forms submit and validate correctly
- [ ] 404 page shows correctly
- [ ] No broken internal links
- [ ] WhatsApp button works with correct phone number

### SEO
- [ ] Every page has unique `<title>` and `<meta name="description">`
- [ ] Every page has `<link rel="canonical">`
- [ ] Sitemap accessible at `/sitemap.xml`
- [ ] Robots accessible at `/robots.txt`
- [ ] JSON-LD on every page
- [ ] Breadcrumbs on all inner pages
- [ ] Google Search Console verified

### Social / OG
- [ ] `og:image` at full `https://` URL per page (1200×630)
- [ ] Facebook Sharing Debugger: passes
- [ ] X Card Validator: shows `summary_large_image`
- [ ] WhatsApp tested (send link in chat)

### Performance
- [ ] Lighthouse score > 90 on Performance, Accessibility, SEO
- [ ] Hero image has `loading="eager"` + `fetchpriority="high"`
- [ ] All below-fold images have `loading="lazy"`
- [ ] No render-blocking scripts (all `defer` or `async`)
- [ ] GTM loads from `gtm-head.html` (async)

### Favicon & PWA
- [ ] `favicon.ico` present
- [ ] `apple-touch-icon.png` (180×180) present
- [ ] `android-chrome-192x192.png` + `512x512` present
- [ ] `site.webmanifest` present and valid
- [ ] `theme-color` meta tag set

### Accessibility
- [ ] Skip navigation link at top (in header.html)
- [ ] All interactive elements keyboard-accessible
- [ ] Color contrast 4.5:1 minimum for body text
- [ ] All icon buttons have `aria-label`
- [ ] All form inputs have paired `<label>`
- [ ] Reduced motion respected (in base.css)
