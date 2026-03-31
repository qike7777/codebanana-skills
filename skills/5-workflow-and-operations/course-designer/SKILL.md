---
name: course-designer
description: |
  Design complete online courses, workshops, and training programs. Produces course outlines,
  lesson plans, learning objectives, exercises, quizzes, and learner guides.
  Use when the user wants to build educational content or training materials.
  Triggers:
  - "course design"
  - "course outline"
  - "curriculum"
  - "e-learning"
  - "training program"
  - "lesson plan"
  - "workshop design"
  - "learning objectives"
  - "course content"
  - "quiz"
  - "assessment"
  - "curriculum design"
  - "课程设计"
  - "课程大纲"
  - "教学计划"
  - "培训材料"
  - "网课"
---

# Course Designer — SKILL.md

> Design courses people actually complete — structured for real learning, not just content coverage.

---

## Overview

Good course design starts with outcomes, not content. The sequence is always:
**WHY** (learning outcomes) → **WHAT** (content structure) → **HOW** (delivery + activities)

---

## Phase 1 — Define Learning Outcomes

Before designing any content, answer:

1. **Who is the learner?** (Role, experience level, prior knowledge, motivation)
2. **What problem does this course solve?** (Skill gap, knowledge gap, behavior change)
3. **What will learners be able to DO after completing?** (Not "understand" — DO)
4. **Format**: Self-paced / Live / Hybrid? Synchronous / Asynchronous?
5. **Constraints**: Time budget? Platform? Existing content to build from?

### Learning Objective Formula (Bloom's Taxonomy)

Write objectives as: **"By the end of [unit], learners will be able to [verb] [object] [condition/standard]"**

Use action verbs matched to the cognitive level:

| Level | Action Verbs |
|-------|-------------|
| Remember | define, list, recall, identify, name |
| Understand | explain, describe, summarize, classify |
| Apply | use, demonstrate, implement, solve, execute |
| Analyze | differentiate, compare, examine, break down |
| Evaluate | assess, critique, justify, recommend |
| Create | design, build, compose, develop, produce |

**Examples:**
- ✅ "Learners will be able to write a SQL JOIN query to combine two related tables"
- ❌ "Learners will understand SQL joins"
- ✅ "Learners will be able to diagnose a slow web page and identify 3 performance bottlenecks"
- ❌ "Learners will learn about web performance"

---

## Phase 2 — Course Architecture

### Course Structure Template

```markdown
# Course: [Title]

**Target learner**: [Persona — who, at what level]
**Duration**: [Total estimated time]
**Format**: [Self-paced / Live / Workshop]
**Prerequisites**: [What learners need to know first]

## Course Outcome
By the end of this course, learners will be able to [MASTER OUTCOME].

---

## Module 1: [Title] — [X min]
**Module outcome**: Learners will be able to [OUTCOME]

### Lesson 1.1: [Title] — [X min]
- Learning objective: [Bloom's-level objective]
- Content type: [Video / Reading / Exercise / Discussion]
- Key concepts: [3-5 bullet points]

### Lesson 1.2: [Title] — [X min]
...

### Module 1 Check: [Quiz / Exercise / Project]
- [Description of assessment]

---

## Module 2: [Title] — [X min]
...

---

## Final Project / Assessment
[Description of capstone or final evaluation]
```

### Pacing Guidelines

| Course Length | Modules | Lessons per Module | Lesson Length |
|--------------|---------|-------------------|---------------|
| 1-hour mini-course | 3-4 | 2-3 | 5-10 min |
| Half-day workshop | 4-5 | 3-5 | 10-20 min |
| Full-day workshop | 6-8 | 4-6 | 15-20 min |
| 4-week online course | 4 | 4-6 | 10-15 min |
| 8-week online course | 8 | 4-6 | 10-20 min |

**Rule**: No lesson longer than 20 minutes for self-paced. Break anything longer.

---

## Phase 3 — Lesson Structure Templates

### Standard Lesson Template

```markdown
# Lesson [X.X]: [Title]

**Duration**: [X minutes]  
**Objective**: By the end of this lesson, learners will be able to [OBJECTIVE]

---

## Hook (1-2 min)
[Attention-grabbing question, story, problem, or surprising fact]
[Why does this matter to the learner?]

## Core Concept (3-5 min)
[Explanation of the main idea — clear, simple, one concept at a time]
[Use analogy or real-world example]

## Demonstration / Example (2-3 min)
[Walk through a concrete example step by step]
[Show the concept being applied]

## Learner Practice (2-5 min)
[Activity, exercise, or reflection that applies the concept]
[Specification: what the learner does, expected output]

## Summary (1 min)
[Recap the key takeaway in 1-2 sentences]
[Connect to next lesson]

---

## Notes for Instructor / Narrator
[Tips for delivery, common misunderstandings, pacing notes]
```

### Lesson Types

| Type | Best For | Structure |
|------|---------|-----------|
| Concept lesson | Introducing new ideas | Hook → Explain → Example → Check |
| How-to lesson | Teaching a skill | Objective → Demo → Practice → Feedback |
| Case study | Applying to real context | Context → Analysis → Discussion → Insight |
| Problem-solving | Building judgment | Problem → Frameworks → Worked example → Your turn |

