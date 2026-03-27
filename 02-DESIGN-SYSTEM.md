# 02-DESIGN-SYSTEM.md — Design Tokens, Typography & Visual Language
> Load after 00-CORE.md + 01-SETUP.md. Defines every visual decision before a single line of code is written.

---

## 🧠 DESIGN PHILOSOPHY

Premium design is **controlled restraint with one dramatic moment per page**. The quiet sections make the loud one land. A designer does not decorate every section — they choose where to be bold and let everything else breathe.

The core principle: **open space is the design**. Content floats in generous whitespace. Cards are used sparingly. The page itself is not a card.

**Before building any section, ask:**
- Does this need a container/card, or can it live in open space?
- Where is the ONE dramatic moment on this page?
- Is the scroll behavior purposeful or just decorative?
- Would removing 50% of the decoration make it feel more premium?

**If the answer to the last question is YES — remove it.**

---

## 🎨 TYPOGRAPHY SYSTEM

Always use **2 fonts**: one dramatic display font + one refined body font. Both loaded via Google Fonts in `gtm-head.html`.

### Font Pairings by Personality

#### 🖤 Dark Luxury / Premium Agency
| Display | Body | Vibe |
|---------|------|------|
| `Cormorant Garamond` | `DM Sans` | Fashion, high-end, editorial |
| `Playfair Display` | `Outfit` | Classic luxury, law, finance |
| `Fraunces` | `Manrope` | Artisanal, premium, handcrafted |
| `Bodoni Moda` | `Plus Jakarta Sans` | Ultra luxury, magazine |
| `Libre Baskerville` | `Nunito Sans` | Elegant, refined, trustworthy |

#### ⚡ Bold / Startup / Tech
| Display | Body | Vibe |
|---------|------|------|
| `Bebas Neue` | `Inter` *(allowed here only)* | Bold tech, SaaS |
| `Syne` | `DM Sans` | Modern agency, creative studio |
| `Space Grotesk` | `Manrope` | Tech product, developer tool |
| `Exo 2` | `Source Sans 3` | Futuristic, tech, innovation |

#### 🌿 Warm / Natural / Lifestyle
| Display | Body | Vibe |
|---------|------|------|
| `Lora` | `Nunito` | Warm blog, wellness, organic |
| `Merriweather` | `Source Sans 3` | Trustworthy, journalistic |
| `Abril Fatface` | `Lato` | Lifestyle brand, food, travel |

#### 🎨 Creative / Portfolio / Agency
| Display | Body | Vibe |
|---------|------|------|
| `Unbounded` | `Plus Jakarta Sans` | Bold creative, Gen-Z |
| `Raleway` | `Open Sans` | Clean portfolio, minimal |
| `Josefin Sans` | `Karla` | Geometric, Scandinavian |

#### 🏢 Corporate / B2B / SaaS
| Display | Body | Vibe |
|---------|------|------|
| `Barlow` | `IBM Plex Sans` | Enterprise, B2B |
| `Poppins` | `Mulish` | Modern corporate |

### Typography Rules
- Headlines: `clamp(3rem, 8vw, 7rem)` — fluid, never fixed pixel
- Line height on headings: `1.05` to `1.15`
- Letter spacing on display: `-0.02em` to `-0.04em`
- Body text: `1rem` minimum, `line-height: 1.7`
- Max line length: `max-width: 72ch`
- Avoid ALL CAPS for long text — use for labels/overlines only

### How to Load Fonts (in `gtm-head.html`)
```html
<!-- Example: Cormorant Garamond + DM Sans -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preload"
  href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600;700&family=DM+Sans:wght@400;500;600&display=swap"
  as="style"
  onload="this.onload=null;this.rel='stylesheet'">
<noscript>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600;700&family=DM+Sans:wght@400;500;600&display=swap">
</noscript>
```

Then in `variables.css`:
```css
:root {
  --font-display: "Cormorant Garamond", Georgia, serif;
  --font-body:    "DM Sans", system-ui, sans-serif;
}
```

---

## 🎨 COLOR SYSTEM — variables.css

### Full Token Set (`:root`)

