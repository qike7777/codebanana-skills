---
name: product-manager
description: |
  Write PRDs, user stories, feature specs, roadmaps, competitive analyses, and OKRs.
  Use when the user needs product documentation, requirements, or strategy artifacts.
  Triggers:
  - "PRD"
  - "product requirements"
  - "user story"
  - "user stories"
  - "feature spec"
  - "roadmap"
  - "product roadmap"
  - "competitive analysis"
  - "OKRs"
  - "acceptance criteria"
  - "product manager"
  - "产品需求"
  - "需求文档"
  - "用户故事"
  - "产品路线图"
  - "竞品分析"
---

# Product Manager — SKILL.md

> Turn product ideas into clear, executable requirements that engineering and stakeholders can act on.

---

## Overview

You produce PM artifacts in this priority order:
1. Understand the business context and user problem
2. Choose the right document type
3. Fill templates with specifics — no padded fluff
4. Deliver scannable, opinionated documents

**Output philosophy**: Every document should be usable the day it's delivered. No placeholders, no "[insert here]", no generic descriptions.

---

## Document Type Selection

| User Need | Document to Produce |
|-----------|---------------------|
| Define a new feature or product | PRD |
| Break down a feature for a sprint | User Stories |
| Align on priorities over time | Product Roadmap |
| Understand the competitive landscape | Competitive Analysis |
| Measure team/product performance | OKRs |
| Short exec update on a feature | Feature Brief (1-pager) |

---

## Phase 1 — Context Gathering

Before writing anything, collect:

**For PRDs:**
- Problem statement: What user pain is being solved?
- Target user: Who specifically? (persona or segment)
- Business goal: What metric does this move?
- Scope: What's in / out of scope for this version?
- Success metrics: How do we know it worked?
- Timeline / constraints: Any deadlines, tech constraints, dependencies?

**For User Stories:**
- The epic or feature context
- The user persona performing the action
- What they're trying to accomplish
- Any known edge cases or edge users

**For Roadmaps:**
- Timeframe (quarter, half-year, annual)
- Top strategic themes or bets
- Known committed work vs. exploratory work
- Audiences (public, internal, engineering-only)

---

## Phase 2 — PRD Template

```markdown
# PRD: [Feature Name]

**Status**: Draft / In Review / Approved  
**Author**: [Name]  
**Last Updated**: [Date]  
**Stakeholders**: [Engineering Lead, Design, Data, Legal if needed]

---

## 1. Problem Statement

[2-3 sentences. What user pain exists? What happens if we don't solve it?]

## 2. Goal & Success Metrics

**Business goal**: [One sentence — what outcome are we driving?]

| Metric | Baseline | Target | Timeframe |
|--------|----------|--------|-----------|
| [e.g. activation rate] | X% | Y% | 90 days post-launch |

## 3. Target User

**Persona**: [Name + description — e.g., "Power User: a team admin managing 20+ seats"]  
**User journey stage**: [Onboarding / Activation / Retention / Expansion]

## 4. Proposed Solution

[What are we building? 1-2 paragraphs. Focus on the user experience, not technical implementation.]

### User Flow
1. User does X
2. System responds with Y
3. User completes Z

## 5. Requirements

### Must Have (P0)
- [ ] [Requirement 1]
- [ ] [Requirement 2]

### Should Have (P1)
- [ ] [Requirement 3]

### Won't Have (This Version)
- [Explicitly list things we're deferring and why]

## 6. Websites & Apps Notes

[Link to Figma or describe key interactions. Note any accessibility requirements.]

## 7. Technical Considerations

[Known constraints, dependencies, integrations, data model changes. Written for engineers.]

## 8. Open Questions

| Question | Owner | Due |
|----------|-------|-----|
| [Question] | [Name] | [Date] |

## 9. Out of Scope

[Explicitly list what this PRD does NOT cover]

## 10. Appendix

[Research, data, competitive context, user feedback that informed this PRD]
```

---

## Phase 3 — User Story Templates

### Standard Story Format

```
As a [user type],
I want to [action/goal],
So that [benefit/value].

**Acceptance Criteria:**
- [ ] Given [context], when [action], then [outcome]
- [ ] Given [context], when [edge case], then [outcome]
- [ ] [Non-functional: performance, accessibility, security]

**Definition of Done:**
- [ ] Unit tests written
- [ ] Design reviewed
- [ ] Analytics event added
- [ ] Documentation updated
```

### Story Sizing Guide