---

## Phase 4 — Exercises & Assessments

### Exercise Design Principles

1. **Every lesson needs one active element** — no passive consumption only
2. **Vary exercise types** — not just multiple choice
3. **Immediate feedback** where possible
4. **Difficulty ladder** — start easy, escalate

### Exercise Types

| Type | When to Use | Format |
|------|-------------|--------|
| Multiple choice | Knowledge recall | 4 options, one clearly correct |
| True/False | Quick concept check | With explanation of why |
| Fill in the blank | Precise recall | Sentence with key term removed |
| Short answer | Comprehension | 2-3 sentence response |
| Reflection prompt | Metacognition | "How would you apply X to your situation?" |
| Scenario / case | Application | Present situation → ask decision |
| Build/Create task | Synthesis | Open-ended output with rubric |

### Quiz Template (Multiple Choice)

```markdown
## Quiz: [Module/Lesson Title]

**Instructions**: Select the best answer for each question.

---

**Q1**: [Question — test understanding, not trivia]

A) [Plausible but wrong]
B) [Correct answer]
C) [Plausible but wrong]
D) [Common misconception]

**Answer**: B
**Explanation**: [1-2 sentences explaining why B is correct and why others are wrong]

---

**Q2**: [Question]
...
```

### Rubric Template (for Open-Ended Exercises)

```markdown
## Assessment Rubric: [Exercise Name]

| Criterion | Excellent (4) | Good (3) | Developing (2) | Beginning (1) |
|-----------|--------------|---------|----------------|---------------|
| [Criterion 1] | [Description] | [Description] | [Description] | [Description] |
| [Criterion 2] | [Description] | [Description] | [Description] | [Description] |
| [Criterion 3] | [Description] | [Description] | [Description] | [Description] |
```

---

## Phase 5 — Workshop Facilitation Guide

```markdown
# Workshop Facilitation Guide: [Title]

**Duration**: [X hours]  
**Participants**: [Number, role, experience level]  
**Room setup**: [In-person / Virtual / Hybrid]  
**Materials needed**: [Slides, whiteboard, handouts, tools]

---

## Pre-Workshop
- [ ] Send calendar invite with pre-reading
- [ ] Prepare materials and test tech
- [ ] Prepare icebreaker activity

---

## Agenda

| Time | Activity | Type | Facilitator Notes |
|------|---------|------|------------------|
| 0:00 | Welcome + icebreaker | Interactive | [Notes] |
| 0:10 | Context + objectives | Presentation | [Key points] |
| 0:25 | [Main activity 1] | [Type] | [Instructions] |
| [X] | [Break] | — | — |
| ... | ... | ... | ... |
| [Last-15min] | Reflection + next steps | Discussion | [Prompts] |
| [Last-5min] | Close + action items | Presentation | [Follow-up] |

---

## Activity Instructions

### Activity 1: [Name]
**Purpose**: [What participants will learn or produce]  
**Time**: [X minutes]  
**Format**: [Individual / Pairs / Small groups]  
**Instructions for participants**: [Step by step]  
**Debrief questions**: [3 questions to discuss after]  
**Common issues**: [What can go wrong and how to handle it]

---

## Facilitator Tips
- If energy drops: [Quick energizer activity]
- If behind schedule: [What to cut]
- If a topic sparks long discussion: [How to gracefully redirect]
```

---

## Phase 6 — Learner Materials

### Module Worksheet Template

```markdown
# Module [X] Worksheet: [Title]

**Learner name**: ___________________  
**Date**: ___________________

---

## Key Concepts (fill in as you learn)

1. [Concept prompt]: ___________________
2. [Concept prompt]: ___________________

## Reflection Questions

1. [How does this connect to your current work?]
2. [What surprised you?]
3. [What's one thing you'll try this week?]

## Action Plan

| I will... | By when | How I'll know it worked |
|-----------|---------|------------------------|
| | | |
```

---

## Quality Standards

**A well-designed course has:**
- Clear, Bloom's-level learning objectives for every lesson
- At least one active exercise per lesson
- Lessons ≤ 20 minutes for self-paced
- A logical sequence: each lesson builds on the previous
- A final assessment tied to the course outcome

**Signs a course needs revision:**
- Objectives use "understand" or "know" — replace with action verbs
- Lessons are just slides with bullet points — add activities
- No assessment at module end
- Content order is arbitrary — check scaffolding logic

---

## Phase 7 — HTML Course Page

**After delivering the course outline (Phase 2) or full lesson plan (Phase 3–6), automatically proceed to generate the HTML course page.** Do not wait for the user to ask. Use the template at `course-page-template-default.html` (or `-dark` / `-bold`) (in this `skill/` directory), replace all `{{VARIABLE}}` placeholders with the course content just produced, and deliver the `.html` file in the same response.

