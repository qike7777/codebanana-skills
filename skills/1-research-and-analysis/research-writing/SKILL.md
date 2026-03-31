---
name: research-writing
description: |
  Generate complete, publication-quality research documents — papers, reports, literature reviews, and scientific analyses.
  The agent guides the user through topic scoping, research, outlining, drafting, and final formatting with proper citations.
  Based on the K-Dense-AI/claude-scientific-skills collection.
  Triggers:
  - "write research paper"
  - "research report"
  - "literature review"
  - "scientific writing"
  - "写研究报告"
  - "写论文"
  - "文献综述"
  - "学术写作"
  - "research writing"
---

# SKILL.md — Research Writing

> Technical authority for generating complete, structured research documents with proper citations, data analysis, and academic formatting.

---

## Overview

This skill defines the end-to-end process for creating research documents. The agent collects a research topic/question from the user, conducts structured research, produces an outlined draft, and delivers a polished Markdown document with proper citations.

### Supported Document Types

| Type | Structure | Typical Length |
|------|-----------|---------------|
| **Research Paper** | IMRAD (Introduction, Methods, Results, Discussion) | 3,000–8,000 words |
| **Literature Review** | Thematic or chronological synthesis | 2,000–6,000 words |
| **Research Report** | Executive summary + sections + recommendations | 2,000–5,000 words |
| **Scientific Analysis** | Problem → Data → Analysis → Conclusions | 1,500–4,000 words |

### Project Structure

All output goes into a `research-output/` directory in the workspace:

```
research-output/
├── config.json              # Research project configuration
├── outline.md               # Approved outline before drafting
├── drafts/
│   └── <slug>-draft-v1.md   # Working drafts
├── final/
│   └── <slug>.md            # Final polished document
├── references/
│   └── <slug>-references.bib  # BibTeX citations (if applicable)
└── assets/
    └── (tables, figures, supplementary)
```

---

## Phase 1 — Environment Setup

### 1.1 Create Project Directory

```bash
mkdir -p research-output/drafts research-output/final research-output/references research-output/assets
```

### 1.2 Verify Tools Available

The research writing workflow uses built-in tools only — no external dependencies required:

- **File read/write**: For creating and editing Markdown documents
- **Web search**: For research, fact-checking, and citation verification
- **Terminal**: For file management operations

### 1.3 Initialize Config

Create `research-output/config.json` after requirements are collected (Phase 2):

```json
{
  "title": "",
  "document_type": "",
  "research_question": "",
  "scope": "",
  "citation_style": "APA",
  "language": "en",
  "target_length": "",
  "status": "initialized"
}
```

**Completion marker**: `research-output/` directory exists with subdirectories.

---

## Phase 2 — Collect Research Requirements

Gather the following from the user. All have sensible defaults.

### Required Fields

| Field | Description | Example |
|-------|-------------|---------|
| **Research Topic / Question** | The central question or topic to investigate | "Impact of remote work on team productivity" |
| **Document Type** | What kind of document to produce | "Research Paper" (default) |
| **Language** | Primary writing language | "en" (default) |

### Optional Fields (with defaults)

| Field | Description | Default |
|-------|-------------|---------|
| **Scope / Constraints** | Specific boundaries, timeframe, domain | Derived from topic |
| **Citation Style** | APA, AMA, Vancouver, Chicago, IEEE | APA |
| **Target Length** | Approximate word count | Based on document type (see table above) |
| **Target Audience** | Who will read this | General academic audience |
| **Key Sources** | Any specific papers/sources to include | None — agent researches independently |
| **Specific Sections** | Custom sections beyond the standard structure | Standard structure for document type |
| **Data / Evidence Requirements** | Types of evidence to prioritize | Peer-reviewed sources preferred |

### Requirements Summary Format

After collecting, present this summary for user confirmation:

