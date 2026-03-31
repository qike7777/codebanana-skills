---
name: build-web-app
description: |
  Combined skill for designing and building production-ready web apps.
  Part 1 (Design): Research styles, generate design systems, implement UIs — backed by 67+ styles,
  161 color palettes, 57 font pairings, 99 UX guidelines across 10 web tech stacks.
  Part 2 (Engineering): Structured development workflow — brainstorm → plan → TDD → code review → verify → ship.
  Triggers:
  - "build a web app"
  - "design a web app"
  - "build web app"
  - "create a website"
  - "landing page"
  - "web frontend"
  - "做一个网站"
  - "前端开发"
  - "前端设计"
---

# SKILL.md — Build a Web App

> From idea to deployed web app: design system first, then clean production code.
> Combines a searchable design database (67+ styles, 161 palettes, 57 font pairings, 99 UX guidelines)
> with a rigorous engineering workflow (brainstorm → plan → TDD → code review → verify → ship).

---

## Overview

This skill combines two capabilities into one pipeline:

**Part 1 — Design (Phases 1–6)**
Research styles → generate design system (primitive → semantic → component tokens) → implement UI → brand polish → accessibility review → deliver

**Part 2 — Engineering (Phases 7–12)**
Brainstorm requirements → write implementation plan → build with TDD → code review → verify → handoff

### Supported Web Tech Stacks

| Stack | Support Level |
|-------|--------------|
| React + Tailwind + shadcn/ui | Full (primary) |
| Next.js | Full |
| Vue.js | Full |
| Svelte | Full |
| HTML/CSS | Full |
| Tailwind CSS | Full |
| MUI / Chakra UI | Partial |

> **Web-only.** This skill covers web stacks only. For React Native, Flutter, or SwiftUI → use Build a Mobile App.

### When to Start from Which Phase

| Scenario | Start From |
|----------|------------|
| "Build me a web app from scratch" | Phase 1 (Design Research) |
| "I have designs, just implement them" | Phase 7 (Brainstorm) → Phase 9 (Execute) |
| "Just a quick UI tweak" | Phase 8 (Plan) → Phase 9 (Execute, lightweight) |
| "Review my code" | Phase 10 (Code Review) |
| "Fix this bug" | Phase 9 (Execute, lightweight TDD) |

### Design Philosophy — Anti-AI-Slop Manifesto

Every design produced by this skill must be **distinctive, intentional, and memorable**. The #1 enemy is "AI slop" — the generic, soulless aesthetic that plagues AI-generated UIs.

#### Commit to a Bold Aesthetic Direction

Before touching code, make a creative decision. Pick a **specific** aesthetic tone and execute it with conviction:

- **Purpose**: What problem does this interface solve? Who uses it?
- **Tone**: Choose deliberately — brutally minimal, maximalist chaos, retro-futuristic, organic/natural, luxury/refined, playful/toy-like, editorial/magazine, brutalist/raw, art deco/geometric, soft/pastel, industrial/utilitarian… There are dozens of directions. Don't default to "clean and modern."
- **Differentiation**: What's the ONE thing someone will remember about this design?

Bold maximalism and refined minimalism both work — the key is **intentionality, not intensity**.

#### The "Never Do" List (AI Slop Indicators)

| ❌ Never Do This | ✅ Do This Instead |
|---|---|
| Default to Inter, Roboto, Arial, system fonts | Choose distinctive, characterful fonts that elevate the aesthetic |
| Purple/blue gradient on white background | Commit to a specific palette with personality |
| Predictable card grids with uniform spacing | Asymmetry, overlap, diagonal flow, grid-breaking elements |
| Cookie-cutter hero + 3 feature cards + CTA | Design a layout that serves the CONTENT, not a template |
| Same design regardless of context | Every project should feel uniquely designed for its purpose |

---

## Phase 1 — Design Research

### 1.1 Purpose

Understand the project requirements and search the design database to find the best matching styles, colors, typography, and UX patterns. This phase produces a **Design Brief** — the foundation for all subsequent work.

### 1.2 The Design Database

The agent has access to a searchable BM25 database with these domains:

| Domain | Records | What It Contains |
|--------|---------|-----------------|
| `style` | 67+ | UI style categories with keywords, colors, effects, best-for/avoid, framework compatibility, complexity |
| `color` | 161 | Color palettes with hex codes, mood, industry fit, accessibility notes |
| `typography` | 57 | Font pairings (heading + body) with Google Fonts links, CSS imports, use cases |
| `product` | 161 | Product-type rules: landing page pattern, style priority, color mood, typography mood, key effects, anti-patterns |
| `ux` | 99 | UX guidelines by category (navigation, forms, feedback, loading, etc.) |
| `chart` | 25 | Chart types with use cases, best-for, avoid, libraries |
| `landing` | 35 | Landing page pattern templates |
| `google-fonts` | 1900+ | Full Google Fonts catalog with weights, styles, categories |

### 1.3 Search Commands

```bash
# Basic domain search
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain <domain> --max-results <N>

# Available domains: style, color, typography, product, ux, chart, landing, google-fonts
# Default max-results: 3

# Examples:
python3 src/ui-ux-pro-max/scripts/search.py "modern fintech" --domain style --max-results 5
python3 src/ui-ux-pro-max/scripts/search.py "trust professional" --domain color --max-results 3
python3 src/ui-ux-pro-max/scripts/search.py "banking app" --domain product
python3 src/ui-ux-pro-max/scripts/search.py "form validation" --domain ux

# Design system generation (combines style + color + typography + product)
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --design-system -p "<Project Name>"

# Stack-specific code snippets
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack <stack>
# Stacks: html, tailwind, react, nextjs, vue, svelte, shadcn

# JSON output (for programmatic use)
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain style --json
```

### 1.4 Research Process

**Step A — Understand the Project & Define Creative Direction**

Gather from the user:
- What are you building? (app type, industry, target audience)
- What's the vibe/feeling? (modern, playful, corporate, minimal, bold...)
- Any existing brand assets? (colors, fonts, logos)
- Tech stack preference?
- Any design references or inspirations?

Then, before any search, **commit to a bold aesthetic direction**:
- What **extreme** best fits this project? (e.g., "luxury editorial" not just "modern")
- What single visual element will make this **unforgettable**?

**Step B — Search the Database**

1. **Product type search**
   ```bash
   python3 src/ui-ux-pro-max/scripts/search.py "<product type>" --domain product
   ```

2. **Style search**
   ```bash
   python3 src/ui-ux-pro-max/scripts/search.py "<keywords>" --domain style --max-results 5
   ```

3. **Color palette search**
   ```bash
   python3 src/ui-ux-pro-max/scripts/search.py "<mood/industry>" --domain color --max-results 5
   ```

4. **Typography search**
   ```bash
   python3 src/ui-ux-pro-max/scripts/search.py "<style/mood>" --domain typography --max-results 3
   ```

5. **UX guidelines search**
   ```bash
   python3 src/ui-ux-pro-max/scripts/search.py "<feature type>" --domain ux
   ```

**Step C — Compose the Design Brief**

```markdown
## Design Brief: <Project Name>

### Project
- **Type:** <e.g., SaaS Dashboard, E-commerce, Portfolio>
- **Industry:** <e.g., Fintech, Healthcare, Education>
- **Target audience:** <who>
- **Tech stack:** <e.g., Next.js + Tailwind + shadcn/ui>

### Style Direction
- **Primary style:** <from search results>
- **Keywords:** <e.g., modern, clean, professional>

### Color Palette
- **Primary / Secondary / Accent / Neutrals / Semantic colors**

### Typography
- **Heading font / Body font** with Google Fonts links

### Key Effects & Animations

### UX Patterns to Follow

### Anti-Patterns to Avoid

### Landing Page Structure
```

### 1.5 Quality Criteria

- [ ] Product type identified and searched
- [ ] At least 3 style options were considered
- [ ] Color palette includes primary, secondary, accent, neutrals, semantic colors
- [ ] Font pairing selected with Google Fonts links
- [ ] UX guidelines pulled for relevant features
- [ ] Anti-patterns documented
- [ ] User approved the Design Brief

---

## Phase 2 — Design System

### 2.1 Purpose

Generate a complete, token-based design system from the Design Brief. Three-layer architecture: Primitive → Semantic → Component tokens.

### 2.2 Quick Generation

```bash
python3 src/ui-ux-pro-max/scripts/search.py "<project description>" --design-system -p "<Project Name>"
```

### 2.3 Token Architecture

#### Layer 1 — Primitive Tokens (raw values)