### Design
**Style**: Accordion Layout (布局07) + Organic Natural (风格#9)
- Warm earth tones: Sand `#f5ede0` · Clay `#D4A373` · Sage `#a8b88a` · Brown `#5c4033`
- Floating blob background shapes, soft rounded cards (20-24px radius)
- Typography: Nunito (rounded, friendly) + Caveat (handwritten accents)
- Accordion curriculum section, pricing cards, instructor profile

### Variable Reference

| Variable | Description |
|----------|-------------|
| `{{BRAND_NAME}}` | Brand/creator name (nav logo in Caveat script) |
| `{{COURSE_NAME}}` | Course name (page `<title>`) |
| `{{COURSE_CATEGORY}}` | Category tag (e.g. "Design · Beginner") |
| `{{COURSE_TITLE_LINE1}}` / `{{COURSE_TITLE_LINE2}}` | Hero headline, line 2 in clay script |
| `{{COURSE_DESC}}` | 2–3 sentence course description |
| `{{TOTAL_HOURS}}` / `{{MODULE_COUNT}}` / `{{LEVEL}}` / `{{LANGUAGE}}` | Course meta |
| `{{INSTRUCTOR_EMOJI}}` | Avatar emoji (shown in gradient circle) |
| `{{INSTRUCTOR_NAME}}` / `{{INSTRUCTOR_TITLE}}` / `{{INSTRUCTOR_BIO}}` | Instructor info |
| `{{STUDENT_COUNT}}` / `{{RATING}}` / `{{COURSE_COUNT}}` | Instructor stats |
| `{{OUTCOMES_TITLE}}` | "What you'll learn" section title |
| `{{O1_ICON}}` – `{{O4_ICON}}` | Outcome emoji icons |
| `{{O1_TITLE}}` – `{{O4_TITLE}}` | Outcome card titles |
| `{{O1_DESC}}` – `{{O4_DESC}}` | Outcome descriptions |
| `{{CURRICULUM_TITLE}}` | Curriculum section title |
| `{{M1_TITLE}}` – `{{M3_TITLE}}` | Module names |
| `{{M1_LESSONS}}` – `{{M3_LESSONS}}` | Lesson counts |
| `{{M1_DURATION}}` – `{{M3_DURATION}}` | Module durations (e.g. "45 min") |
| `{{M1_L1}}` – `{{M3_L3}}` | Lesson titles (3 per module shown) |
| `{{M1_L1_DUR}}` – `{{M3_L3_DUR}}` | Lesson durations |
| `{{PRICING_TITLE}}` | Pricing section title |
| `{{CURRENCY}}` | Currency symbol |
| `{{PLAN1_NAME}}` / `{{PLAN2_NAME}}` | Plan names |
| `{{PLAN1_PRICE}}` / `{{PLAN2_PRICE}}` | Prices |
| `{{PLAN1_DESC}}` / `{{PLAN2_DESC}}` | Plan descriptions |
| `{{PLAN1_F1}}` – `{{PLAN2_F4}}` | Feature list items |
| `{{PLAN1_CTA}}` / `{{PLAN2_CTA}}` | Button labels |
| `{{PLAN1_URL}}` / `{{PLAN2_URL}}` | Button links |
| `{{YEAR}}` | Footer year |

### Delivery
1. Read the template file
2. Replace all `{{VARIABLE}}` with user's course content
3. Save as `course-[name].html`
4. Deliver the file — standalone, no build step required


## Theme Selection Guide

When generating the HTML output, **pick the theme that best matches the user's topic and audience**. Do not default to the same theme every time.

### Theme Options

| Theme | File Suffix | Visual Style | Best For |
|-------|-------------|--------------|----------|
| **Default** | `-default.html` | Warm sand/clay — approachable, structured, learning-focused | General education, professional training, e-learning |
| **Dark** | `-dark.html` | Cyberpunk terminal — deep black, neon cyan/green, monospace fonts, scan-line overlay | Developer tools, technical docs, automation platforms, CLI-focused content, anything "engineering/hacker" |
| **Bold** | `-bold.html` | Neo-brutalism/Memphis — cream background, thick black borders, offset shadows, Space Mono, yellow+pink accents | Creative work, marketing, social/viral content, startups, anything that needs to feel energetic and memorable |

### Decision Rules

```
User's topic / audience                  → Recommended theme
────────────────────────────────────────────────────────────
Dev tools, APIs, coding, automation      → dark
Corporate SaaS, enterprise, formal       → default
Marketing, social, brand, creative       → bold
Data, analytics, dashboards, technical   → dark  
E-learning, education, professional docs → default
Startups, indie, viral, fun content      → bold
Writing, journalism, editorial content   → default
Music, gaming, culture, subculture       → dark or bold
Personal brand, portfolio, resume        → bold (if expressive) or default (if conservative)
```

### How to Apply

1. **Detect theme from user's context** — their topic, brand tone, or explicit preference
2. **Load the matching template file** (`course-page-template-[theme].html`)
3. **Replace all `{VARIABLE}` placeholders** with generated content
4. **Deliver the file** — no build step required

If the user asks for a specific style or you're unsure, default to **default** and offer alternatives:
> "I used the clean default theme. Want to try the dark cyberpunk or bold neo-brutalism version?"