```
📋 Research Project Summary
━━━━━━━━━━━━━━━━━━━━━━━━━
• Topic: [topic]
• Type: [document type]
• Language: [language]
• Citation Style: [style]
• Target Length: ~[X] words
• Audience: [audience]
• Scope: [scope details]
━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Completion marker**: User confirms the requirements summary. `config.json` written.

---

## Phase 3 — Research & Outline

This is the core intellectual phase. Two stages: research, then outline.

### 3.1 Conduct Research

Use web search to gather information on the topic. For each search:

1. **Search strategy**: Use 3-5 varied search queries to cover different angles
   - Primary topic keywords
   - Related concepts and terminology
   - Key researchers / landmark studies
   - Contrasting viewpoints
   - Recent developments (last 2-3 years)

2. **Source evaluation criteria**:
   - Prefer peer-reviewed publications
   - Check publication date (prioritize recent for fast-moving fields)
   - Verify author credentials and institutional affiliation
   - Cross-reference claims across multiple sources
   - Note sample sizes, methodologies, and limitations

3. **Research notes**: Keep structured notes internally:
   - Key findings per source
   - Direct quotes worth citing (with source info)
   - Statistical data and figures
   - Gaps in the literature
   - Conflicting findings

### 3.2 Build Outline

Create `research-output/outline.md` with this structure:

```markdown
# [Document Title] — Outline

## Document Type: [type]
## Target Length: ~[X] words

---

### 1. [Section Name]
**Purpose**: [what this section accomplishes]
**Key Points**:
- [Point 1 — with source indication]
- [Point 2 — with source indication]
- [Point 3]
**Estimated Length**: ~[X] words

### 2. [Section Name]
...

---

## Planned Citations
1. [Author (Year). Title. Source.]
2. ...

## Key Data / Evidence
- [Statistic or finding 1 — source]
- [Statistic or finding 2 — source]
```

#### Structure by Document Type

**Research Paper (IMRAD)**:
1. Abstract (~150-300 words)
2. Introduction (background, gap, research question, thesis)
3. Methods (approach, data sources, analytical framework)
4. Results (findings organized by theme or hypothesis)
5. Discussion (interpretation, implications, limitations)
6. Conclusion (summary, recommendations, future directions)
7. References

**Literature Review**:
1. Abstract (~150-250 words)
2. Introduction (scope, significance, review methodology)
3. Thematic Section 1 (grouped by theme, not chronologically)
4. Thematic Section 2
5. Thematic Section 3
6. Synthesis & Gaps (what we know, what's missing)
7. Conclusion (summary, future research directions)
8. References

**Research Report**:
1. Executive Summary (~200-400 words)
2. Introduction (context, objectives)
3. Methodology (approach, data sources)
4. Findings (organized by theme or question)
5. Analysis & Discussion
6. Recommendations
7. References
8. Appendices (if applicable)

**Scientific Analysis**:
1. Abstract (~150 words)
2. Problem Statement
3. Data & Methods
4. Analysis
5. Results
6. Discussion & Implications
7. Conclusion
8. References

**Completion marker**: User approves the outline in `research-output/outline.md`.

---

## Phase 4 — Draft Writing

### 4.1 Writing Principles

Follow these rules strictly:

1. **Full prose paragraphs** — never bullet points in the body text. Bullet points are only for lists of items (e.g., inclusion criteria, recommendations).
2. **Two-stage per section**:
   - Stage A: Write key points and evidence mapping
   - Stage B: Convert to flowing prose with transitions
3. **Academic tone**: Third person, objective, evidence-based. Avoid hedging language ("it seems", "perhaps") unless genuinely uncertain.
4. **Signposting**: Use clear topic sentences. Each paragraph should have one main idea.
5. **Citation integration**: Weave citations naturally into the text, not just tacked on at the end of sentences.

### 4.2 Citation Format

Support these styles (default: APA 7th edition):

**APA**: (Author, Year) — "Smith (2023) found that..." or "...significant effect (Smith, 2023)."
**AMA**: Superscript numbers¹ — sequential by order of appearance
**Vancouver**: Numbers in brackets [1] — sequential by order of appearance
**Chicago**: Footnotes or (Author Year)
**IEEE**: Numbers in brackets [1] — sequential by order of appearance

### 4.3 Draft File Format

Write to `research-output/drafts/<slug>-draft-v1.md`:

```markdown
---
title: "<Document Title>"
type: "<Document Type>"
date: "<YYYY-MM-DD>"
citation_style: "<style>"
word_count: <approximate>
status: "draft"
---

# <Document Title>

## Abstract

<Abstract text — 150-300 words summarizing the entire document>

## 1. Introduction

<Full prose paragraphs...>

## 2. [Next Section]

<Full prose paragraphs...>

...

## References

