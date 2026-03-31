---
name: market-research
description: |
  Conduct structured market research on any industry, product category, or business opportunity.
  Produces a comprehensive Markdown report and an Excel summary covering market size, trends,
  customer segments, competitive landscape, Porter's Five Forces, and go-to-market insights.
  Based on openclaw/skills market-research-agent by 1kalin.
  Triggers:
  - "market research"
  - "research a market"
  - "market analysis"
  - "industry research"
  - "market opportunity"
  - "market sizing"
  - "市场调研"
  - "市场分析"
  - "行业研究"
---

# Market Research Skill

## Purpose

Help users conduct thorough, structured research on a market, industry, or business opportunity.
Outputs a Markdown research report + Excel summary with actionable recommendations.

---

## Defaults

| Parameter | Default |
|-----------|---------|
| Report language | English (match user's language) |
| Competitors to research | 5–8 |
| Output: Markdown report | Yes |
| Output: Excel summary | Yes |

### Completion condition
User confirms the market/industry/topic to research. All other fields are optional.

---

## Phase 1 — Requirements Gathering

### 1.1 Required fields

| Field | Description | Example |
|-------|-------------|---------|
| `market` | The market, industry, or opportunity to research | "AI writing tools", "electric bikes in Southeast Asia" |

### 1.2 Optional fields

| Field | Description | Example |
|-------|-------------|---------|
| `geography` | Target geography | "US", "Southeast Asia", "Global" |
| `use_case` | Why you're doing this research | "Evaluating entry", "investor deck", "product strategy" |
| `known_players` | Any players or companies you already know | "OpenAI, Jasper, Copy.ai" |
| `depth` | Quick overview vs. deep dive | "quick" / "deep" (default: deep) |

### 1.3 Gathering conversation

Ask these 3 questions in the welcome message:
1. What market or industry do you want to research?
2. What geography are you focused on? (or "Global" if not sure)
3. Why are you doing this research? (evaluating entry, investor deck, curiosity, etc.)

---

## Phase 2 — Market Research

### 2.1 Market Overview

Search for and compile:
- **Market size** (TAM / SAM / SOM if possible)
- **Growth rate** (CAGR, YoY)
- **Key trends** (3–5 major trends shaping the market)
- **Market maturity** (emerging / growing / mature / declining)
- **Key drivers** (what's making this market grow or shrink)
- **Key risks** (regulatory, technological, macro)

**Search queries to run:**
```
"<market> market size 2024 2025"
"<market> industry trends 2025"
"<market> growth rate CAGR"
"<market> market report"
"<geography> <market> market overview"
```

### 2.2 Customer Segments

For each major segment:
- **Segment name**
- **Size / proportion** of market
- **Demographics / firmographics**
- **Pain points** (top 3)
- **Buying behavior** (how they discover, evaluate, purchase)
- **Willingness to pay** (price sensitivity)
- **Key channels** to reach them

**Search queries:**
```
"<market> customer segments"
"<market> target audience"
"<market> buyer persona"
"who uses <market> products"
```

### 2.3 Competitive Landscape

Identify 5–8 major players (mix of leaders, challengers, niche players):

For each player:
- **Company name + founding year**
- **Type:** Market leader / Challenger / Niche / Emerging
- **Positioning:** What problem they solve, who they serve
- **Revenue / funding** (if available)
- **Key strengths**
- **Key weaknesses**
- **Market share** (if available)

**Search queries:**
```
"<market> top companies 2024 2025"
"best <market> companies"
"<market> market leaders"
"<market> startups funding"
```

### 2.4 Opportunity Analysis

Identify:
- **Market gaps** — unmet needs, underserved segments
- **Entry points** — where a new player could gain traction
- **Timing** — is now a good time to enter? Why?
- **Risks** — top 3 risks for a new entrant
- **Required capabilities** — what you'd need to compete

### 2.5 Porter's Five Forces

Analyze each force with a rating (Low / Medium / High):

| Force | Rating | Evidence |
|-------|--------|----------|
| Supplier Power | | |
| Buyer Power | | |
| Competitive Rivalry | | |
| Threat of Substitution | | |
| Threat of New Entry | | |

**Overall attractiveness score:** X/5

### 2.6 Go-to-Market Insights

- **Best channels** to reach customers (SEO, paid, community, partnerships, sales)
- **Common pricing models** in this market
- **Sales cycle** expectations (self-serve / transactional / enterprise)
- **Key partnerships** to consider
- **Regulatory considerations** (if any)

---

## Phase 3 — Analysis & Synthesis

### 3.1 SWOT Summary (market perspective)

For a new entrant into this market:

| | Strengths | Weaknesses |
|---|-----------|------------|
| **Internal** | What advantages a new entrant could bring | What they'd be lacking |
| **External** | **Opportunities** | **Threats** |
| | Market gaps, timing, trends | Incumbents, regulation, saturation |

### 3.2 Key Findings

Synthesize 3–5 headline findings:
- Each finding is 1–2 sentences
- Separate facts from estimates
- Flag areas needing primary research

---

## Phase 4 — Markdown Report Output

### Report structure

```markdown
# Market Research: [Market Name]
*Researched: [Date] | Geography: [Geography] | Depth: [Quick/Deep]*

## Executive Summary
3–5 bullet points. Most important findings first.

## 1. Market Overview
- Size, growth, maturity
- Key trends (numbered list)
- Key drivers and risks

## 2. Customer Segments
Table + description for each segment

## 3. Competitive Landscape
### Market Map
Brief description of how the market is structured

### Key Players
[Player profiles — name, type, positioning, strengths, weaknesses]

## 4. Opportunity Analysis
- Market gaps
- Entry points
- Timing assessment
- Risks for new entrants

## 5. Porter's Five Forces
Table with ratings + evidence

## 6. Go-to-Market Insights
- Channels, pricing, sales cycle, partnerships

## 7. Key Findings & Recommendations
- Finding 1: ...
- Finding 2: ...
- Recommendation 1: ...
- Recommendation 2: ...

## Methodology & Limitations
- Data sources used
- Areas needing primary research
- Confidence level on key estimates
```

---

## Phase 5 — Excel Summary Output

Use `openpyxl` to generate an Excel file: `market-research-[market]-[date].xlsx`

### Sheet 1: Market Overview
| Metric | Value | Source | Confidence |
|--------|-------|--------|------------|
| TAM | | | |
| SAM | | | |
| Growth Rate (CAGR) | | | |
| Market Maturity | | | |
| Key Trends | | | |

### Sheet 2: Customer Segments
| Segment | Size | Pain Points | Channels | WTP |
|---------|------|-------------|----------|-----|

### Sheet 3: Competitive Landscape
| Company | Type | Positioning | Strengths | Weaknesses | Revenue/Funding |
|---------|------|-------------|-----------|------------|-----------------|

### Sheet 4: Porter's Five Forces
| Force | Rating | Evidence |
|-------|--------|----------|

### Sheet 5: Opportunities & Risks
| Type | Description | Priority |
|------|-------------|----------|

**Formatting rules:**
- Dark header row: `#1F3864` (white text)
- Alternate row shading: `#F2F7FF`
- Column widths: auto-fit
- Freeze top row on all sheets
- Bold key metrics

```python
import openpyxl
from openpyxl.styles import PatternFill, Font, Alignment
from datetime import datetime

def build_market_research_excel(data: dict, market: str) -> str:
    wb = openpyxl.Workbook()
    date_str = datetime.now().strftime("%Y-%m-%d")
    filename = f"market-research-{market.lower().replace(' ', '-')}-{date_str}.xlsx"

    header_fill = PatternFill("solid", fgColor="1F3864")
    alt_fill = PatternFill("solid", fgColor="F2F7FF")
    header_font = Font(color="FFFFFF", bold=True)

    # Build sheets using data dict
    # ... (agent generates full script based on actual research data)

    wb.save(filename)
    return filename
```

---

## Phase 6 — Delivery

1. Save Markdown report as `market-research-[market]-[date].md`
2. Generate and save Excel file as `market-research-[market]-[date].xlsx`
3. Deliver summary:

```
✅ Market Research Complete: [Market Name]

📄 Report saved: market-research-[market]-[date].md
📊 Excel saved: market-research-[market]-[date].xlsx

**Top 3 Findings:**
1. [Finding 1]
2. [Finding 2]
3. [Finding 3]

**Next steps:**
- [ ] Validate market size estimates with primary research
- [ ] Interview 3–5 potential customers in top segment
- [ ] Deep-dive on [most promising opportunity]
```

---

## Output Standards

- Always cite reasoning and assumptions
- Separate facts from estimates — label estimates with "(est.)"
- Use tables for comparisons
- End with actionable recommendations
- Flag areas needing primary research
- Match user's language (English / Chinese)
