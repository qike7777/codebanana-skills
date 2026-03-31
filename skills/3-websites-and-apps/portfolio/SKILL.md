---
name: portfolio
description: |
  Build a stunning personal portfolio website with a distinctive visual identity.
  Integrates the ui-ux-design-pro design database (67+ styles, 161 palettes, 57 font pairings)
  for bold, memorable aesthetics. Covers personal branding, project showcase layout,
  responsive HTML/CSS, interactive effects (hover/filter/lightbox), and Vercel deployment.
  Designed for designers, developers, freelancers, and creative professionals.
  Triggers:
  - "portfolio"
  - "personal portfolio"
  - "build a portfolio"
  - "portfolio website"
  - "showcase my work"
  - "personal website"
  - "作品集"
  - "个人网站"
  - "个人作品集"
---

# SKILL.md — Portfolio Builder

> Build a personal portfolio that makes people remember you.
> Backed by 67+ UI styles, 161 color palettes, 57 font pairings, and portfolio-specific patterns.
> Pipeline: understand → brand identity → content → build → deploy.

---

## Overview

A portfolio has one job: **make someone want to work with you**.
Everything — design, copy, project selection, interactions — serves that goal.

**Pipeline (5 Phases):**

```
Phase 0 — Understand    →  Who are you? What do you do? Who's looking at this?
Phase 1 — Design        →  Visual identity: style, palette, fonts — the "feel" of your work
Phase 2 — Content       →  Bio, project descriptions, skills, contact
Phase 3 — Build         →  HTML/CSS + interactive project grid + animations
Phase 4 — Deliver       →  Live preview → Vercel deploy
```

### Portfolio Types

| Type | Audience | Focus |
|------|----------|-------|
| **Designer** | Clients, hiring managers | Visual work, case studies, process |
| **Developer** | Recruiters, clients, collaborators | Projects, tech stack, GitHub |
| **Creative (photographer/illustrator/etc.)** | Clients, galleries | Full-bleed imagery, minimal text |
| **Freelancer** | Potential clients | Services, past work, contact/CTA |
| **Student / Career Changer** | Recruiters, hiring managers | Projects, skills, story arc |

### Tech Stack

Default: **HTML + CSS + Vanilla JS** — same as Landing Page. No framework needed.
- Fastest to build and deploy
- Zero dependencies, maximum control over animations
- Vercel deploys as static files instantly

### File Structure

```
portfolio/
├── index.html          # Main page (single-page or anchor-nav)
├── styles.css          # Design tokens → reset → layout → components → sections → animations
├── main.js             # Filter, lightbox, scroll effects, mobile nav
├── assets/
│   ├── images/         # Project screenshots, avatar, OG image
│   └── fonts/          # Self-hosted fonts (optional)
└── README.md
```

---

## Phase 0 — Understand

### 0.1 Key Questions

| Question | Why It Matters |
|----------|---------------|
| **What do you do?** | Designer / developer / photographer / freelancer / student / hybrid |
| **Who will visit this?** | Recruiters, clients, collaborators, gallery curators |
| **What's the #1 goal?** | Get hired / get freelance clients / get into a program / just have a presence |
| **How many projects to show?** | 3–6 is ideal; quality over quantity |
| **Do you have a brand already?** | Existing colors, fonts, logo — or starting fresh |
| **Tone?** | Minimal, bold, dark, playful, editorial, technical, luxury |
| **Any reference portfolios?** | Sites you love or want to differentiate from |

### 0.2 Portfolio Archetype Mapping

| Archetype | Key Sections | Visual Priority |
|-----------|-------------|-----------------|
| **Designer** | Hero + Selected Work (case studies) + About + Skills + Contact | Case study detail, process shots |
| **Developer** | Hero + Projects (live links + GitHub) + Skills/Stack + About + Contact | Code snippets, live demos, GitHub stats |
| **Creative** | Full-bleed Hero + Gallery/Grid + About + Contact | Imagery dominates, minimal UI chrome |
| **Freelancer** | Hero + Services + Work + Testimonials + Contact | CTA prominence, trust signals |
| **Student** | Hero + Projects + Skills + About + Contact | Story arc — growth and potential |

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

# Portfolio-specific patterns
python3 src/ui-ux-pro-max/scripts/search.py "portfolio" --domain landing --max-results 5

# Product-type rules for personal sites
python3 src/ui-ux-pro-max/scripts/search.py "portfolio personal site" --domain product

