---
name: landing-page
description: |
  Build a stunning, conversion-optimized landing page with a distinctive visual identity.
  Integrates the ui-ux-design-pro design database (67+ styles, 161 palettes, 57 font pairings)
  for bold aesthetics. Covers copywriting, section layout, responsive HTML/CSS or Next.js,
  live preview, and Vercel deployment. No heavy engineering workflow — fast and focused.
  Triggers:
  - "landing page"
  - "build a landing page"
  - "create landing page"
  - "product page"
  - "launch page"
  - "waitlist page"
  - "做一个落地页"
  - "落地页"
  - "产品页面"
---

# SKILL.md — Landing Page Builder

> Build a conversion-optimized landing page with a distinctive visual identity —
> backed by 67+ UI styles, 161 color palettes, 57 font pairings, and 35 landing page patterns.
> Ships fast: design research → copy → code → live preview → deploy.

---

## Overview

A landing page has one job: **convert visitors into leads, signups, or customers**.
Everything — design, copy, structure, CTA — serves that one goal.

**Pipeline (5 Phases):**

```
Phase 0 — Understand    →  What is this page for? Who is it for? What's the #1 action?
Phase 1 — Design        →  Style, color palette, fonts from the design database
Phase 2 — Copy          →  Headline, subheadline, sections, CTA text
Phase 3 — Build         →  HTML/CSS (default) or Next.js static page
Phase 4 — Deliver       →  Live preview → Vercel deploy
```

### Tech Stack Options

| Stack | When to Use |
|-------|-------------|
| **HTML + CSS + Vanilla JS** | Default. Fastest to build and deploy. No dependencies. |
| **Next.js (static export)** | When user already has a Next.js project, or needs MDX/i18n |
| **React + Vite** | When user wants component-based structure but not full Next.js |

Default to **HTML + CSS + Vanilla JS** unless the user specifies otherwise. A landing page doesn't need a framework.

### File Structure (HTML default)

```
landing-page/
├── index.html          # Main page
├── styles.css          # All styles + CSS variables (design tokens)
├── main.js             # Minimal JS: mobile nav toggle, scroll effects, form
├── assets/
│   ├── fonts/          # Self-hosted fonts (if not using Google Fonts CDN)
│   └── images/         # Logo, hero image, etc.
└── README.md
```

---

## Phase 0 — Understand the Page

### 0.1 Key Questions

Before any design or copy work, answer these:

| Question | Why It Matters |
|----------|---------------|
| **What is this page promoting?** | Product, service, event, waitlist, app, course... |
| **Who is the target visitor?** | Demographics, pain points, level of awareness |
| **What is the ONE action you want visitors to take?** | Sign up, buy, download, contact, join waitlist |
| **What's the current stage?** | Pre-launch (waitlist) / launch / established product |
| **Any existing brand?** | Colors, fonts, logo — or starting from scratch? |
| **Tone?** | Professional, playful, bold, minimalist, luxury... |
| **Competitors' pages?** | Any to emulate or differentiate from? |

### 0.2 Conversion Goal Mapping

| Page Type | Primary CTA | Key Sections |
|-----------|-------------|--------------|
| **Waitlist / Pre-launch** | "Join the waitlist" | Hero + Problem + Teaser + Social proof + CTA |
| **SaaS Product** | "Start free trial" / "Get started" | Hero + Problem + Solution + Features + Pricing + FAQ + CTA |
| **Mobile App** | "Download on App Store" | Hero + Screenshots + Features + Reviews + Download |
| **Course / Event** | "Enroll now" / "Register" | Hero + What you'll learn + Instructor + Curriculum + Testimonials + CTA |
| **Agency / Service** | "Book a call" / "Get a quote" | Hero + Services + Process + Case studies + Trust + CTA |
| **Physical Product** | "Buy now" / "Shop" | Hero + Product shots + Benefits + Reviews + FAQ + CTA |

---

## Phase 1 — Design Research

### 1.1 The Design Database

```bash
# Style search
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain style --max-results 5

# Color palette
python3 src/ui-ux-pro-max/scripts/search.py "<mood/industry>" --domain color --max-results 5

# Typography
python3 src/ui-ux-pro-max/scripts/search.py "<style/mood>" --domain typography --max-results 3

# Landing page patterns (35 templates)
python3 src/ui-ux-pro-max/scripts/search.py "<product type>" --domain landing --max-results 3

# Product-type rules (anti-patterns included)
python3 src/ui-ux-pro-max/scripts/search.py "<product type>" --domain product

# Full design system shortcut
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --design-system -p "<Page Name>"

# Stack-specific snippets
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack html
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack nextjs
```