<Formatted reference list per citation style>
```

### 4.4 Writing Quality Standards

Each section must meet these criteria:

| Criterion | Standard |
|-----------|----------|
| **Coherence** | Clear logical flow between paragraphs; transitions connect ideas |
| **Evidence** | Every major claim supported by cited evidence |
| **Depth** | Analysis, not just description; explain *why*, not just *what* |
| **Objectivity** | Present multiple viewpoints; acknowledge limitations |
| **Precision** | Specific data, dates, figures — not vague generalizations |
| **Citations** | Minimum 5 citations for short docs, 10+ for papers/reviews |
| **Length** | Within ±20% of target word count |

### 4.5 Section-by-Section Writing Process

For each section in the approved outline:

1. Review the outline points and planned evidence
2. Research additional details if needed (web search)
3. Write Stage A: key points with evidence mapping
4. Write Stage B: convert to flowing prose
5. Add in-text citations
6. Review for coherence with previous sections
7. Move to next section

**Completion marker**: Complete draft written to `research-output/drafts/`.

---

## Phase 5 — Review & Polish

### 5.1 Self-Review Checklist

Run through this checklist on the draft:

**Structure & Flow**
- [ ] Abstract accurately summarizes the full document
- [ ] Introduction establishes context → gap → question/thesis
- [ ] Each section follows logically from the previous
- [ ] Conclusion addresses the research question directly
- [ ] No orphaned claims (every claim has evidence)

**Writing Quality**
- [ ] No bullet points in body text (prose only)
- [ ] Active voice predominant (passive voice only where conventions require it)
- [ ] Jargon defined on first use
- [ ] Paragraphs are 3-8 sentences (not too short, not too long)
- [ ] Transitions between sections are smooth

**Citations & Evidence**
- [ ] All factual claims have citations
- [ ] Citation format is consistent throughout
- [ ] Reference list matches in-text citations (no orphans either way)
- [ ] Sources are credible (peer-reviewed preferred)
- [ ] Recent sources included (within last 5 years for fast-moving fields)

**Formatting**
- [ ] Headings follow consistent hierarchy (h1 → h2 → h3)
- [ ] Tables and figures have captions and are referenced in text
- [ ] Abbreviations spelled out on first use
- [ ] Word count within target range

### 5.2 Polish Steps

1. Fix any issues found in the checklist
2. Improve transitions between sections
3. Tighten prose: remove redundant phrases, strengthen verbs
4. Verify all citations are properly formatted
5. Write/refine the abstract (should be written LAST, as it summarizes everything)
6. Copy final version to `research-output/final/<slug>.md`
7. Update frontmatter `status` to `"final"`

### 5.3 Final Document Format

The final file in `research-output/final/<slug>.md`:

```markdown
---
title: "<Document Title>"
type: "<Document Type>"
date: "<YYYY-MM-DD>"
citation_style: "<style>"
word_count: <actual>
status: "final"
---

# <Document Title>

<Full polished document with all sections>

## References

<Complete, properly formatted reference list>
```

**Completion marker**: Final document in `research-output/final/`, passes all checklist items.

---

## Phase 6 — Delivery

### 6.1 Present to User

Provide:
- **Document location**: Path to the final `.md` file
- **Summary**: Title, type, word count, number of citations, key sections
- **Quality notes**: Any limitations or areas the user might want to expand

### 6.2 Delivery Summary Format

```
📄 Research Document Complete
━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 Title: [title]
📝 Type: [document type]
📊 Word Count: ~[X] words
📚 Citations: [N] references
🗂️ Sections: [list main sections]

📁 Files:
  • Final: research-output/final/<slug>.md
  • Outline: research-output/outline.md
  • Draft: research-output/drafts/<slug>-draft-v1.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### 6.3 Post-Delivery Options

Inform the user they can:
- **Revise sections**: Request changes to specific sections or arguments
- **Add sections**: Expand with additional analysis or data
- **Change format**: Convert citation style or restructure
- **Deepen research**: Dive deeper into specific subtopics
- **Generate companion docs**: Create an executive summary, presentation outline, or abstract
- **Export**: The Markdown is ready for conversion to PDF, Word, or LaTeX

---

## Technical Reference

### Source Skills (from K-Dense-AI/claude-scientific-skills)