| Points | Complexity | Typical scope |
|--------|-----------|---------------|
| 1 | Trivial | Text change, config flag |
| 2 | Small | Single UI component, simple API endpoint |
| 3 | Medium | New feature with BE + FE |
| 5 | Large | Multi-component feature, new data model |
| 8 | X-Large | Should be broken down further |

### Epic Structure

```markdown
# Epic: [Name]

**Goal**: [One sentence]  
**Business value**: [Why this epic matters]  
**Success metric**: [How we measure the epic's success]

## Stories
1. [Story title] — [X points]
2. [Story title] — [X points]
3. [Story title] — [X points]

**Total estimate**: [X points / Y sprints]
```

---

## Phase 4 — Product Roadmap Templates

### Quarterly Roadmap (Themes-based)

```markdown
# Product Roadmap — Q[X] [Year]

**Vision**: [One sentence on where we're headed]  
**Themes this quarter**: [Theme 1], [Theme 2], [Theme 3]

---

## Theme 1: [Name]
**Why**: [Strategic rationale — 1 sentence]

| Initiative | Status | Owner | Notes |
|-----------|--------|-------|-------|
| [Feature A] | 🟢 Committed | [Team] | Ships week 4 |
| [Feature B] | 🟡 Likely | [Team] | Depends on design |
| [Feature C] | ⚪ Stretch | [Team] | Only if Theme 2 ships early |

## Theme 2: [Name]
...

---

## Not This Quarter (Parking Lot)
- [Item]: [Why deferred]
- [Item]: [Why deferred]
```

### Now / Next / Later Format

```markdown
# Roadmap: Now / Next / Later

## NOW (Current Quarter)
- [Feature] — [Owner] — [Target date]
- [Feature] — [Owner] — [Target date]

## NEXT (Next Quarter)
- [Feature] — [Hypothesis / why we think it matters]
- [Feature] — [Dependencies]

## LATER (6+ months)
- [Feature] — [Strategic bet]
- [Feature] — [Requires X to be true first]
```

---

## Phase 5 — Competitive Analysis Template

```markdown
# Competitive Analysis: [Category/Problem Space]

**Date**: [Date]  
**Purpose**: [Why we're doing this analysis]

## Competitive Landscape

| Dimension | Us | [Competitor A] | [Competitor B] | [Competitor C] |
|-----------|-----|---------------|---------------|---------------|
| Target user | | | | |
| Core value prop | | | | |
| Pricing | | | | |
| [Key feature 1] | | | | |
| [Key feature 2] | | | | |
| Market position | | | | |

## Deep Dive: [Competitor A]

**Strengths**:
- 
- 

**Weaknesses**:
- 
- 

**What they do better than us**:
-

**Our defensible advantage vs. them**:
-

## Key Takeaways

1. [Insight 1]
2. [Insight 2]
3. [Implication for our roadmap]
```

---

## Phase 6 — OKRs Template

```markdown
# OKRs — [Team/Product] — [Quarter/Year]

## Objective 1: [Ambitious, qualitative goal]

*Why it matters*: [1 sentence]

| Key Result | Baseline | Target | Current |
|-----------|---------|--------|---------|
| KR1: [Measurable outcome] | X | Y | — |
| KR2: [Measurable outcome] | X | Y | — |
| KR3: [Measurable outcome] | X | Y | — |

## Objective 2: [Next objective]
...
```

**OKR quality checklist:**
- [ ] Objective is inspiring and qualitative (not a metric)
- [ ] Each KR is measurable and has a baseline + target
- [ ] KRs are outcomes, not outputs ("revenue grows 20%" not "ship feature X")
- [ ] 3-5 KRs per objective max
- [ ] KRs are stretch goals (~70% achievement = success)

---

## Quality Standards

**Every PM document must have:**
- A clear problem statement before the solution
- Explicit success metrics (not "improve user experience")
- An "out of scope" section (scope creep starts with omission)
- Named owners for open questions

**Signs a document needs revision:**
- Uses vague language: "improved", "better", "enhanced" with no metric
- No user persona — "users" is not a persona
- Requirements are really implementation details
- No definition of success

---

## Writing Style Guide

- **Active voice**: "User clicks X" not "X is clicked by user"
- **Specific**: "Reduce time-to-first-value from 4 days to 1 day" not "improve onboarding"
- **Scannable**: Use tables, bullet lists, headers — not paragraphs of prose
- **Annotate assumptions**: Use `[ASSUMPTION]` tag inline when something is uncertain
- **Flag blockers**: Use `[BLOCKER]` tag for unresolved dependencies

---

## HTML Delivery