```css
:root {
  --color-blue-500: #3b82f6;
  --space-4: 16px;
  --font-size-base: 1rem;
  --radius-md: 8px;
}
```

#### Layer 2 — Semantic Tokens (purpose-based)

```css
:root {
  --color-primary: var(--color-blue-500);
  --color-text-primary: var(--color-gray-900);
  --color-surface: var(--color-white);
  --color-success: #22c55e;
  --color-error: #ef4444;
}
[data-theme="dark"] {
  --color-surface: var(--color-gray-900);
  --color-text-primary: var(--color-white);
}
```

#### Layer 3 — Component Tokens (per-component)

```css
:root {
  --button-height-md: 40px;
  --button-radius: var(--radius-md);
  --card-padding: var(--space-6);
  --input-height: 40px;
}
```

### 2.4 Component Specifications

| Component | Height (sm/md/lg) | Border Radius | States |
|-----------|-------------------|---------------|--------|
| Button | 32/40/48px | 8px | default, hover, active, disabled, loading |
| Input | 36/40/44px | 8px | default, focus, error, disabled |
| Card | auto | 12px | default, hover, selected |
| Modal | auto | 16px | open, closing |

### 2.5 Tailwind Configuration

```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: { DEFAULT: 'var(--color-primary)', hover: 'var(--color-primary-hover)' },
        surface: { DEFAULT: 'var(--color-surface)' },
      },
      fontFamily: {
        heading: ['var(--font-heading)', 'sans-serif'],
        body: ['var(--font-body)', 'sans-serif'],
      },
    },
  },
}
```

### 2.6 Quality Criteria

- [ ] All three token layers defined
- [ ] Dark mode tokens included
- [ ] Colors from Design Brief mapped to tokens
- [ ] Spacing scale is consistent (4px or 8px base)
- [ ] Tailwind config extends (not overrides) defaults

---

## Phase 3 — UI Implementation

### 3.1 Purpose

Build the actual UI using the design system, following the style direction from the Design Brief.

### 3.2 Creative Aesthetics in Implementation

- **Typography**: Choose fonts that are beautiful and interesting — never generic
- **Color**: Commit to a cohesive, opinionated palette — dominant colors with sharp accents
- **Motion**: One well-orchestrated page load with staggered reveals > scattered micro-interactions
- **Layout**: Unexpected layouts — asymmetry, overlap, diagonal flow, grid-breaking elements
- **Backgrounds**: Create atmosphere and depth — never default to solid white/gray

### 3.3 Implementation Process

**Step A — Structure**: Set up page structure, layout skeleton, wire up design tokens

**Step B — Components**: For each component: use shadcn/ui as base → apply tokens → implement all states → ensure responsiveness

**Step C — Styling Rules**

| Rule | Details |
|------|---------| 
| **Never hardcode colors** | Always use tokens: `var(--color-primary)` |
| **Never hardcode spacing** | Use spacing scale: `var(--space-4)` or `p-4` |
| **Never skip states** | hover, focus, active, disabled for every interactive element |
| **Always use semantic HTML** | `<nav>`, `<main>`, `<button>` not `<div onClick>` |
| **Responsive-first** | Mobile → desktop. Use `min-width` breakpoints |
| **Dark mode from day one** | Semantic tokens switch via `[data-theme="dark"]` |

**Step D — shadcn/ui Integration**

```bash
npx shadcn@latest init
npx shadcn@latest add button card input dialog dropdown-menu
```

### 3.4 Responsive Breakpoints

| Breakpoint | Width | Target |
|-----------|-------|--------|
| xs | < 475px | Small phones |
| sm | ≥ 475px | Large phones |
| md | ≥ 768px | Tablets |
| lg | ≥ 1024px | Laptops |
| xl | ≥ 1280px | Desktops |

### 3.5 Quality Criteria

- [ ] All colors use design tokens (no hardcoded hex values)
- [ ] All spacing uses the token scale
- [ ] Every interactive element has proper states
- [ ] Responsive layout works at all breakpoints (375px → 1536px)
- [ ] Dark mode works without layout breakage
- [ ] Animations respect `prefers-reduced-motion`
- [ ] Semantic HTML used throughout
- [ ] **No AI slop**: distinctive font choices, no cliché purple gradients, no cookie-cutter layouts

---

## Phase 4 — Brand & Polish

### 4.1 Brand Identity (if needed)