```css
:root {
  /* Backgrounds */
  --color-bg:            #0a0a0a;
  --color-bg-secondary:  #111111;
  --color-bg-tertiary:   #1a1a1a;
  --color-bg-elevated:   #222222;

  /* Text */
  --color-text:          #f0f0f0;
  --color-text-muted:    #888888;
  --color-text-subtle:   #555555;

  /* Brand — replace with project colors */
  --color-primary:       #your-brand;
  --color-primary-hover: #your-brand-darker;
  --color-accent:        #your-accent;

  /* Borders */
  --color-border:        rgba(255,255,255,0.08);
  --color-border-hover:  rgba(255,255,255,0.16);

  /* Cards */
  --color-card:          rgba(255,255,255,0.04);

  /* Semantic */
  --color-success: #22c55e;
  --color-warning: #f59e0b;
  --color-error:   #ef4444;
  --color-info:    #3b82f6;

  /* Typography */
  --font-display: "Cormorant Garamond", Georgia, serif;
  --font-body:    "DM Sans", system-ui, sans-serif;

  /* Fluid font sizes */
  --text-hero: clamp(3rem, 8vw, 7rem);
  --text-h1:   clamp(2.5rem, 5vw, 4.5rem);
  --text-h2:   clamp(1.75rem, 3.5vw, 2.75rem);
  --text-xs:   0.75rem;
  --text-sm:   0.875rem;
  --text-base: 1rem;
  --text-lg:   1.125rem;
  --text-xl:   1.25rem;
  --text-2xl:  1.5rem;
  --text-3xl:  1.875rem;

  /* Spacing */
  --space-1:  0.25rem;
  --space-2:  0.5rem;
  --space-3:  0.75rem;
  --space-4:  1rem;
  --space-6:  1.5rem;
  --space-8:  2rem;
  --space-10: 2.5rem;
  --space-12: 3rem;
  --space-16: 4rem;
  --space-20: 5rem;
  --space-24: 6rem;

  --section-py:    clamp(4rem, 8vw, 8rem);
  --container-max: 1280px;
  --container-px:  clamp(1.25rem, 4vw, 2rem);

  /* Radius */
  --radius-xs:   4px;
  --radius-sm:   8px;
  --radius-md:   12px;
  --radius-lg:   16px;
  --radius-xl:   24px;
  --radius-2xl:  32px;
  --radius-full: 9999px;

  /* Shadows */
  --shadow-sm:   0 1px 3px rgba(0,0,0,0.4), 0 1px 2px rgba(0,0,0,0.3);
  --shadow-md:   0 4px 16px rgba(0,0,0,0.5), 0 2px 6px rgba(0,0,0,0.3);
  --shadow-lg:   0 8px 32px rgba(0,0,0,0.6), 0 4px 12px rgba(0,0,0,0.4);
  --shadow-xl:   0 20px 60px rgba(0,0,0,0.7), 0 8px 24px rgba(0,0,0,0.5);
  --shadow-glow: 0 0 40px rgba(120,80,255,0.3);  /* update to brand color */
  --shadow-card: 0 2px 8px rgba(0,0,0,0.4), inset 0 1px 0 rgba(255,255,255,0.06);

  /* Transitions */
  --ease-out:    cubic-bezier(0.22, 1, 0.36, 1);
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
  --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);
  --duration-fast:   150ms;
  --duration-base:   250ms;
  --duration-slow:   400ms;
  --duration-slower: 700ms;

  /* Z-index scale */
  --z-base:     0;
  --z-raised:   10;
  --z-dropdown: 100;
  --z-sticky:   200;
  --z-overlay:  300;
  --z-modal:    400;
  --z-toast:    500;
  --z-tooltip:  600;
}

[data-theme="light"] {
  --color-bg:           #ffffff;
  --color-bg-secondary: #f8f8f8;
  --color-bg-tertiary:  #f0f0f0;
  --color-bg-elevated:  #e8e8e8;
  --color-text:         #0a0a0a;
  --color-text-muted:   #666666;
  --color-text-subtle:  #999999;
  --color-border:       rgba(0,0,0,0.08);
  --color-border-hover: rgba(0,0,0,0.16);
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.08);
  --shadow-md: 0 4px 16px rgba(0,0,0,0.10);
  --shadow-lg: 0 8px 32px rgba(0,0,0,0.12);
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 🏗️ BASE.CSS — Structure & Utilities

`base.css` always loads after `variables.css`. It provides:

- CSS reset
- Body/element base styles using CSS variables only
- Utility classes that mirror a lean subset of what Tailwind provided
- All animation primitives (`.reveal`, `.delay-N`, `.text-shimmer`, `.grain`, `.page-grid`)
- Button styles (`.btn`, `.btn-primary`, `.btn-secondary`, `.btn-outline`, `.btn-ghost`)
- Form styles (`.form-input`, `.form-label`, `.form-group`, `.form-textarea`)
- Card style (`.card`)
- Badge style (`.badge`)
- Container (`.container`)
- Section padding (`.section`)
- Breadcrumb (`.breadcrumb`)

### Key Utility Classes Reference

```css
/* Layout */
.container        /* max-width + centered + responsive padding */
.section          /* standard vertical padding via --section-py */