### 1.2 Anti-AI-Slop Rule

Before touching code, commit to a **specific aesthetic direction**. Landing pages especially suffer from generic AI aesthetics:

| ❌ Generic (avoid) | ✅ Distinctive (aim for) |
|---|---|
| Purple/blue gradient hero on white | A palette with real personality and contrast |
| Inter font everywhere | A distinctive heading font that sets the tone |
| 3 icon cards with "Feature 1, 2, 3" | Layouts that serve the actual content |
| Stock photo hero | Purposeful imagery or illustration direction |
| CTA button that looks like every other site | A button that earns attention |

### 1.3 Design Brief for Landing Pages

```markdown
## Landing Page Design Brief: <Product/Service Name>

### Page Goal
- **What it promotes:** <product/service/event>
- **Primary CTA:** <exact CTA text>
- **Target visitor:** <who>
- **Page type:** <waitlist / SaaS / app / course / agency / product>

### Style Direction
- **Primary style:** <from search — e.g., "Dark Glassmorphism", "Warm Editorial">
- **One-word aesthetic:** <e.g., "urgent", "premium", "playful", "authoritative">
- **Why this style:** <one sentence>

### Color Palette
- **Background:** <hex> — <name>
- **Text primary:** <hex>
- **Accent / CTA color:** <hex> — must be high-contrast, draws the eye
- **Surface / card:** <hex>
- **Gradient (if any):** <from> → <to>

### Typography
- **Heading font:** <name> + Google Fonts URL
- **Body font:** <name> + Google Fonts URL
- **Heading size (hero):** <e.g., 64px desktop / 40px mobile>

### Landing Page Pattern
- **Pattern:** <from landing search results>
- **Section order:** <list>

### Key Effects
- <animations, scroll effects, hover states from style search>

### Anti-Patterns to Avoid
- <from product search>
```

### 1.4 Quality Criteria

- [ ] Page type identified, conversion goal clear
- [ ] At least 3 style options searched and considered
- [ ] Color palette: bg, text, accent, surface all defined
- [ ] Accent/CTA color is high-contrast and visually dominant
- [ ] Font pairing selected with Google Fonts links
- [ ] Landing page pattern chosen (section order)
- [ ] User approved the Design Brief

---

## Phase 2 — Copy

### 2.1 Copy Hierarchy

Every section of a landing page serves a specific persuasion function:

```
Hero           → Grab attention + communicate core value in 3 seconds
Problem        → Make the visitor feel understood ("you have this pain")
Solution       → Introduce the product as the answer
Features/Benefits → Prove it — show HOW it solves the problem
Social Proof   → Build trust — testimonials, logos, numbers
Pricing        → Remove the decision barrier (if applicable)
FAQ            → Address objections
CTA            → Ask for the action — clear, urgent, low-friction
```

### 2.2 Headline Formula

The hero headline must answer: **"What is it, who is it for, and why should I care?"** in under 10 words.

Strong formulas:
- `[Outcome] for [Audience] — without [Pain]`
- `The [adjective] way to [do the thing]`
- `[Big promise]. [Short proof].`
- `Stop [bad thing]. Start [good thing].`

Avoid:
- Vague value props ("The platform that transforms your workflow")
- Feature-first headlines ("Built with AI and machine learning")
- Question headlines ("Tired of X?") — overused

### 2.3 CTA Copy Rules

| ❌ Weak | ✅ Strong |
|---------|---------|
| Submit | Get early access |
| Click here | Start my free trial |
| Learn more | See how it works |
| Sign up | Join 2,400 founders |

CTA should state what the visitor **gets**, not what they **do**.

### 2.4 Copy by Page Type

**Waitlist page:**
- Headline: The promise of what's coming
- Subheadline: Who it's for + when
- Social proof: "X people already on the list"
- CTA: "Get early access" / "Join the waitlist"

**SaaS product:**
- Headline: The outcome the software delivers
- Features: 3-4, benefit-first (not feature-first)
- Social proof: customer logos OR testimonials (not both on small pages)
- Pricing: 3 tiers max, highlight the recommended one