- **Brand Voice**: personality traits, voice attributes, messaging hierarchy
- **Visual Identity**: logo usage rules, color application, typography hierarchy, imagery style

### 4.2 Asset Creation

**Banners** — generate as HTML+CSS using design tokens:

| Platform | Size |
|----------|------|
| Social — Instagram Post | 1080×1080 |
| Web — Hero | 1920×600 |
| Ad — Medium Rectangle | 300×250 |

**Slides** — HTML-based presentations using design system tokens + Chart.js

### 4.3 Consistency Checklist

- [ ] All pages use the same color palette
- [ ] Typography hierarchy consistent across all pages
- [ ] Button styles unified (same radius, padding, states)
- [ ] Spacing rhythm consistent throughout
- [ ] Dark mode consistent across all pages

---

## Phase 5 — Review

### 5.1 Accessibility Review — WCAG 2.1 AA

**Perceivable:**
- [ ] All images have meaningful `alt` text
- [ ] Color contrast ≥ 4.5:1 for normal text, ≥ 3:1 for large text
- [ ] Color is NOT the only indicator of state

**Operable:**
- [ ] All interactive elements keyboard accessible
- [ ] Focus indicator visible (never `outline: none` without replacement)
- [ ] Touch targets ≥ 44×44px on mobile

**Understandable:**
- [ ] Form fields have visible labels (not just placeholders)
- [ ] Error messages are specific and helpful
- [ ] Navigation is consistent across pages

**Robust:**
- [ ] Semantic HTML used
- [ ] ARIA labels on custom widgets
- [ ] ARIA roles/states correct

### 5.2 UX Guidelines Review

```bash
python3 src/ui-ux-pro-max/scripts/search.py "navigation" --domain ux
python3 src/ui-ux-pro-max/scripts/search.py "forms" --domain ux
python3 src/ui-ux-pro-max/scripts/search.py "loading states" --domain ux
```

### 5.3 Performance Review

| Area | Target |
|------|--------|
| First Contentful Paint | < 1.8s |
| Largest Contentful Paint | < 2.5s |
| Cumulative Layout Shift | < 0.1 |
| Bundle size | < 200KB JS, < 50KB CSS |

### 5.4 Mobile Review

- [ ] Touch targets ≥ 44×44px
- [ ] No horizontal scroll on any breakpoint
- [ ] Text readable without zooming (≥ 16px body)
- [ ] Forms usable on mobile (proper input types, auto-complete)
- [ ] Navigation adapted for mobile

### 5.5 Output

A review report categorized by:
- **Must Fix** — accessibility violations, broken layouts, anti-patterns
- **Should Fix** — UX guideline violations, performance issues
- **Nice to Have** — polish items, minor improvements

---

## Phase 6 — Deliver (Design)

### 6.1 Deliverables

1. **Working UI** — Running application with design system applied
2. **Design System docs** (`DESIGN-SYSTEM.md`) — Tokens, components, usage rules
3. **Design Brief** — Research findings and decisions
4. **Review Report** — Accessibility and UX audit results

### 6.2 Completion Summary Format

```markdown
## ✅ Design Complete

**Project:** <name>
**Stack:** <tech stack>
**Style:** <primary style>

### What Was Built
- <page/component list>
- Design system with N tokens
- Dark mode support

### Design Decisions
- **Style / Palette / Typography:** <why>

### Quality
- Accessibility: WCAG 2.1 AA ✅
- Responsive: 375px → 1536px ✅
- Dark mode: ✅
```

### 6.3 Next Steps to Offer

1. Iterate — colors, typography, layout adjustments
2. Extend — more pages or components
3. Continue to engineering Phase 7 — build the full app with TDD

---

## Phase 7 — Brainstorm & Requirements

### 7.1 Purpose

Transform a vague idea or design into a clear, actionable specification for the engineering build phase.

### 7.2 Process

**Step A — Understand the Problem**
- What problem are you solving? (the "why")
- Who is the user/audience? (the "who")
- What does success look like? (the "what")

Do NOT propose solutions yet. Listen first.

**Step B — Explore the Solution Space**

Propose 2-3 distinct approaches with trade-offs. For each:
- **How it works** (1-2 sentences)
- **Pros / Cons**
- **Effort estimate** (S/M/L)

**Step C — Nail Down the Spec**

- **Functional requirements** — numbered list
- **Non-functional requirements** — performance, security, UX constraints
- **Out of scope** — what we explicitly will NOT build
- **Open questions** — things to resolve during implementation