# Full design system
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --design-system -p "Portfolio"

# Stack-specific snippets
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack html
```

### 1.2 Portfolio-Specific Aesthetic Principles

A portfolio IS the design work. If it looks generic, the viewer assumes the work is generic.

| ❌ Generic (avoid) | ✅ Distinctive (aim for) |
|---|---|
| White background, Inter font, grey text | A palette that expresses personality |
| "Hi, I'm [Name]. I'm a designer." hero | An opening line that creates intrigue |
| 3-column card grid with identical padding | A layout that reflects the work's character |
| Hover: opacity 0.8 | Hover: reveals project info with personality |
| Footer: "© 2024 Name. All rights reserved." | Footer that's an extension of the brand |

### 1.3 Color Strategy for Portfolios

Unlike landing pages (accent = CTA), portfolio colors express **personality**:

| Persona | Palette Direction |
|---------|-----------------|
| Minimalist designer | Near-white + 1 strong accent, generous whitespace |
| Bold creative | Dark bg + vivid accent, high contrast |
| Technical developer | Dark/terminal aesthetic OR clean light with code-inspired accents |
| Warm freelancer | Earthy tones, approachable, human-feeling |
| Luxury/premium | Deep neutrals + gold/champagne, refined typography |

### 1.4 Design Brief for Portfolios

```markdown
## Portfolio Design Brief: <Name>

### Identity
- **Who:** <name + role>
- **Archetype:** <designer / developer / creative / freelancer / student>
- **Portfolio goal:** <get hired / get clients / just have a presence>
- **Target viewer:** <recruiters / clients / collaborators>

### Personality (pick 3 adjectives)
- <e.g., precise, warm, bold>

### Style Direction
- **Primary style:** <from search — e.g., "Dark Editorial", "Soft Minimalism">
- **One-word aesthetic:** <e.g., "focused", "expressive", "technical">
- **Why this style fits the work:** <one sentence>

### Color Palette
- **Background:** <hex> — <name>
- **Background secondary:** <hex> — for cards/panels
- **Text primary:** <hex>
- **Text muted:** <hex>
- **Accent:** <hex> — used sparingly, maximum impact
- **Border/divider:** <hex>

### Typography
- **Heading font:** <name> + weight + Google Fonts URL
- **Body font:** <name> + Google Fonts URL
- **Heading style note:** <e.g., "tight tracking, large scale contrast">

### Layout Pattern
- **Grid:** <e.g., asymmetric 2-col, masonry, full-bleed row>
- **Project card style:** <e.g., image-first with hover overlay, minimal text card>
- **Section transitions:** <e.g., full-width dividers, staggered entry, none>

### Interactions
- **Project card hover:** <e.g., scale + overlay with title/tags>
- **Scroll animations:** <e.g., stagger fade-up on work grid>
- **Cursor:** <default / custom dot / none>
```

### 1.5 Quality Criteria

- [ ] Archetype identified, primary goal clear
- [ ] 3+ style options searched before deciding
- [ ] Palette expresses personality (not just "clean" defaults)
- [ ] Font pairing has contrast — one display, one body
- [ ] Layout pattern matches work type (photography ≠ code projects)
- [ ] User approved Design Brief

---

## Phase 2 — Content

### 2.1 Hero Copy

The hero opening line is the most important text on the page. It must answer: **who are you, what do you do, and why does it matter** — with personality.

**Formula options:**

| Formula | Example |
|---------|---------|
| Role + differentiator | "Product designer who obsesses over the 0.1-second moments" |
| Outcome-first | "I make complex software feel simple" |
| Bold statement | "Good design is invisible. Great design is inevitable." |
| Playful/direct | "I build things for the web. Currently open to work." |
| Name + descriptor | "Hi, I'm Maya — I design for humans, not screens" |

Avoid:
- "I'm a passionate designer with 5 years of experience"
- "Welcome to my portfolio"
- Generic adjectives: creative, innovative, dedicated, passionate

### 2.2 Project Descriptions

Each project entry needs:

```markdown
### <Project Name>
**Role:** <your role — e.g., "Lead Designer", "Solo Developer">
**Type:** <e.g., "Mobile App", "Brand Identity", "Web App">
**Tags:** [<tech/skill tags>]

**The challenge:** <1-2 sentences — what problem needed solving>
**What I did:** <2-3 sentences — your specific contribution and decisions>
**The outcome:** <1 sentence with a result if possible — "Reduced onboarding time by 40%">