**Mobile app:**
- Hero: app screenshots prominently displayed
- Feature list: 3-5 key things, 1-line each
- Reviews: 3 star ratings with text
- Download badges: App Store + Google Play

### 2.5 Quality Criteria

- [ ] Hero headline answers "what, who, why" in under 10 words
- [ ] Subheadline adds one specific detail the headline can't fit
- [ ] All CTAs state what the visitor gets (not what they do)
- [ ] Features are written as benefits (outcome-first, not feature-first)
- [ ] Social proof is specific (numbers, names, logos — not vague)
- [ ] No filler phrases ("world-class", "cutting-edge", "revolutionize")

---

## Phase 3 — Build

### 3.1 HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><Page Title> — <Tagline></title>
  <meta name="description" content="<150-char description>">

  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=<HeadingFont>:wght@400;600;700&family=<BodyFont>:wght@400;500&display=swap" rel="stylesheet">

  <!-- OG Tags for social sharing -->
  <meta property="og:title" content="<title>">
  <meta property="og:description" content="<description>">
  <meta property="og:image" content="<og-image-url>">

  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <!-- Nav -->
  <nav class="nav">...</nav>

  <!-- Hero -->
  <section class="hero">...</section>

  <!-- [Sections per Design Brief pattern] -->

  <!-- Final CTA -->
  <section class="cta-section">...</section>

  <!-- Footer -->
  <footer class="footer">...</footer>

  <script src="main.js"></script>
</body>
</html>
```

### 3.2 CSS Architecture

```css
/* ── 1. Google Fonts import (or @font-face) ── */
/* ── 2. CSS Variables (design tokens) ── */
:root {
  /* Colors from Design Brief */
  --color-bg: <hex>;
  --color-bg-secondary: <hex>;
  --color-text: <hex>;
  --color-text-muted: <hex>;
  --color-accent: <hex>;        /* CTA color — high contrast */
  --color-accent-hover: <hex>;
  --color-surface: <hex>;       /* card/panel bg */
  --color-border: <hex>;

  /* Typography */
  --font-heading: '<HeadingFont>', serif;
  --font-body: '<BodyFont>', sans-serif;

  /* Type scale */
  --text-hero: clamp(2.5rem, 6vw, 4.5rem);  /* responsive hero headline */
  --text-h2: clamp(1.75rem, 4vw, 2.75rem);
  --text-h3: clamp(1.25rem, 2.5vw, 1.75rem);
  --text-body: 1.125rem;
  --text-sm: 0.9rem;

  /* Spacing */
  --section-padding: clamp(4rem, 10vw, 8rem);
  --container-width: 1200px;
  --gap: 1.5rem;
  --radius: 12px;
  --radius-lg: 20px;

  /* Shadows */
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.08);
  --shadow-md: 0 4px 16px rgba(0,0,0,0.12);
  --shadow-lg: 0 8px 32px rgba(0,0,0,0.16);
}

/* ── 3. Reset + Base ── */
/* ── 4. Layout utilities (.container, .section, .grid) ── */
/* ── 5. Components (nav, button, card, badge, form) ── */
/* ── 6. Sections (hero, features, pricing, testimonials, cta, footer) ── */
/* ── 7. Animations + effects ── */
/* ── 8. Responsive (mobile-first: min-width breakpoints) ── */
```

### 3.3 Key Section Implementations

#### Hero Section

```html
<section class="hero">
  <div class="container">
    <div class="hero__content">
      <!-- Badge (optional) -->
      <span class="badge">✦ Now in beta</span>

      <!-- Headline — most important element on the page -->
      <h1 class="hero__headline">
        <span class="hero__headline-main"><Main headline></span>
        <span class="hero__headline-accent"><Accented part></span>
      </h1>

      <!-- Subheadline -->
      <p class="hero__subheadline"><Subheadline — 1-2 sentences></p>

      <!-- CTA group -->
      <div class="hero__cta">
        <a href="#signup" class="btn btn--primary btn--lg"><Primary CTA></a>
        <a href="#how-it-works" class="btn btn--ghost"><Secondary CTA></a>
      </div>

      <!-- Social proof micro-signal -->
      <p class="hero__proof"><★★★★★ Trusted by 2,400+ users></p>
    </div>

    <!-- Visual (product screenshot, illustration, etc.) -->
    <div class="hero__visual">
      <div class="hero__visual-wrapper">
        <!-- product screenshot / mockup / illustration -->
      </div>
    </div>
  </div>