**Step D — Confirm**

Present requirements summary, get explicit user approval before moving on.

### 7.3 Output

A `spec.md` file (or verbal for small tasks < 30 min). For tasks that come from Phase 1-6, the Design Brief serves as the spec foundation.

### 7.4 Quality Criteria

- [ ] Problem statement is clear and specific
- [ ] At least 2 approaches were considered
- [ ] Functional requirements are numbered and testable
- [ ] Out-of-scope items are documented
- [ ] User explicitly approved the spec

---

## Phase 8 — Write Implementation Plan

### 8.1 Purpose

Convert the spec into a step-by-step implementation plan that any engineer could follow without additional context.

### 8.2 Plan Structure

```markdown
# Implementation Plan: <Feature Name>

## Context
<1-2 paragraphs: what we're building and why>

## Architecture
<How the pieces fit together. Reference existing code/files.>

## Tasks

### Task 1: <Name>
**Files:** `path/to/file.ts`
**Tests:** `path/to/test.test.ts`
**Description:** <What to do, step by step>
**Acceptance criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

## Task Dependencies
## Risks & Mitigations
```

### 8.3 Plan Writing Rules

1. **Assume zero context** — reference specific file paths, function names, existing patterns
2. **Every task must be independently testable** — if you can't write a test, the task is too vague
3. **Specify files for each task** — create, modify, or delete
4. **Include test file paths** — each task specifies where its tests go
5. **Order tasks by dependency** — independent tasks first
6. **Keep tasks focused** — each task should take 15-60 minutes

### 8.4 Quality Criteria

- [ ] Every requirement from the spec maps to at least one task
- [ ] Every task has files, tests, and acceptance criteria
- [ ] Task order respects dependencies
- [ ] Risks section addresses the hardest parts
- [ ] User approved the plan before execution

---

## Phase 9 — Execute with TDD

### 9.1 The TDD Iron Law

```
NEVER write implementation code without a failing test that demands it.
```

The cycle for every task: **Red → Green → Refactor**, no exceptions.

1. **RED** — Write a test. Run it. It MUST fail.
2. **GREEN** — Write the minimum code to make it pass.
3. **REFACTOR** — Clean up. Tests must still pass.

### 9.2 Task Execution Protocol

```
1. Read task description and acceptance criteria
2. Examine existing code referenced by the task
3. Write test(s) for the first acceptance criterion
4. Run tests → confirm RED
5. Write minimal implementation → confirm GREEN
6. Refactor if needed → confirm still GREEN
7. Repeat for remaining acceptance criteria
8. Mark task complete in plan.md
9. Git commit: "Task N: <description>"
```

### 9.3 When TDD Feels Impossible

| Situation | Strategy |
|-----------|----------|
| UI/visual changes | Test the logic/state, manually verify visual |
| External API calls | Mock the API boundary, test the handling logic |
| Database queries | Use test fixtures or in-memory DB |
| One-line fixes | Still write a regression test |

### 9.4 Commit Discipline

- Commit after each task (not at the end)
- Format: `Task N: <short description>`
- Never commit failing tests
- One task per commit

### 9.5 Quality Criteria

- [ ] Every task has at least one test written before its implementation
- [ ] All tests pass at the end of each task
- [ ] No implementation code exists without a corresponding test
- [ ] Commits are per-task

---

## Phase 10 — Code Review

### 10.1 Review Process

**Step A — Plan Compliance**: All acceptance criteria met, tests exist and pass, no unplanned changes

**Step B — Code Quality**

| Area | What to Check |
|------|---------------|
| **Correctness** | Does it do what the plan says? Edge cases handled? |
| **Readability** | Clear naming, reasonable function length |
| **Architecture** | Separation of concerns, clean interfaces |
| **Error handling** | Failures handled gracefully |
| **Testing** | Tests are meaningful, edge cases covered |
| **Performance** | No N+1, no unnecessary computation |
| **Security** | Input validation, no secrets in code, proper auth |

**Step C — Issue Classification**

| Severity | Action |
|----------|--------|
| **Critical** | Must fix before proceeding |
| **Important** | Should fix, present to user |
| **Minor** | Note but don't block |

### 10.2 Output

Review summary in chat: tasks reviewed, issues by severity, actions taken, remaining recommendations.

---

## Phase 11 — Verification

### 11.1 Verification Protocol

