---
name: blog-creation
description: |
  Create a complete, personalized blog site using the Noteworthy theme (Next.js).
  Integrates the ui-ux-design-pro database (67+ styles, 161 palettes, 57 font pairings)
  to give every blog a distinctive visual identity — not the same default look.
  The agent guides through design research → style customization → configuration →
  content generation → live preview → deployment.
  Triggers:
  - "create blog"
  - "new blog"
  - "build blog"
  - "make blog"
  - "personal blog"
  - "创建博客"
  - "新建博客"
  - "制作博客"
  - "blog creation"
---

# SKILL.md — Personal Blog Creation

> Build a complete, deployable personal blog on the Noteworthy (Next.js) theme —
> with a distinctive visual identity researched from 67+ UI styles, 161 color palettes,
> and 57 font pairings. Every blog looks uniquely crafted, not AI-slop generic.

---

## Overview

**Two-track pipeline:**

**Track A — Quick Launch** (user wants to go live fast)
Skip design research → Phase 1 (env setup) → Phase 2 (requirements) → Phase 3 (config) → Phase 4 (content) → Phase 5 (preview) → Phase 6 (deliver)

**Track B — Designed Blog** (user wants a distinctive visual identity)
Phase 0 (design research) → Phase 1 (env) → Phase 2 (requirements) → Phase 3 (config + style applied) → Phase 4 (content) → Phase 5 (preview) → Phase 6 (deliver)

Default to **Track B**. Only skip design research if user explicitly says "just get it running".

### The Anti-Generic-Blog Rule

The default Noteworthy theme looks the same for every user who doesn't customize it. This skill fixes that. Before writing a single config line, search the design database and commit to a specific aesthetic direction:

- A tech blogger should NOT look the same as a travel blogger or a designer's portfolio
- Font choices, color palette, and visual tone should reflect the author's personality and content
- "Clean and minimal" is not a design direction — it's a cop-out

---

## Phase 0 — Design Research

### 0.1 Purpose

Find a visual identity that fits the author's personality, writing style, and content type — before touching any config. This phase produces a **Blog Design Brief** that guides all subsequent customization.

### 0.2 The Design Database

```bash
# Search commands (run from the workspace root)
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain style --max-results 5
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain color --max-results 5
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain typography --max-results 3
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain google-fonts --max-results 5
python3 src/ui-ux-pro-max/scripts/search.py "personal blog" --domain product

# Full design system generation shortcut
python3 src/ui-ux-pro-max/scripts/search.py "<blog type + vibe>" --design-system -p "<Blog Name>"

# Next.js stack-specific snippets
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack nextjs
```

### 0.3 Blog-Specific Search Queries

Use these as starting points, adapted to the user's actual content type:

| Blog Type | Style Query | Color Query | Typography Query |
|-----------|-------------|-------------|-----------------|
| Tech / Dev | `"minimal code developer"` | `"dark technical"` | `"monospace code"` |
| Design / Creative | `"editorial bold creative"` | `"vibrant artistic"` | `"display expressive"` |
| Writing / Essay | `"literary editorial"` | `"warm neutral"` | `"serif elegant"` |
| Travel / Lifestyle | `"organic warm photography"` | `"earthy warm"` | `"humanist clean"` |
| Business / Thought Leadership | `"professional refined"` | `"trust authority"` | `"modern sans"` |
| Personal / Journal | `"personal warm handcrafted"` | `"soft pastel"` | `"friendly readable"` |
| Photography | `"dark minimal dramatic"` | `"monochrome accent"` | `"refined minimal"` |

### 0.4 Research Process

**Step A — Understand the Author**

Ask (or infer from context):
- What do you write about? (tech, design, travel, life, business, etc.)
- Who are your readers?
- What vibe/feeling should visitors get? (inspiring, technical, intimate, authoritative...)
- Any blogs or sites you love the look of?
- Light or dark preference? Or let the research decide?

**Step B — Commit to a Direction BEFORE Searching**