</section>
```

#### CTA Button System

```css
.btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  font-family: var(--font-body);
  font-weight: 600;
  border-radius: var(--radius);
  cursor: pointer;
  transition: all 0.2s ease;
  text-decoration: none;
  border: 2px solid transparent;
}
.btn--primary {
  background: var(--color-accent);
  color: #fff;
  padding: 0.875rem 2rem;
}
.btn--primary:hover {
  background: var(--color-accent-hover);
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}
.btn--ghost {
  background: transparent;
  color: var(--color-text);
  border-color: var(--color-border);
  padding: 0.875rem 1.5rem;
}
.btn--lg { font-size: 1.0625rem; padding: 1rem 2.25rem; }
.btn--sm { font-size: 0.875rem; padding: 0.5rem 1.25rem; }
```

#### Signup Form (waitlist / lead capture)

```html
<form class="signup-form" id="signup">
  <input
    type="email"
    name="email"
    placeholder="Enter your email"
    required
    autocomplete="email"
    class="signup-form__input"
  >
  <button type="submit" class="btn btn--primary">
    <Primary CTA text>
  </button>
</form>
```

```js
// main.js — form submission (connect to Formspree / Mailchimp / etc.)
document.querySelector('.signup-form')?.addEventListener('submit', async (e) => {
  e.preventDefault()
  const email = e.target.email.value
  // TODO: connect to form backend (Formspree, ConvertKit, etc.)
  e.target.innerHTML = '<p class="signup-form__success">✓ You\'re on the list!</p>'
})
```

### 3.4 Scroll Animations

```js
// Intersection Observer for scroll-in animations
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible')
      observer.unobserve(entry.target)
    }
  })
}, { threshold: 0.1 })