[Live →](<url>)  [Case Study →](<url>)  [GitHub →](<url>)
```

Rules:
- Lead with the PROBLEM, not the technology
- Your ROLE must be specific — "designer" is not a role, "designed the onboarding flow" is
- If you have metrics, use them. If not, describe the qualitative outcome
- 3–6 projects max — curate ruthlessly

### 2.3 About Section

Structure:
1. **Who you are** — current state (role, location, what you're focused on)
2. **How you got here** — brief arc (not a resume, a story)
3. **What you care about** — the "why" behind the work
4. **What you're looking for** — open to work / specific types of projects / collaborations

Length: 80–150 words. Not a bio, not a CV. A person.

### 2.4 Skills / Stack Section

**For designers:**
```
Design: Figma, Prototyping, Design Systems, Typography, Motion
Research: User Interviews, Usability Testing, Competitive Analysis
```

**For developers:**
```
Frontend: React, TypeScript, Next.js, CSS/Tailwind
Backend: Node.js, PostgreSQL, REST/GraphQL
Tools: Git, Docker, Vercel, Figma
```

Display as tag clouds, icon grids, or grouped lists — never just a plain bullet list.

### 2.5 Contact Section

Keep it dead simple:
- Email (the most important thing — make it clickable with `mailto:`)
- 2–3 social links (GitHub, LinkedIn, Twitter/X, Dribbble — pick what's relevant)
- Optional: a short "currently available for..." line
- Optional: a simple contact form (Formspree)

No contact forms with 6 fields. No "let's grab coffee" filler.

### 2.6 Quality Criteria

- [ ] Hero opening line has personality (not generic)
- [ ] Each project leads with the problem, not the tech
- [ ] Roles are specific, not just job titles
- [ ] About section reads like a person wrote it
- [ ] 3–6 projects, curated for quality
- [ ] Contact section has a mailto link as the primary action

---

## Phase 3 — Build

### 3.1 HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><Name> — <Role></title>
  <meta name="description" content="<150-char personal description>">

  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=<HeadingFont>:wght@400;600;700&family=<BodyFont>:wght@400;500&display=swap" rel="stylesheet">

  <!-- OG Tags -->
  <meta property="og:title" content="<Name> — <Role>">
  <meta property="og:description" content="<description>">
  <meta property="og:image" content="<og-image.jpg>">
  <meta name="twitter:card" content="summary_large_image">

  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <nav class="nav" id="nav">...</nav>

  <main>
    <section class="hero" id="home">...</section>
    <section class="work" id="work">...</section>
    <section class="about" id="about">...</section>
    <section class="skills" id="skills">...</section>  <!-- optional -->
    <section class="contact" id="contact">...</section>
  </main>

  <footer class="footer">...</footer>

  <div class="project-modal" id="modal" aria-hidden="true">...</div>  <!-- optional lightbox -->

  <script src="main.js"></script>
</body>
</html>
```

### 3.2 CSS Architecture

```css
/* ── 1. Google Fonts ── */
/* ── 2. CSS Variables ── */
:root {
  /* Colors from Design Brief */
  --color-bg: <hex>;
  --color-bg-secondary: <hex>;
  --color-text: <hex>;
  --color-text-muted: <hex>;
  --color-accent: <hex>;
  --color-border: <hex>;

  /* Typography */
  --font-heading: '<HeadingFont>', serif;
  --font-body: '<BodyFont>', sans-serif;

  /* Type scale */
  --text-hero: clamp(3rem, 8vw, 7rem);
  --text-h2: clamp(1.75rem, 4vw, 2.75rem);
  --text-h3: clamp(1.125rem, 2vw, 1.5rem);
  --text-body: 1.0625rem;
  --text-sm: 0.875rem;

  /* Spacing */
  --section-padding: clamp(5rem, 12vw, 10rem);
  --container-width: 1200px;
  --gap: 1.5rem;
  --radius: 8px;
  --radius-lg: 16px;

  /* Transitions */
  --transition: 0.3s ease;
  --transition-slow: 0.6s ease;
}

/* ── 3. Reset + Base ── */
/* ── 4. Layout (.container, .section, .grid-2, .grid-3) ── */
/* ── 5. Nav (fixed, scroll-aware bg change) ── */
/* ── 6. Hero ── */
/* ── 7. Work / Project Grid ── */
/* ── 8. Project Card + Hover ── */
/* ── 9. About ── */
/* ── 10. Skills ── */
/* ── 11. Contact ── */
/* ── 12. Footer ── */
/* ── 13. Modal / Lightbox (if used) ── */
/* ── 14. Scroll animations ── */
/* ── 15. Responsive ── */
```

