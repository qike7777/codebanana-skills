---
name: user-research
description: |
  Plan and execute user research projects. Produces interview guides, usability test scripts,
  survey templates, affinity maps, persona documents, journey maps, and a structured insights report.
  Based on saeed-vayghan/gemini-agent-skills ux-researcher skill, adapted for CodeBanana.
  Triggers:
  - "user research"
  - "ux research"
  - "user interviews"
  - "usability testing"
  - "persona"
  - "user insights"
  - "用户调研"
  - "用户访谈"
  - "用户洞察"
---

# User Research Skill

## Purpose

Help users plan and execute structured user research — from defining research questions through
to actionable insights. Outputs research artifacts (interview guide, survey, personas, journey map)
and a final Markdown insights report + Excel summary.

---

## Defaults

| Parameter | Default |
|-----------|---------|
| Research method | User interviews (qualitative) |
| Participants | 5–8 |
| Output: Markdown report | Yes |
| Output: Excel summary | Yes |
| Output: Research artifacts | Yes (interview guide / survey / personas) |

### Completion condition
User confirms the research topic and product/context. All other fields optional.

---

## Phase 1 — Requirements Gathering

### 1.1 Required fields

| Field | Description | Example |
|-------|-------------|---------|
| `topic` | What you want to learn | "Why users churn after trial", "How people manage expenses" |
| `product` | The product or context | "A B2B SaaS invoicing tool", "a mobile fitness app" |

### 1.2 Optional fields

| Field | Description | Example |
|-------|-------------|---------|
| `method` | Research method | interviews / survey / usability test / diary study |
| `target_users` | Who to research | "SMB founders", "college students", "first-time homebuyers" |
| `goal` | Business goal behind research | "Improve onboarding", "validate new feature", "understand drop-off" |
| `existing_data` | Any data you already have | "We have 200 survey responses", "NPS is 32" |

### 1.3 Gathering conversation