**Test Suite**
```bash
<project test command>  # e.g. npm test, pytest
# Confirm: ALL tests pass, zero failures
```

**Plan Checklist**: Each task marked complete, acceptance criteria verified, git commit exists

**Integration Sanity**: Start the app, exercise the happy path, try one error case

### 11.2 Completion Report

```markdown
## ✅ Implementation Complete

**Spec:** <brief description>
**Tasks:** N/N completed
**Tests:** X passing, 0 failing
**Commits:** N commits

### What was built
### What to try
### Known limitations
```

---

## Phase 12 — Handoff & Continuation

### 12.1 Handoff Checklist

- [ ] Completion report delivered
- [ ] `plan.md` updated with all tasks marked done
- [ ] Code committed, no uncommitted changes
- [ ] Deploy live preview (Vercel/Netlify or `start_web_service`)
- [ ] Deliver: live URL + design system summary + next steps

### 12.2 Next Steps to Offer

1. **Iterate** — Adjust design, add animations, polish UX
2. **Extend** — Add more pages or features (restart from Phase 7)
3. **Deploy** — Vercel/Netlify production deployment
4. **Review together** — Walk through the codebase
5. **Done** — Project delivered ✅

---

## Technical Reference

### Design Tool Quick Reference

```bash
# Style / color / typography / product / ux / landing / google-fonts search
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain <domain>

# Full design system generation
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --design-system -p "<Project>"

# Stack-specific snippets (web stacks)
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack <react|tailwind|vue|svelte|shadcn|html|nextjs>
```

### Database Files

All CSV data lives in `src/ui-ux-pro-max/data/`:

| File | Records |
|------|---------|
| `styles.csv` | 67+ |
| `colors.csv` | 161 |
| `typography.csv` | 57 |
| `products.csv` | 161 |
| `ux-guidelines.csv` | 99 |
| `charts.csv` | 25 |
| `landing.csv` | 35 |
| `google-fonts.csv` | 1900+ |

### Engineering Workflow Decision Tree

```
User request arrives
  │
  ├─ Vague idea? ──────────── Phase 7 (Brainstorm)
  ├─ Clear requirements? ──── Phase 8 (Plan)
  ├─ Plan exists? ──────────── Phase 9 (Execute)
  ├─ Code needs review? ───── Phase 10 (Review)
  ├─ Bug fix / small tweak? ─ Phase 9 (lightweight TDD)
  └─ Quick question? ──────── Answer directly
```

### Scaling Rules

| Task Size | Brainstorm | Plan | TDD | Review | Verify |
|-----------|-----------|------|-----|--------|--------|
| Tiny (< 5 min) | Skip | Skip | Regression test only | Skip | Run tests |
| Small (5-30 min) | Verbal | Mental | Full TDD | Self-review | Run tests |
| Medium (30min–2hr) | Brief | plan.md | Full TDD | Full review | Full verification |
| Large (> 2 hr) | Full spec.md | Detailed plan.md | Full TDD + per-task commits | Full review | Full verification + integration |

### Git Workflow

```bash
git checkout -b feature/<feature-name>
# After each task:
git add -A && git commit -m "Task N: <description>"
# After all complete:
git push origin feature/<feature-name>
```

### Debugging Protocol

1. **Stop and observe** — Read the error completely. Don't guess.
2. **Form a hypothesis** — "I think the failure is because X."
3. **Find evidence** — Add logging, read the code path. Don't change code yet.
4. **Confirm or revise** — If evidence doesn't fit, form a new hypothesis.
5. **Fix the root cause** — Not the symptom.
6. **Write a regression test** — Prove the bug is fixed.
7. **Check for siblings** — Does the same pattern exist elsewhere?

### Quality Maxims

**Design:**
1. "Search, don't guess." — Use the database before making design decisions.
2. "Tokens, not values." — Never hardcode a color, spacing, or font.
3. "Bold beats safe." — A distinctive design beats a generic "clean modern" one.
4. "No AI slop." — If it looks like every other AI UI, start over.

**Engineering:**
1. "If you can't test it, you don't understand it." — Go back to requirements.
2. "Make it work, make it right, make it fast." — In that order.
3. "The plan is not the territory." — If reality diverges, update the plan.
4. "Every bug is a missing test." — Write the test first, then fix.
5. "Done means verified." — Not "I think it works" but "tests prove it works."