| Skill | Used For |
|-------|----------|
| scientific-writing | IMRAD structure, writing principles, prose style |
| literature-review | Systematic search, synthesis, thematic organization |
| statistical-analysis | Data interpretation, test selection, results reporting |
| citation-management | Citation formatting, BibTeX, reference validation |
| peer-review | Self-review checklist, quality assessment criteria |
| scientific-critical-thinking | Evidence evaluation, bias identification, logical rigor |

### Reference Materials

| File | Content |
|------|---------|
| `imrad_structure.md` | Detailed IMRAD section guide with venue-specific variations |
| `writing_principles.md` | Clarity, conciseness, precision rules |
| `citation_styles.md` | APA, AMA, Vancouver, Chicago, IEEE format guides |
| `reporting_guidelines.md` | CONSORT, STROBE, PRISMA checklists |
| `figures_tables.md` | Best practices for data visualization in papers |
| `professional_report_formatting.md` | Report layout and typography |
| `report_formatting_guide.md` | LaTeX template usage guide |

### Key Conventions

- **Output directory**: All files go to `research-output/`
- **File naming**: lowercase, hyphenated slugs (e.g., `remote-work-productivity`)
- **Markdown format**: Standard Markdown with YAML frontmatter
- **Citation minimum**: 5 for short documents, 10+ for full papers/reviews
- **Language**: Match user's language; academic English for formal documents unless specified otherwise
- **Tone**: Objective, evidence-based, analytical — not promotional or speculative

---

## Phase 7 — HTML Visual Report

**Trigger:** Automatically after Phase 5 (final document complete). Do not wait for user to ask.

### What to generate
Use `report-template.html` (in this `skill/` directory). Replace all `{{VARIABLE}}` placeholders and save as `research-output/final/<slug>-report.html`.

### Variable reference

| Variable | Source |
|----------|--------|
| `{{TITLE}}` | `config.json → title` |
| `{{DOC_TYPE}}` | `config.json → document_type` |
| `{{DATE}}` | Today's date (YYYY-MM-DD) |
| `{{WORD_COUNT}}` | Count words in final `.md` |
| `{{CITATION_COUNT}}` | Count entries in References section |
| `{{SECTION_COUNT}}` | Count H2 headings (excluding References) |
| `{{READ_TIME}}` | `round(word_count / 200)` |
| `{{LANGUAGE}}` | `config.json → language` |
| `{{TOC_ITEMS}}` | One `<a href="#section-id" class="toc-h2">Title</a>` per H2 section |
| `{{BODY_SECTIONS}}` | Full body — each H2 → `<section id="...">`, H3 → `<h3>`, paragraphs → `<p>`, lists → `<ul>/<ol>` |
| `{{REFERENCES_LIST}}` | Each reference → `<li><span class="ref-num">[N]</span> citation text</li>` |

### Callout boxes
For every section, wrap the **first sentence** in a `.callout` block to surface the key finding:
```html
<div class="callout"><strong>Key Finding</strong> First sentence of this section...</div>
```

### Delivery
```
📊 Visual Report Generated
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📁 research-output/final/<slug>-report.html
   Open in any browser — no server required.
   Browser → Print → Save as PDF to export.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Phase 8 — Reference Export (xlsx)

**Trigger:** Automatically offer after Phase 7 when citation count ≥ 5.

### Run
```bash
python .codebanana/.skills/research-writing/export_references.py <slug>
```

### Output
`research-output/final/<slug>-references.xlsx`

Columns: `# · Authors · Year · Title / Full Citation · Journal / Source · DOI / URL · Style · Notes`

Design: Navy header (`#1A2E4A`), alternating row shading, amber summary row, frozen header row.

After running, confirm to user:
```
📊 References Exported
━━━━━━━━━━━━━━━━━━━━━━
📁 research-output/final/<slug>-references.xlsx
   [N] references · [citation_style] format
━━━━━━━━━━━━━━━━━━━━━━
```

---

## Word Document Export

After delivering the final Markdown report, offer:

> "Want a .docx version for Word / Google Docs?"

If yes, run:
```bash
python .codebanana/.skills/research-writing/export_docx.py research-output/final/<slug>.md
```

Output: `research-output/final/<slug>.docx`

Confirm to user:
```
📄 Word Document Exported
━━━━━━━━━━━━━━━━━━━━━━━━
📁 research-output/final/<slug>.docx
   Ready to open in Word or Google Docs
━━━━━━━━━━━━━━━━━━━━━━━━
```