document.querySelectorAll('.animate-on-scroll').forEach(el => observer.observe(el))
```

```css
.animate-on-scroll {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.animate-on-scroll.visible {
  opacity: 1;
  transform: translateY(0);
}
/* Staggered delays for grids */
.feature-card:nth-child(1) { transition-delay: 0s; }
.feature-card:nth-child(2) { transition-delay: 0.1s; }
.feature-card:nth-child(3) { transition-delay: 0.2s; }

/* Respect prefers-reduced-motion */
@media (prefers-reduced-motion: reduce) {
  .animate-on-scroll { opacity: 1; transform: none; transition: none; }
}
```

### 3.5 Mobile Nav Toggle

```js
const menuToggle = document.querySelector('.nav__toggle')
const navLinks = document.querySelector('.nav__links')
menuToggle?.addEventListener('click', () => {
  navLinks.classList.toggle('nav__links--open')
  menuToggle.setAttribute('aria-expanded',
    navLinks.classList.contains('nav__links--open'))
})
```

### 3.6 Implementation Rules

| Rule | Detail |
|------|--------|
| **CSS variables only** | No hardcoded colors or spacing anywhere in the CSS |
| **`clamp()` for type** | Hero headline uses `clamp()` — responsive without media queries |
| **Mobile-first** | Write base styles for mobile, use `min-width` for desktop |
| **Semantic HTML** | `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>` — no `<div>` soup |
| **No layout shift** | Set explicit dimensions on images, reserve space for fonts |
| **CTA above the fold** | Primary CTA must be visible without scrolling on desktop AND mobile |
| **One primary CTA per section** | Don't confuse visitors with multiple competing actions |
| **Alt text on all images** | Even decorative ones get `alt=""` |

### 3.7 Quality Criteria

- [ ] CSS uses variables exclusively (no hardcoded hex/px values in components)
- [ ] Hero headline uses `clamp()` for responsive sizing
- [ ] CTA is visible above the fold on both mobile (375px) and desktop (1280px)
- [ ] All interactive elements have hover + focus states
- [ ] Mobile nav works (hamburger toggle, links accessible)
- [ ] Scroll animations use IntersectionObserver (not scroll listeners)
- [ ] `prefers-reduced-motion` respected
- [ ] All images have alt text
- [ ] No layout shift on font load (font-display: swap + explicit dimensions)
- [ ] `<html lang="">` set correctly
- [ ] OG tags present (title, description, image)

---

## Phase 4 — Deliver

### 4.1 Preview

Start local preview with:
```bash
# Option 1: Python (no install)
python3 -m http.server 8080

# Option 2: Node (npx)
npx serve .

# Option 3: start_web_service tool (for hosted preview URL)
```

Use `start_web_service` tool to generate a hosted preview URL.

### 4.2 Validation Checklist

**Visual:**
- [ ] Design Brief's aesthetic is recognizable and distinctive
- [ ] Fonts loaded correctly (check browser Network tab)
- [ ] Color palette applied throughout
- [ ] Responsive at 375px (iPhone SE), 768px (tablet), 1280px (desktop)
- [ ] No horizontal scroll on any breakpoint

**Conversion:**
- [ ] Hero CTA visible above the fold on mobile AND desktop
- [ ] CTA text states what the visitor gets
- [ ] Social proof visible early (ideally in or near hero)
- [ ] Form works (submits, shows success state)

**Technical:**
- [ ] Page loads < 3s on simulated slow 3G
- [ ] No console errors
- [ ] OG tags present (paste URL into opengraph.xyz to verify)
- [ ] Favicon set

### 4.3 Deploy to Vercel

```bash
# One-command deploy
npx vercel --prod

# Or use deploy_to_vercel tool
```

Vercel deploys HTML/CSS/JS as static files with zero config needed.

### 4.4 Optional Integrations

| Feature | Tool | Setup |
|---------|------|-------|
| Email capture form | [Formspree](https://formspree.io) | Add `action="https://formspree.io/f/YOUR_ID"` to `<form>` |
| Email capture | [Mailchimp embed](https://mailchimp.com) | Paste embed code |
| Analytics | [Plausible](https://plausible.io) / Google Analytics | Add script tag |
| A/B testing | [Vercel Edge Config](https://vercel.com/docs/storage/edge-config) | Advanced |

### 4.5 Delivery Summary Format

```markdown
## ✅ Landing Page Live!

**Page:** <Product/Service Name>
**Goal:** <Primary CTA — what visitors are asked to do>
**Style:** <Primary style from Design Brief>
**Palette:** <2-3 main colors>
**Fonts:** <Heading> + <Body>

### Sections built
- <list of sections>

### Preview
<live URL>

### To deploy to production
1. `npx vercel --prod` from this folder
2. Connect to custom domain in Vercel dashboard

### To connect email capture
<Formspree or Mailchimp instructions>

### What to do next
- Test on real devices (iOS + Android)
- Set up analytics
- A/B test the headline copy
- Connect email capture to your list
```

---

## Technical Reference

### Landing Page Patterns (from database)

Search for the right pattern:
```bash
python3 src/ui-ux-pro-max/scripts/search.py "<product type>" --domain landing
```

Common patterns:

| Pattern | Best For | Section Order |
|---------|----------|---------------|
| **Hero-First** | Apps, SaaS | Hero → Features → Social proof → Pricing → CTA |
| **Problem-Solution** | B2B, complex products | Hero → Problem → Solution → Features → Proof → CTA |
| **Waitlist** | Pre-launch | Hero → Teaser features → Social proof → Signup form |
| **Video-Led** | Demos, courses | Hero + video → Benefits → Testimonials → CTA |
| **Comparison** | Against competitors | Hero → Comparison table → Testimonials → CTA |

### Design Database Quick Reference

```bash
# All domain searches
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain style
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain color
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain typography
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain landing
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain product
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain ux
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain google-fonts

# Full design system
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --design-system -p "<Name>"

# HTML stack snippets
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack html
```

### Quality Maxims

1. **One page, one goal.** Every section, every word, every button exists to drive toward the single CTA. Remove anything that doesn't.
2. **Above the fold is everything.** If the headline and CTA aren't compelling in the first screen, the visitor is gone.
3. **Specific beats vague.** "Join 2,400 founders" beats "Join thousands". "Save 4 hours/week" beats "Save time".
4. **Design earns trust.** A bad-looking page signals a bad product. Design is not decoration — it's credibility.
5. **Bold beats safe.** A distinctive aesthetic makes the page memorable. Generic = forgettable = low conversion.
6. **No AI slop.** If it looks like every other AI landing page (purple gradient, Inter font, 3 icon cards), the conversion rate will match.