Before running any search, make a creative bet:
- What's the ONE word that should define this blog's aesthetic? (e.g., "warm", "sharp", "dreamy", "raw")
- This becomes the lens for evaluating search results

**Step C — Search the Database**

Run at minimum:
1. Product type: `python3 src/ui-ux-pro-max/scripts/search.py "personal blog" --domain product`
2. Style (2-3 queries): match blog type + vibe
3. Color (1-2 queries): match mood + industry
4. Typography (1-2 queries): match reading experience

**Step D — Compose Blog Design Brief**

```markdown
## Blog Design Brief: <Blog Name>

### Author & Content
- **Blog type:** <tech / creative / writing / travel / etc.>
- **Audience:** <who reads this>
- **Aesthetic word:** <the one-word creative direction>

### Style Direction
- **Primary style:** <from search results — e.g., "Dark Minimal", "Warm Editorial">
- **Why this style:** <one sentence justification>

### Color Palette
- **Background:** <hex> — <name>
- **Text primary:** <hex> — <name>
- **Accent:** <hex> — <name>
- **Surface/card:** <hex> — <name>
- **Mood:** <from search results>

### Typography
- **Heading font:** <name> (Google Fonts link)
- **Body font:** <name> (Google Fonts link)
- **Why this pairing:** <one sentence>

### Noteworthy Customization Plan
- CSS variables to override: <list>
- Tailwind config changes: <list>
- Any layout/component tweaks: <list>
```

### 0.5 Quality Criteria

- [ ] Author's content type identified
- [ ] At least 3 style options searched and considered
- [ ] Color palette has bg, text, accent, surface
- [ ] Font pairing selected with Google Fonts links
- [ ] Blog Design Brief approved by user

---

## Phase 1 — Environment Setup

### 1.1 Verify Project Structure

The Noteworthy theme must already be cloned in the workspace. Check:

```bash
ls -la
# Expected: package.json, src/, content/, public/
```

If not present, clone it:
```bash
git clone https://github.com/cnly1987/noteworthy.git .
# OR the canonical repo — verify with user
```

### 1.2 Install Dependencies

```bash
npm install
```

Verify key dependencies are present: `next`, `react`, `react-dom`, `gray-matter`, `remark`.

### 1.3 Verify Build Readiness

```bash
npm run build 2>&1 | tail -20
```

If build fails, diagnose and fix before proceeding.

### 1.4 Quality Criteria

- [ ] Project structure verified
- [ ] `npm install` completed without errors
- [ ] Build passes (or known issues documented)

---

## Phase 2 — Collect Blog Requirements

### Required Fields

| Field | Description | Example |
|-------|-------------|---------|
| `title` | Blog name | "Syntax & Soul" |
| `description` | One-sentence tagline | "A developer's notes on code, design, and life" |
| `author` | Author's name | "Alex Chen" |
| `language` | Site language | `en` / `zh` / `ja` |

### Optional Fields (with defaults)

| Field | Default | Description |
|-------|---------|-------------|
| `postsPerPage` | `10` | Posts per listing page |
| `nav` | Home, Archive, About | Navigation links |
| `socialLinks` | empty | GitHub, Twitter, LinkedIn, etc. |
| `favicon` | Noteworthy default | Path to custom favicon |

### 2.1 Content Direction

Also gather:
- **About page**: Brief author bio (will expand in Phase 4)
- **Initial posts**: Topics for 2-3 starter posts (will write in Phase 4)
- **Tone**: casual / technical / literary / mixed

---

## Phase 3 — Configuration & Style Application

### 3.1 Write Site Configuration

Update `src/lib/config.ts`:

```typescript
export const siteConfig = {
  title: "<blog title>",
  description: "<tagline>",
  author: "<name>",
  language: "<lang>",
  postsPerPage: <N>,
  nav: [
    { label: "Home", href: "/" },
    { label: "Archive", href: "/archive" },
    { label: "About", href: "/about" },
  ],
  socialLinks: {
    // github: "https://github.com/username",
    // twitter: "https://twitter.com/username",
  },
}
```

### 3.2 Apply Design Brief (Track B only)