/* Reveal animations (triggered by global-scripts.js) */
.reveal           /* opacity:0 + translateY(40px), transitions to visible */
.delay-1          /* 100ms delay */
.delay-2          /* 200ms delay */
.delay-3          /* 300ms delay */
.delay-4          /* 400ms delay */
.delay-5          /* 500ms delay */

/* Shimmer text */
.text-shimmer     /* animated gradient sweep on ONE hero keyword */

/* Ambient decorations */
.grain            /* noise texture overlay on body */
.page-grid        /* vertical grid lines overlay on body (desktop only) */

/* Section label overline */
.section-label    /* uppercase, tracked, brand color, small */

/* Buttons */
.btn              /* base button */
.btn-primary      /* filled brand color */
.btn-secondary    /* elevated bg + border */
.btn-outline      /* transparent + brand border */
.btn-ghost        /* transparent + muted text */
.btn-lg           /* larger padding */
.btn-sm           /* smaller padding */

/* Components */
.card             /* card container with border + hover lift */
.badge            /* pill label */

/* Forms */
.form-group       /* flex column gap */
.form-label       /* label styling */
.form-input       /* input styling */
.form-textarea    /* textarea styling */
.form-hint        /* helper text */
.form-error       /* error message */

/* Counters (triggered by global-scripts.js) */
/* Usage: <span class="counter" data-target="250" data-suffix="+"></span> */

/* FAQ Accordion (triggered by global-scripts.js) */
/* Usage: .faq-item > .faq-trigger + .faq-body */

/* Slider (triggered by global-scripts.js) */
/* Usage: .testimonial-slider > .testimonial-track > .testimonial-slide */

/* Portfolio Filter (triggered by global-scripts.js) */
/* Usage: .filter-btn[data-filter="x"] + .portfolio-item[data-category="x"] */
```

---

## 🎭 SCROLL PATTERNS — CSS + IntersectionObserver

No Framer Motion. These are the equivalent patterns in CSS + Vanilla JS.

### Pattern 1: Appear (fade + rise)
```html
<section class="reveal">
  <h2>Section heading</h2>
</section>
```
The `.reveal` class + IntersectionObserver in `global-scripts.js` handles this. Adds `.visible` class when element enters viewport, which triggers the CSS transition.

### Pattern 2: Stagger Cascade
```html
<div class="services-grid">
  <div class="card reveal delay-1">...</div>
  <div class="card reveal delay-2">...</div>
  <div class="card reveal delay-3">...</div>
</div>
```

### Pattern 3: Counter Sweep
```html
<span class="counter" data-target="250" data-suffix="+">0</span>
```
Triggered by IntersectionObserver in `global-scripts.js`. Animates from 0 to `data-target` when in view.

### Pattern 4: Marquee Drift (CSS only)
```html
<div class="marquee-wrapper">
  <div class="marquee-track">
    <!-- Items duplicated for seamless loop -->
    <span>Partner One</span>
    <span>Partner Two</span>
    <!-- ... duplicate the full set ... -->
  </div>
</div>
```
```css
.marquee-track {
  display: flex;
  gap: 4rem;
  width: max-content;
  animation: marquee-scroll 30s linear infinite;
}
@keyframes marquee-scroll {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}
```

### Pattern 5: Ghost List (FAQ)
```html
<div class="faq-list">
  <div class="faq-item">
    <button class="faq-trigger" aria-expanded="false">Question?</button>
    <div class="faq-body"><p>Answer text here.</p></div>
  </div>
</div>
```
The `.ghost-list:hover .ghost-list-item` CSS + JS accordion in `global-scripts.js`.

---

## 🏛️ SPATIAL PATTERNS — HTML/CSS Equivalents

### Text Island (Mission statement — no card)
```html
<section class="section text-island">
  <div class="container" style="max-width: 800px">
    <span class="section-label reveal">Who We Are</span>
    <p class="text-island-text reveal delay-1">
      Your most powerful brand statement. Written like a human.
      <em>Two sentences maximum.</em>
    </p>
  </div>
</section>
```
```css
.text-island-text {
  font-family: var(--font-display);
  font-size: clamp(1.5rem, 3vw, 2.5rem);
  font-weight: 400;
  line-height: 1.4;
  color: var(--color-text);
}
.text-island-text em { font-style: italic; color: var(--color-primary); }
```

### Full-Bleed Contrast Section
```html
<section class="contrast-section section">
  <div class="contrast-bg-pattern" aria-hidden="true"></div>
  <div class="container">...</div>