### 3.3 Project Grid

```html
<!-- Work section with optional filter -->
<section class="work" id="work">
  <div class="container">
    <div class="work__header">
      <h2 class="work__title">Selected Work</h2>
      <!-- Optional filter tabs -->
      <div class="work__filters" role="tablist">
        <button class="filter-btn active" data-filter="all">All</button>
        <button class="filter-btn" data-filter="design">Design</button>
        <button class="filter-btn" data-filter="dev">Development</button>
      </div>
    </div>

    <div class="work__grid" id="work-grid">
      <!-- Project Card -->
      <article class="project-card" data-category="design" data-index="0">
        <div class="project-card__image">
          <img src="assets/images/project-1.jpg" alt="<Project Name> — <brief description>" loading="lazy">
          <div class="project-card__overlay">
            <span class="project-card__type"><Type></span>
            <h3 class="project-card__title"><Project Name></h3>
            <div class="project-card__links">
              <a href="<live-url>" target="_blank" rel="noopener" class="project-link">Live ↗</a>
              <a href="<case-study-url>" class="project-link">Case Study →</a>
            </div>
          </div>
        </div>
        <div class="project-card__meta">
          <div class="project-card__tags">
            <span class="tag">Figma</span>
            <span class="tag">React</span>
          </div>
          <p class="project-card__desc"><One sentence problem description></p>
        </div>
      </article>
    </div>
  </div>
</section>
```

```css
.work__grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
  gap: var(--gap);
}

.project-card__image {
  position: relative;
  overflow: hidden;
  border-radius: var(--radius);
  aspect-ratio: 16 / 10;
  background: var(--color-bg-secondary);
}

.project-card__image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.project-card:hover .project-card__image img {
  transform: scale(1.04);
}

.project-card__overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.75);
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 1.5rem;
  opacity: 0;
  transition: opacity var(--transition);
}

.project-card:hover .project-card__overlay,
.project-card:focus-within .project-card__overlay {
  opacity: 1;
}
```

### 3.4 Project Filter

```js
// main.js — project filter
const filterBtns = document.querySelectorAll('.filter-btn')
const projectCards = document.querySelectorAll('.project-card')

filterBtns.forEach(btn => {
  btn.addEventListener('click', () => {
    const filter = btn.dataset.filter

    filterBtns.forEach(b => b.classList.remove('active'))
    btn.classList.add('active')

    projectCards.forEach(card => {
      const match = filter === 'all' || card.dataset.category === filter
      card.style.display = match ? 'block' : 'none'
      // or use animate: card.classList.toggle('hidden', !match)
    })
  })
})
```

### 3.5 Scroll-Aware Nav

```js
const nav = document.getElementById('nav')
window.addEventListener('scroll', () => {
  nav.classList.toggle('nav--scrolled', window.scrollY > 60)
}, { passive: true })
```

```css
.nav {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  padding: 1.5rem 2rem;
  transition: background var(--transition), padding var(--transition);
}
.nav--scrolled {
  background: var(--color-bg);
  padding: 1rem 2rem;
  box-shadow: 0 1px 0 var(--color-border);
}
```

### 3.6 Scroll Animations (staggered for grid)

```js
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
  transform: translateY(20px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}
.animate-on-scroll.visible {
  opacity: 1;
  transform: translateY(0);
}
/* Stagger project cards */
.project-card { transition-delay: calc(var(--index, 0) * 0.08s); }

@media (prefers-reduced-motion: reduce) {
  .animate-on-scroll { opacity: 1; transform: none; transition: none; }
}
```

### 3.7 Implementation Rules

| Rule | Detail |
|------|--------|
| **CSS variables only** | No hardcoded colors or spacing in components |
| **`clamp()` for type** | Hero heading scales with viewport, no media query needed |
| **`loading="lazy"` on all images** | Especially project screenshots — they're large |
| **`aspect-ratio` on image containers** | Prevents layout shift before images load |
| **Hover must work on keyboard too** | `focus-within` on project cards, not just `:hover` |
| **Mobile nav** | Hamburger at <768px, full menu on desktop |
| **Alt text** | Every project image needs descriptive alt text |
| **Active nav state** | Highlight current section via IntersectionObserver |