Ask these 3 questions in the welcome message:
1. What do you want to learn? (What's the core research question?)
2. What product or context is this for?
3. Do you have a preferred method? (Interviews, survey, usability test — or not sure?)

---

## Phase 2 — Research Design

### 2.1 Define Research Questions

Convert the user's goal into 3–5 focused research questions:
- Start with "How", "Why", "What" — never yes/no
- Each maps to a specific area of inquiry
- Prioritize by importance

Example format:
```
RQ1: Why do users stop using the product after the first week?
RQ2: What alternatives are they using instead?
RQ3: How do they describe their ideal solution?
```

### 2.2 Choose Research Method

| Method | Best for | Participants | Time |
|--------|----------|--------------|------|
| User Interviews | Deep "why" questions, mental models | 5–8 | 45–60 min each |
| Usability Testing | Task completion, UI friction | 5–8 | 30–45 min each |
| Survey | Quantitative patterns, validation | 50–200+ | 5–10 min |
| Diary Study | Longitudinal behavior, habits | 8–15 | 1–2 weeks |
| Card Sorting | IA, navigation, categorization | 15–20 | 20–30 min |

Recommend method based on user's goal. Default to interviews for open-ended discovery.

### 2.3 Participant Recruitment Criteria

Define:
- **Demographics** (age range, occupation, geography)
- **Behavioral criteria** (must have used X, must be Y type of user)
- **Exclusion criteria** (no internal employees, no competitors)
- **Screener questions** (3–5 questions to qualify participants)

---

## Phase 3 — Research Artifacts

Generate the appropriate artifacts based on chosen method.

### 3.1 Interview Guide (for user interviews)

```markdown
# Interview Guide: [Topic]
**Duration:** 45–60 minutes
**Interviewer note:** This is a guide, not a script. Follow the conversation naturally.

## Introduction (5 min)
- Thank participant, explain purpose (learning, not testing them)
- Ask permission to record
- "There are no right or wrong answers — we want to understand your experience."

## Warm-up (5 min)
- Tell me a bit about yourself and what you do day-to-day.
- How long have you been using [product/category]?

## Core Questions (30–35 min)
[Generated based on research questions — 8–12 open-ended questions]

## Probing Techniques
- "Tell me more about that."
- "Can you walk me through what happened?"
- "What were you thinking at that moment?"
- "How did that make you feel?"
- "What would ideal look like?"

## Wrap-up (5 min)
- Is there anything else you'd want us to know?
- Can we follow up if we have more questions?
```

### 3.2 Usability Test Script (for usability testing)

```markdown
# Usability Test Script: [Product / Feature]
**Duration:** 30–45 minutes

## Introduction
- Explain think-aloud protocol: "Say everything you're thinking as you go."
- "We're testing the product, not you."

## Tasks
[3–5 realistic tasks based on user goals]
Task 1: "Imagine you want to [goal]. Please show me how you'd do that."
Task 2: ...

## Observation Notes
| Task | Completed? | Time | Errors | Comments |
|------|------------|------|--------|----------|

## Post-task Questions
- What was confusing or unexpected?
- What would you change?
- How does this compare to how you do it today?
```

### 3.3 Survey Template (for surveys)

Structure:
- **Section 1:** Screener / demographics (3–5 questions)
- **Section 2:** Current behavior (5–8 questions)
- **Section 3:** Pain points & needs (5–8 questions)
- **Section 4:** Reactions to concept / solution (optional, 3–5 questions)
- **Section 5:** Open-ended (1–2 questions)

Question types: Likert scale / multiple choice / ranking / open text
Keep total under 15 questions.

---

## Phase 4 — Analysis Frameworks

### 4.1 Affinity Mapping

Group raw observations/quotes into themes:

```
Level 1 — Raw notes (direct quotes, observations)
Level 2 — Patterns (cluster of similar notes)
Level 3 — Themes (named insight category)
Level 4 — Insight (actionable "How might we..." statement)
```

Output format:
```markdown
## Theme: [Name]
**Insight:** Users struggle with X because Y.
**Supporting quotes:**
- "..." (P3)
- "..." (P7)
**Frequency:** 6/8 participants
**Severity:** High / Medium / Low
```

### 4.2 Persona Template

```markdown
## Persona: [Name], [Age], [Role]
**Quote:** "..."
**Goals:** What they're trying to achieve
**Frustrations:** Top 3 pain points
**Behaviors:** How they currently solve the problem
**Tools they use:** ...
**Decision factors:** What makes them choose a solution
```

### 4.3 Journey Map

Stages: Awareness → Consideration → Onboarding → Regular Use → Advocacy / Churn

For each stage:
- **Actions** (what they do)
- **Thoughts** (what they're thinking)
- **Feelings** (emotional tone: 😊/😐/😤)
- **Pain points**
- **Opportunities**

---

## Phase 5 — Insights Report (Markdown)

### Report structure

```markdown
# User Research Report: [Topic]
*Method: [Method] | Participants: [N] | Date: [Date]*

## Executive Summary
3–5 bullet points. Most important insights first.

## Research Questions
[List RQs and one-line answers]

## Key Insights
### Insight 1: [Title]
Description. Supporting evidence. Frequency.

### Insight 2: [Title]
...

## Personas
[1–3 persona profiles]

## Journey Map
[Table or narrative]

## Recommendations
| Recommendation | Priority | Rationale |
|----------------|----------|-----------|

## Methodology
- Method, participants, recruitment criteria
- Limitations and caveats
- Areas for follow-up research
```

---

## Phase 6 — Excel Summary

Use `openpyxl` to generate `user-research-[topic]-[date].xlsx`

### Sheet 1: Insights Summary
| # | Insight | Theme | Frequency | Severity | Recommendation |
|---|---------|-------|-----------|----------|----------------|

### Sheet 2: Raw Observations
| Participant | Quote / Observation | Theme | Research Question |
|-------------|---------------------|-------|-------------------|

### Sheet 3: Personas
| Name | Age/Role | Goals | Frustrations | Behaviors |
|------|----------|-------|--------------|-----------|

### Sheet 4: Recommendations
| Recommendation | Priority | Effort | Impact | Owner |
|----------------|----------|--------|--------|-------|

**Formatting:**
- Dark header: `#1F3864` (white text)
- Alternate rows: `#F2F7FF`
- Freeze top row, auto-fit columns

---

## Phase 7 — Delivery

1. Save report as `user-research-[topic]-[date].md`
2. Save Excel as `user-research-[topic]-[date].xlsx`
3. Deliver summary:

```
✅ User Research Complete: [Topic]

📄 Report: user-research-[topic]-[date].md
📊 Excel: user-research-[topic]-[date].xlsx

**Top 3 Insights:**
1. [Insight 1]
2. [Insight 2]
3. [Insight 3]

**Top Recommendations:**
1. [Rec 1] — Priority: High
2. [Rec 2] — Priority: Medium

**Next steps:**
- [ ] Share findings with product team
- [ ] Prioritize top recommendations in roadmap
- [ ] Schedule follow-up research on [open question]
```

---

## Research Principles

- **Separate observation from interpretation** — quote directly, label inferences
- **Look for patterns, not outliers** — n=1 is a data point, not an insight
- **Stay curious, not confirmatory** — seek to understand, not validate assumptions
- **Empathy first** — users aren't wrong, the product is
- **Always cite frequency** — "6/8 participants said..." not "users feel..."