If a Blog Design Brief was created in Phase 0, apply it now.

**Step A — Override CSS Variables**

In `src/app/globals.css`, add/override CSS custom properties:

```css
:root {
  /* From Blog Design Brief */
  --color-bg: <background hex>;
  --color-text: <text primary hex>;
  --color-accent: <accent hex>;
  --color-surface: <surface hex>;

  /* Typography */
  --font-heading: '<Heading Font>', serif;
  --font-body: '<Body Font>', sans-serif;
}

/* Dark mode (if design called for it) */
[data-theme="dark"] {
  --color-bg: <dark bg>;
  --color-text: <dark text>;
}
```

**Step B — Load Google Fonts**

In `src/app/layout.tsx`, add font imports (or use `next/font/google`):

```tsx
import { <FontName> } from 'next/font/google'

const headingFont = <FontName>({
  subsets: ['latin'],
  variable: '--font-heading',
  weight: ['400', '600', '700'],
})
```

**Step C — Tailwind Config**

In `tailwind.config.js` or `tailwind.config.ts`:

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        accent: 'var(--color-accent)',
        surface: 'var(--color-surface)',
      },
      fontFamily: {
        heading: ['var(--font-heading)'],
        body: ['var(--font-body)'],
      },
    },
  },
}
```

**Step D — Apply Fonts to Components**

In key components (title, headings, nav), apply `font-heading` class; body text gets `font-body`.

### 3.3 Localization Rules

- If `language: "zh"`: use Chinese placeholder content, set `<html lang="zh">`
- If `language: "ja"`: use Japanese placeholder, set `<html lang="ja">`
- Always update the `lang` attribute in `src/app/layout.tsx`

### 3.4 Quality Criteria

- [ ] `config.ts` written with all required fields
- [ ] CSS variables match Blog Design Brief colors (Track B)
- [ ] Google Fonts loaded and applied to heading/body (Track B)
- [ ] Tailwind config extended (Track B)
- [ ] `lang` attribute set correctly

---

## Phase 4 — Generate Content

### 4.1 About Page

Write `content/about.md`:

```markdown
---
title: About
---

# About <Author Name>

<2-3 paragraph bio that matches the blog's tone>

- What you write about
- Your background (brief)
- Where to find you (social links)
```

Tone must match the Design Brief's aesthetic direction — a "warm personal" blog gets a warm bio; a "sharp technical" blog gets a precise, direct bio.

### 4.2 Blog Posts

Write 2-3 starter posts in `content/posts/`. Each post:

```markdown
---
title: "<Post Title>"
date: "<YYYY-MM-DD>"
tags: ["<tag1>", "<tag2>"]
excerpt: "<1-2 sentence summary>"
---

<Full post content, 400-800 words>
```

**Post Generation Guidelines**

- Topics must match what the user said they write about
- Writing style matches the tone they specified
- Each post must be substantive — not placeholder filler
- Use headings, code blocks, or other formatting appropriate to the content type
- At least one post should demonstrate the blog's "voice" clearly

### 4.3 Clean Up Existing Content

Remove any default template content:
```bash
rm -f content/posts/example-*.md
rm -f content/posts/hello-world.md  # or similar defaults
```

### 4.4 Quality Criteria

- [ ] About page written in author's voice
- [ ] 2-3 real posts written (not placeholder)
- [ ] All posts have correct frontmatter (title, date, tags, excerpt)
- [ ] Default example content removed
- [ ] Writing tone consistent across all content

---

## Phase 5 — Preview & Validation

### 5.1 Start Dev Server

```bash
npm run dev
```

Use `start_web_service` tool to get a live preview URL.

### 5.2 Validation Checklist

**Visual (Track B — design applied):**
- [ ] Custom fonts loading correctly (check Network tab)
- [ ] Color palette applied throughout (bg, text, accent)
- [ ] No fallback system fonts visible where custom fonts should appear
- [ ] Design Brief's aesthetic direction is recognizable

**Functional:**
- [ ] Homepage loads without errors
- [ ] Post listing renders correctly
- [ ] Individual post page renders correctly
- [ ] About page renders correctly
- [ ] Navigation links work
- [ ] Social links correct (if provided)
- [ ] Language/locale correct

**Content:**
- [ ] Blog title and tagline display correctly
- [ ] Author name displays correctly
- [ ] Posts show correct dates and tags
- [ ] No placeholder or "Lorem ipsum" content

### 5.3 Fix Issues

If fonts don't load: check `next/font/google` import syntax, verify font name spelling, clear `.next` cache and restart.

If colors don't apply: check CSS variable names match between `globals.css` and component usage, verify Tailwind purge isn't removing the classes.

---

## Phase 6 — Delivery

### 6.1 Present to User

```markdown
## ✅ Your Blog is Ready!