### 3.8 Quality Criteria

- [ ] CSS variables only (no hardcoded hex/px values in components)
- [ ] Hero heading uses `clamp()` for responsive scaling
- [ ] Project card hover works via both mouse and keyboard (`focus-within`)
- [ ] All images have `loading="lazy"` and explicit `aspect-ratio`
- [ ] Filter works correctly (all / per category)
- [ ] Scroll animations use IntersectionObserver, `prefers-reduced-motion` respected
- [ ] Nav becomes visible/compact on scroll
- [ ] Mobile nav works at 375px
- [ ] All project images have descriptive alt text
- [ ] OG tags set (title, description, image)
- [ ] No console errors

---

## Phase 4 — Deliver

### 4.1 Preview

```bash
# Start local preview
python3 -m http.server 8080
# or
npx serve .
```

Use `start_web_service` tool for a hosted preview URL.

### 4.2 Validation Checklist

**Visual:**
- [ ] Design Brief's aesthetic is recognizable throughout
- [ ] Fonts loaded correctly, no layout shift
- [ ] Responsive at 375px / 768px / 1280px
- [ ] No horizontal scroll at any breakpoint
- [ ] Dark mode (if applicable) looks intentional, not accidental

**Content:**
- [ ] Hero opening line has personality
- [ ] 3–6 projects shown, each with problem + role + outcome
- [ ] About section reads like a human
- [ ] Contact has a working mailto link

**Technical:**
- [ ] Page loads fast (images optimized or use placeholder until user provides real ones)
- [ ] No console errors
- [ ] OG preview looks good (check with opengraph.xyz)
- [ ] Favicon set

### 4.3 Deploy to Vercel

```bash
npx vercel --prod
```
Or use `deploy_to_vercel` tool. Zero config needed for static HTML.

### 4.4 Image Placeholders

If the user doesn't have project screenshots yet, use elegant placeholders:

```html
<!-- Placeholder with aspect ratio preserved -->
<div class="project-placeholder" style="aspect-ratio: 16/10; background: var(--color-bg-secondary);">
  <span class="project-placeholder__label">Project Screenshot</span>
</div>
```

Or use a generated gradient:
```css
.project-card--placeholder .project-card__image {
  background: linear-gradient(135deg, var(--color-bg-secondary) 0%, var(--color-accent-dim) 100%);
}
```

### 4.5 Delivery Summary Format

```markdown
## ✅ Portfolio Live!

**Name:** <Name>
**Role:** <Role>
**Archetype:** <designer / developer / creative / freelancer / student>
**Style:** <primary style from Design Brief>
**Palette:** <2-3 main colors>
**Fonts:** <Heading> + <Body>

### Projects showcased
- <list of projects>

### Sections built
- Hero / Work (N projects, filter: <categories>) / About / Skills / Contact

### Preview
<live URL>

### To deploy to production
1. `npx vercel --prod` from this folder
2. Connect to custom domain in Vercel dashboard

### Next steps
- Replace placeholder images with real screenshots
- Add your real email to the contact section
- Set up analytics (Plausible / GA4)
- Consider adding a case study page for your best project
```

---

## Technical Reference

### Portfolio Patterns (common layouts)

| Pattern | Best For |
|---------|----------|
| **Single page, anchor nav** | Most portfolios — simple, fast |
| **Multi-page (index + case studies)** | Designers with deep process documentation |
| **Full-bleed grid** | Photographers, illustrators — image-forward |
| **List / editorial** | Developers, writers — text-forward |
| **Experimental / interactive** | Creative technologists — the portfolio IS the work |

### Design Database Quick Reference

```bash
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain style
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain color
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain typography
python3 src/ui-ux-pro-max/scripts/search.py "portfolio" --domain landing
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain product
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --design-system -p "Portfolio"
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack html
```

### Quality Maxims

1. **The portfolio IS the design work.** If it looks generic, the viewer assumes the work is generic.
2. **Curate ruthlessly.** 4 great projects beat 10 mediocre ones. Every time.
3. **Lead with problems, not tools.** Nobody hired anyone for knowing Figma. They hired them for solving the right problems.
4. **Your opening line is your handshake.** Generic hero copy = forgettable person. One specific, true sentence > three polished clichés.
5. **Interactions should feel like the work.** A motion designer's portfolio should move. A minimalist's should be still. The UI IS the proof of concept.
6. **Make contact embarrassingly easy.** Big visible email address. One click.