</section>
```
```css
.contrast-section {
  position: relative;
  background: #1a2472; /* ONE dramatic background per page */
  overflow: hidden;
}
.contrast-bg-pattern {
  position: absolute;
  inset: 0;
  opacity: 0.06;
  background-image: repeating-linear-gradient(
    -45deg, transparent, transparent 60px,
    rgba(255,255,255,0.08) 60px, rgba(255,255,255,0.08) 61px
  );
}
```

### Floating Label Images (Portfolio grid)
```html
<div class="portfolio-item-wrap">
  <img src="..." alt="..." width="600" height="450" loading="lazy" class="portfolio-img">
  <span class="floating-label">Web Design</span>
  <div class="portfolio-hover-overlay">
    <p class="portfolio-title">Project Name</p>
    <p class="portfolio-category">Category</p>
  </div>
</div>
```

### Concentric SVG Rings (Hero + CTA — desktop only)
```html
<!-- Add inside hero, aria-hidden -->
<div class="hero-rings" aria-hidden="true">
  <svg viewBox="0 0 900 900" xmlns="http://www.w3.org/2000/svg">
    <circle cx="450" cy="450" r="200" fill="none" stroke="currentColor" stroke-width="1"/>
    <circle cx="450" cy="450" r="300" fill="none" stroke="currentColor" stroke-width="1"/>
    <circle cx="450" cy="450" r="400" fill="none" stroke="currentColor" stroke-width="1"/>
    <circle cx="450" cy="450" r="430" fill="none" stroke="currentColor" stroke-width="1" stroke-dasharray="4 8"/>
  </svg>
</div>
```
```css
.hero-rings {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  pointer-events: none;
  color: var(--color-text);
  opacity: 0.07;
}
@media (max-width: 767px) { .hero-rings { display: none; } }
```

---

## 🎯 RESTRAINT RULES

### The "One Dramatic Moment" Rule
Every page gets **exactly one** dramatic moment:
- A Full-Bleed Contrast section, OR
- A Scroll Parade (marquee strip), OR
- A full-viewport image takeover

Everything else breathes quietly. The dramatic moment lands harder because of the quiet around it.

### The "Open Space Is The Design" Rule
Before adding a card, border, or background to a section — ask: can this live in open space?
- Mission/about copy → Text Island, not a card
- Stats → numbers floating on the page, not boxed
- Process steps → numbered list with generous spacing, not a card grid

### The "Max 3 Scroll Patterns Per Page" Rule
Choose from:
- Hero: Word March + Scale Peek → `.reveal` on H1 words
- Services: Stagger Cascade → `.reveal .delay-N` on cards
- Stats: Counter Sweep → `.counter[data-target]`
- Gallery: Marquee Drift → CSS animation
- FAQ: Ghost List → JS accordion
- CTA: Appear → `.reveal`

### The "Decoration Hierarchy" Rule
Only these levels exist. Never mix more than one per section:
1. **Nothing** — open space. Most powerful.
2. **Grid lines** — `.page-grid` class on body. Global, everywhere.
3. **Grain** — `.grain` class on body. Global, everywhere.
4. **One shimmer word** — `.text-shimmer`. Hero only. One word.
5. **Concentric SVG rings** — Hero + CTA only. Desktop only.
6. **Ambient glow blobs** — `opacity: 0.08`. Dark sections only. Max 2 per page.

### The "Mobile Gets None Of The Decoration" Rule
```css
@media (max-width: 767px) {
  .hero-rings    { display: none; }
  .page-grid::after { display: none; }
  .ambient-glow  { display: none; }
}
```

---

## 📋 PATTERN SELECTION TABLE — CHOOSE BEFORE BUILDING

| Section | Scroll Pattern | Spatial Pattern | Notes |
|---------|---------------|-----------------|-------|
| Hero | `.reveal` on words | — | Always |
| Logo strip | CSS Marquee | — | Always |
| Mission/About | `.reveal` | Text Island | No card, open space |
| Stats | `.reveal` + Counter | Floating numbers | No boxes |
| Services | Stagger `.delay-N` | `.card` grid | Standard |
| Gallery/Portfolio | CSS Marquee or JS filter | Floating Label Images | |
| Dark feature | Stagger `.delay-N` | Full-Bleed Contrast | Once per page |
| Testimonials | `.reveal` | JS Slider | Auto-play |
| CTA | `.reveal` | Concentric Rings | |
| FAQ | JS Accordion | Ghost List | |
| Blog grid | Stagger `.delay-N` | Borderless grid | No card borders |

**Total scroll patterns per page: max 3.**
**Total decoration: grid lines + grain (global) + 1 contextual.**