**After completing the PRD (Phase 4), automatically generate the HTML document.** Do not wait for the user to ask. Use `prd-template.html` (in this `skill/` directory), replace all `{{VARIABLE}}` placeholders, and deliver the `.html` file in the same response.

**Design:** F-Pattern layout + minimal — white background, sticky left sidebar navigation, Inter font, priority badges (P0/P1/P2), OKR progress bars, user story cards with acceptance criteria.

### Variable Reference

| Variable | Description |
|----------|-------------|
| `{{PRD_TITLE}}` | Feature / product title |
| `{{PRD_DESC}}` | One-paragraph description |
| `{{OWNER}}` | PM name |
| `{{VERSION}}` | e.g. 1.0 |
| `{{STATUS}}` | Draft / In Review / Final |
| `{{STATUS_CLASS}}` | tb-draft / tb-review / tb-final |
| `{{TARGET_DATE}}` | Target launch date |
| `{{PROBLEM_DESC}}` | Problem context paragraph |
| `{{CORE_PROBLEM}}` | One-sentence core problem |
| `{{WHY_NOW}}` | Why this must be solved now |
| `{{GOALS_DESC}}` | Goals intro |
| `{{OBJ1}}` / `{{OBJ2}}` | Objective statements |
| `{{KR1_1}}` – `{{KR1_3}}` | Key results for Obj 1 |
| `{{KR1_1_PCT}}` – `{{KR1_3_PCT}}` | Progress % (0–100) |
| `{{KR2_1}}` – `{{KR2_2}}` | Key results for Obj 2 |
| `{{STORIES_INTRO}}` | User stories intro |
| `{{US1_ID}}` – `{{US4_ID}}` | Story IDs (e.g. US-001) |
| `{{US1_TEXT}}` – `{{US4_TEXT}}` | "As a [user], I want…" |
| `{{US1_AC1}}` – `{{US1_AC3}}` | Acceptance criteria |
| `{{US1_PRIORITY}}` – `{{US4_PRIORITY}}` | P0 / P1 / P2 |
| `{{FR1_ID}}` – `{{FR6_ID}}` | Functional req IDs |
| `{{FR1_TEXT}}` – `{{FR6_TEXT}}` | Requirement descriptions |
| `{{FR1_NOTE}}` – `{{FR6_NOTE}}` | Notes |
| `{{NFR_PERF}}` / `{{NFR_SEC}}` / `{{NFR_SCALE}}` | Non-functional req details |
| `{{OOS_INTRO}}` | Out of scope intro |
| `{{OOS_1}}` – `{{OOS_4}}` | Out of scope items |
| `{{M1_NAME}}` – `{{M4_NAME}}` | Milestone names |
| `{{M1_DATE}}` – `{{M4_DATE}}` | Milestone dates |
| `{{M1_OWNER}}` – `{{M4_OWNER}}` | Milestone owners |
| `{{M1_DELIVER}}` – `{{M4_DELIVER}}` | Milestone deliverables |
| `{{RISK1}}` – `{{RISK3}}` | Risk descriptions |
| `{{RISK1_LIKE}}` – | Likelihood labels |
| `{{RISK1_MIT}}` – `{{RISK3_MIT}}` | Mitigations |
| `{{OQ1}}` – `{{OQ3}}` | Open questions |
| `{{OQ1_OWNER}}` – | Question owners |
| `{{OQ1_DUE}}` – | Due dates |

### Delivery
1. Read `prd-template.html`
2. Replace all `{{VARIABLE}}` placeholders
3. Save as `prd-[feature-name].html`
4. Deliver as standalone `.html` file

---

## Phase 0 — Market Research (Auto)

Before gathering requirements from the user, automatically run background research. Do not ask for permission.

**Trigger:** Once the user names the product or feature to document.

**Research steps:**

1. **Similar products / competitors** — search `"[feature/product name] product" OR "[category] SaaS tool"` to find 3–5 comparable products
2. **Existing user pain points** — search `"[category] problems" OR "[category] user complaints site:reddit.com"` for real user language
3. **Industry benchmarks** — search `"[category] best practices 2025"` for standard feature expectations
4. **Regulatory / compliance signals** — if fintech/health/legal, search for relevant compliance requirements

**Use findings to:**
- Pre-fill "Background & Context" section with market framing
- Suggest more precise problem statements based on real user language
- Add realistic success metrics benchmarked to industry standards
- Flag any compliance requirements the PRD should address

**Report to user in Phase 1:**
> "I found X similar products and Y relevant user pain points. I've pre-filled the background section — does this framing match your intent?"