**Blog:** <title> — <tagline>
**Style:** <primary style from Design Brief>
**Palette:** <main colors>
**Fonts:** <heading> + <body>

### What's included
- <N> starter posts
- About page
- Custom visual identity (Track B)

### Preview
<live URL from start_web_service>

### To deploy
**Vercel (recommended):**
1. Push to GitHub
2. Connect repo to Vercel
3. Deploy → automatic

**Or deploy now:** I can run `deploy_to_vercel` if your project is ready.
```

### 6.2 Post-Delivery Options

Offer the user:
1. **Write more posts** — "Want me to write additional posts on specific topics?"
2. **Tweak the design** — "Want to adjust colors, fonts, or layout?"
3. **Add features** — "Want search, RSS feed, comments, or newsletter signup?"
4. **Deploy now** — "Ready to go live on Vercel?"
5. **Done** — "All good! Let me know when you want to add content."

---

## Technical Reference

### Noteworthy Theme Structure

```
noteworthy/
├── src/
│   ├── app/
│   │   ├── layout.tsx          # HTML shell, fonts, metadata
│   │   ├── globals.css         # CSS variables + Noteworthy base styles
│   │   └── page.tsx            # Homepage
│   ├── components/             # Reusable UI components
│   └── lib/
│       └── config.ts           # ← PRIMARY CONFIG FILE
├── content/
│   ├── about.md                # About page content
│   └── posts/                  # Blog posts (MDX/Markdown)
├── public/
│   └── fonts/                  # Self-hosted fonts (Noto Sans/Serif defaults)
├── tailwind.config.js
└── package.json
```

### Key Files for Customization

| File | What to Change |
|------|----------------|
| `src/lib/config.ts` | Title, description, author, social links, nav, postsPerPage |
| `src/app/globals.css` | CSS variables (colors, fonts), base typography |
| `src/app/layout.tsx` | HTML lang, Google Fonts imports |
| `tailwind.config.js` | Extend with design token variables |
| `content/about.md` | About page content |
| `content/posts/*.md` | Blog posts |

### Dependencies

Core: `next`, `react`, `react-dom`, `gray-matter`, `remark`, `remark-html`
Styling: `tailwindcss`, `autoprefixer`, `postcss`
Optional: `@next/font` (for Google Fonts via next/font/google)

### Design Database Files (ui-ux-pro-max)

All CSV data lives in `src/ui-ux-pro-max/data/`:

| File | Records | Used In Phase |
|------|---------|--------------|
| `styles.csv` | 67+ | Phase 0 — style direction |
| `colors.csv` | 161 | Phase 0 — color palette |
| `typography.csv` | 57 | Phase 0 — font pairing |
| `products.csv` | 161 | Phase 0 — blog type rules |
| `google-fonts.csv` | 1900+ | Phase 0 — font discovery |

### Quality Maxims

1. **Every blog needs a face.** Run design research even for "simple" blogs. A distinctive aesthetic takes 10 minutes; the generic look lasts forever.
2. **Content must be real.** Never write placeholder posts. If the user hasn't given you topics, ask — don't generate generic filler.
3. **Fonts are the cheapest upgrade.** Swapping from Noto Sans to a well-chosen Google Font pair costs nothing and changes everything.
4. **Track B by default.** Unless the user explicitly says "just get it running", run Phase 0 first.
