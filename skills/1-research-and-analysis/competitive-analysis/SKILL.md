---
name: competitive-analysis
description: |
  Research competitors, build a competitive matrix, identify market gaps, and produce both a Markdown report and an Excel workbook.
  The agent guides the user through competitor discovery, multi-dimensional analysis, positioning mapping, and strategic recommendations.
  Triggers:
  - "competitive analysis"
  - "competitor analysis"
  - "analyze competitors"
  - "competitor matrix"
  - "competitive landscape"
  - "market analysis"
  - "SWOT analysis"
  - "competitor research"
  - "竞品分析"
  - "竞争对手分析"
  - "竞品矩阵"
  - "市场竞争分析"
---

# SKILL.md — Competitive Analysis

> Research competitors, build a structured matrix, identify market gaps, and deliver a Markdown report + Excel workbook.

---

## Overview

This skill executes a full competitive analysis workflow:

1. Scope the market and identify 5–8 competitors
2. Research each competitor via web search (homepage, pricing, reviews, job postings)
3. Build a feature/positioning matrix
4. Run SWOT + Porter's Five Forces analysis
5. Output a Markdown report + `competitive-analysis.xlsx`

**Output directory:** `competitive-analysis-output/`

---

## Phase 1 — Environment Setup

### 1.1 Create output directory

```bash
mkdir -p competitive-analysis-output
```

### 1.2 Verify tools available

No external dependencies required. This skill uses:
- **Web search**: competitor discovery + data gathering
- **File read/write**: Markdown + Python-generated xlsx
- **Terminal**: run Python to build xlsx

### 1.3 Verify Python + openpyxl

```bash
python3 -c "import openpyxl; print('openpyxl OK')"
```

If missing:
```bash
pip install openpyxl
```

**Completion condition:** directory created, openpyxl available.

---

## Phase 2 — Requirements Gathering

Collect these fields conversationally before starting research.

### Required fields

| Field | Description | Example |
|-------|-------------|---------|
| `product_name` | The user's product or company being analyzed | "Notion" |
| `product_description` | What it does, who it's for | "All-in-one workspace for teams" |
| `market_focus` | Geography / language / industry vertical | "Global SaaS, English-speaking market" |
| `analysis_depth` | Quick (3–5 competitors) or Deep (6–8 competitors) | "Deep" |

### Optional fields

| Field | Description | Default |
|-------|-------------|---------|
| `known_competitors` | Competitors the user already knows | Auto-discovered |
| `key_dimensions` | Custom feature dimensions to compare | Auto-generated |
| `output_language` | Language for the final report | Match user's language |

### Config file

After requirements collected, write `competitive-analysis-output/config.json`:

```json
{
  "product_name": "",
  "product_description": "",
  "market_focus": "",
  "analysis_depth": "deep",
  "known_competitors": [],
  "key_dimensions": [],
  "output_language": "auto"
}
```

**Completion condition:** config.json written, user confirms.

---

## Phase 3 — Competitor Discovery & Research

### 3.1 Discover competitors

Search queries to run (adapt to the product):

```
"<product_name> competitors" site:g2.com OR site:capterra.com OR site:producthunt.com
"<product_name> alternatives"
"best <product_category> tools <year>"
"<product_category> market leaders"
```

Target: 5–8 competitors (mix of direct + indirect).

### 3.2 Research each competitor

For each competitor, collect the following via web search:

| Data point | Where to find | Search query |
|------------|---------------|--------------|
| Homepage tagline & positioning | Official website | `site:<domain>` |
| Pricing tiers & model | Pricing page | `<competitor> pricing` |
| Key features | Product page / docs | `<competitor> features` |
| User sentiment | Reviews | `<competitor> reviews site:g2.com` or `site:capterra.com` |
| Target customer | Marketing copy | homepage + case studies |
| Recent moves | News / job posts | `<competitor> news 2025` or `<competitor> site:linkedin.com/jobs` |
| Funding / size | Crunchbase / LinkedIn | `<competitor> crunchbase` |

### 3.3 Write raw research notes

Save findings to `competitive-analysis-output/research-notes.md`:

```markdown
# Research Notes

## <Competitor Name>
- **Website:** <url>
- **Tagline:** <tagline>
- **Pricing:** <tiers>
- **Key features:** <list>
- **Target customer:** <description>
- **Strengths (from reviews):** <list>
- **Weaknesses (from reviews):** <list>
- **Recent moves:** <list>
- **Sources:** <urls>
```

**Completion condition:** research-notes.md written with data for all competitors.

---

## Phase 4 — Analysis

### 4.1 Feature Comparison Matrix

Build a matrix comparing the user's product against all competitors across 6–10 dimensions.

**Dimension selection rules:**
- Include dimensions the user explicitly mentioned
- Always include: Pricing model, Free tier, Target customer, Integrations
- Add product-specific dimensions (e.g., "AI features", "Offline mode", "API access")

**Rating scale:**
- ✅ Full support
- ⚠️ Partial / limited
- ❌ Not available
- — Unknown / N/A

### 4.2 SWOT Analysis

Run SWOT for the **user's product** in the competitive context:

| | Positive | Negative |
|---|---------|---------|
| **Internal** | Strengths | Weaknesses |
| **External** | Opportunities | Threats |

- Strengths: where user's product leads vs competitors
- Weaknesses: where user's product lags vs competitors
- Opportunities: underserved segments, gaps in competitor offerings
- Threats: competitor momentum, market trends working against user

### 4.3 Positioning Map

Identify 2 dimensions that most differentiate the market (e.g., Price vs. Features, Enterprise vs. SMB).

Describe each competitor's position in text (since we can't render a 2D chart in Markdown):

```
## Positioning Map: [Dimension X] vs [Dimension Y]

- [User's product]: [X position], [Y position] — [brief rationale]
- [Competitor A]: ...
- [Competitor B]: ...
```

### 4.4 Porter's Five Forces (optional — include for Deep analysis)

| Force | Assessment | Key factors |
|-------|------------|-------------|
| Threat of new entrants | Low / Med / High | |
| Bargaining power of buyers | Low / Med / High | |
| Bargaining power of suppliers | Low / Med / High | |
| Threat of substitutes | Low / Med / High | |
| Competitive rivalry | Low / Med / High | |

**Completion condition:** all four analysis sections drafted.

---

## Phase 5 — Build Outputs

### 5.1 Write Markdown Report

Save to `competitive-analysis-output/competitive-analysis-report.md`:

```markdown
# Competitive Analysis: <Product Name>

**Date:** <date>
**Market:** <market_focus>
**Competitors analyzed:** <count>

---

## Executive Summary

<3–5 sentences: biggest finding, key opportunity, top threat>

---

## Competitors Analyzed

| # | Competitor | Website | Positioning |
|---|-----------|---------|-------------|
| 1 | | | |

---

## Feature Comparison Matrix

<full matrix table>

---

## SWOT Analysis

### Strengths
<list>

### Weaknesses
<list>

### Opportunities
<list>

### Threats
<list>

---

## Positioning Map

<positioning description>

---

## Porter's Five Forces

<table>

---

## Strategic Recommendations

### Immediate Actions (0–3 months)
1. <action> — <rationale>

### Medium-term (3–12 months)
1. <action> — <rationale>

### Monitor
- <competitor or trend to watch>

---

## Sources

<list of all URLs used>
```

### 5.2 Build Excel Workbook

Save the following Python script to `competitive-analysis-output/build_xlsx.py`, then run it:

```python
import openpyxl
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side
from openpyxl.utils import get_column_letter
import json, os, sys
from datetime import datetime

# ── Load config ──────────────────────────────────────────────
with open("competitive-analysis-output/config.json") as f:
    config = json.load(f)

PRODUCT = config.get("product_name", "Your Product")
OUTPUT_PATH = "competitive-analysis-output/competitive-analysis.xlsx"

# ── Style helpers ─────────────────────────────────────────────
HEADER_FILL  = PatternFill("solid", fgColor="1F2937")   # dark gray
ACCENT_FILL  = PatternFill("solid", fgColor="3B82F6")   # blue
ALT_FILL     = PatternFill("solid", fgColor="F9FAFB")   # light gray
OWN_FILL     = PatternFill("solid", fgColor="DBEAFE")   # light blue (own product row)
WHITE_FILL   = PatternFill("solid", fgColor="FFFFFF")
HEADER_FONT  = Font(bold=True, color="FFFFFF", size=11)
TITLE_FONT   = Font(bold=True, size=14, color="1F2937")
LABEL_FONT   = Font(bold=True, size=11, color="1F2937")
NORMAL_FONT  = Font(size=10)

thin = Side(style="thin", color="E5E7EB")
BORDER = Border(left=thin, right=thin, top=thin, bottom=thin)

def hdr(ws, row, col, value):
    c = ws.cell(row=row, column=col, value=value)
    c.fill = HEADER_FILL; c.font = HEADER_FONT
    c.alignment = Alignment(horizontal="center", vertical="center", wrap_text=True)
    c.border = BORDER
    return c

def cell(ws, row, col, value, fill=None, bold=False, center=False):
    c = ws.cell(row=row, column=col, value=value)
    c.font = Font(bold=bold, size=10)
    if fill: c.fill = fill
    c.alignment = Alignment(horizontal="center" if center else "left",
                             vertical="center", wrap_text=True)
    c.border = BORDER
    return c

wb = openpyxl.Workbook()

# ══════════════════════════════════════════════════
# Sheet 1: Feature Matrix  (PLACEHOLDER — agent fills data)
# ══════════════════════════════════════════════════
ws1 = wb.active
ws1.title = "Feature Matrix"

# Agent: replace COMPETITORS and DIMENSIONS with real data
COMPETITORS = ["<<COMPETITOR_1>>", "<<COMPETITOR_2>>", "<<COMPETITOR_3>>"]
DIMENSIONS = [
    "Pricing model",
    "Free tier",
    "Target customer",
    "Integrations",
    "<<DIMENSION_5>>",
    "<<DIMENSION_6>>",
]
# Rating legend: ✅ Full  ⚠️ Partial  ❌ None  — Unknown
MATRIX_DATA = {
    PRODUCT:            ["<<val>>" for _ in DIMENSIONS],
    COMPETITORS[0]:     ["<<val>>" for _ in DIMENSIONS],
    COMPETITORS[1]:     ["<<val>>" for _ in DIMENSIONS],
    COMPETITORS[2]:     ["<<val>>" for _ in DIMENSIONS],
}

ws1.row_dimensions[1].height = 30
hdr(ws1, 1, 1, "Product / Feature")
for ci, comp in enumerate(DIMENSIONS):
    hdr(ws1, 1, ci + 2, comp)
    ws1.column_dimensions[get_column_letter(ci + 2)].width = 18

ws1.column_dimensions["A"].width = 22

for ri, (prod, vals) in enumerate(MATRIX_DATA.items()):
    row = ri + 2
    fill = OWN_FILL if prod == PRODUCT else (ALT_FILL if ri % 2 else WHITE_FILL)
    cell(ws1, row, 1, prod, fill=fill, bold=(prod == PRODUCT))
    for ci, v in enumerate(vals):
        cell(ws1, row, ci + 2, v, fill=fill, center=True)
    ws1.row_dimensions[row].height = 22

# ══════════════════════════════════════════════════
# Sheet 2: SWOT
# ══════════════════════════════════════════════════
ws2 = wb.create_sheet("SWOT")
ws2.column_dimensions["A"].width = 20
ws2.column_dimensions["B"].width = 55

swot_sections = [
    ("💪 Strengths",     "ACCENT_FILL",  ["<<strength_1>>", "<<strength_2>>"]),
    ("⚠️ Weaknesses",    "HEADER_FILL",  ["<<weakness_1>>", "<<weakness_2>>"]),
    ("🚀 Opportunities", "ACCENT_FILL",  ["<<opportunity_1>>", "<<opportunity_2>>"]),
    ("🔴 Threats",       "HEADER_FILL",  ["<<threat_1>>", "<<threat_2>>"]),
]

row = 1
ws2.cell(row=row, column=1, value=f"SWOT Analysis — {PRODUCT}").font = TITLE_FONT
ws2.merge_cells(f"A{row}:B{row}")
ws2.row_dimensions[row].height = 28
row += 1

for section, fill_key, items in swot_sections:
    fill = ACCENT_FILL if fill_key == "ACCENT_FILL" else HEADER_FILL
    c = ws2.cell(row=row, column=1, value=section)
    c.font = HEADER_FONT; c.fill = fill; c.border = BORDER
    c.alignment = Alignment(vertical="center")
    ws2.merge_cells(f"A{row}:B{row}")
    ws2.row_dimensions[row].height = 24
    row += 1
    for item in items:
        cell(ws2, row, 1, "•", fill=ALT_FILL, center=True)
        cell(ws2, row, 2, item, fill=ALT_FILL)
        ws2.row_dimensions[row].height = 20
        row += 1
    row += 1

# ══════════════════════════════════════════════════
# Sheet 3: Positioning Map
# ══════════════════════════════════════════════════
ws3 = wb.create_sheet("Positioning Map")
ws3.column_dimensions["A"].width = 24
ws3.column_dimensions["B"].width = 20
ws3.column_dimensions["C"].width = 20
ws3.column_dimensions["D"].width = 40

ws3.cell(1, 1, "Positioning Map").font = TITLE_FONT
ws3.merge_cells("A1:D1")
ws3.row_dimensions[1].height = 28

headers = ["Competitor", "<<Axis X>>", "<<Axis Y>>", "Notes"]
for ci, h in enumerate(headers):
    hdr(ws3, 2, ci + 1, h)

pos_data = [
    [PRODUCT,         "<<X>>", "<<Y>>", "<<notes>>"],
    [COMPETITORS[0],  "<<X>>", "<<Y>>", "<<notes>>"],
    [COMPETITORS[1],  "<<X>>", "<<Y>>", "<<notes>>"],
    [COMPETITORS[2],  "<<X>>", "<<Y>>", "<<notes>>"],
]
for ri, row_data in enumerate(pos_data):
    r = ri + 3
    fill = OWN_FILL if row_data[0] == PRODUCT else (ALT_FILL if ri % 2 else WHITE_FILL)
    for ci, v in enumerate(row_data):
        cell(ws3, r, ci + 1, v, fill=fill, bold=(row_data[0] == PRODUCT), center=(ci > 0))
    ws3.row_dimensions[r].height = 22

# ══════════════════════════════════════════════════
# Sheet 4: Strategic Recommendations
# ══════════════════════════════════════════════════
ws4 = wb.create_sheet("Recommendations")
ws4.column_dimensions["A"].width = 28
ws4.column_dimensions["B"].width = 55

ws4.cell(1, 1, "Strategic Recommendations").font = TITLE_FONT
ws4.merge_cells("A1:B1")
ws4.row_dimensions[1].height = 28

rec_sections = [
    ("⚡ Immediate (0–3 months)",   ["<<action_1>>", "<<action_2>>"]),
    ("📈 Medium-term (3–12 months)", ["<<action_3>>", "<<action_4>>"]),
    ("👀 Monitor",                   ["<<competitor or trend>>", "<<competitor or trend>>"]),
]

row = 2
for section, items in rec_sections:
    c = ws4.cell(row=row, column=1, value=section)
    c.font = HEADER_FONT; c.fill = HEADER_FILL; c.border = BORDER
    c.alignment = Alignment(vertical="center")
    ws4.merge_cells(f"A{row}:B{row}")
    ws4.row_dimensions[row].height = 24
    row += 1
    for item in items:
        cell(ws4, row, 1, "→", fill=ALT_FILL, center=True)
        cell(ws4, row, 2, item, fill=ALT_FILL)
        ws4.row_dimensions[row].height = 20
        row += 1
    row += 1

# ══════════════════════════════════════════════════
# Save
# ══════════════════════════════════════════════════
wb.save(OUTPUT_PATH)
print(f"✅ Saved: {OUTPUT_PATH}")
```

**After generating real data**, replace all `<<PLACEHOLDER>>` values in the script with actual research findings, then run:

```bash
python3 competitive-analysis-output/build_xlsx.py
```

**Completion condition:** both files exist:
- `competitive-analysis-output/competitive-analysis-report.md`
- `competitive-analysis-output/competitive-analysis.xlsx`

---

## Phase 6 — Deliver

### Final checklist

- [ ] `research-notes.md` — raw data for all competitors
- [ ] `competitive-analysis-report.md` — full Markdown report
- [ ] `competitive-analysis.xlsx` — 4-sheet workbook (Feature Matrix / SWOT / Positioning / Recommendations)
- [ ] All `<<PLACEHOLDER>>` values replaced with real data
- [ ] Executive Summary written (3–5 sentences)
- [ ] Strategic Recommendations section has at least 3 concrete actions

### Show the user

1. Announce both output files with paths
2. Summarize: competitors found, top opportunity, top threat
3. Ask if they want to go deeper on any section

---

## Technical Reference

### Dependencies

| Tool | Purpose | Install |
|------|---------|---------|
| `openpyxl` | Build xlsx workbook | `pip install openpyxl` |
| Web search | Competitor research | Built-in |
| Python 3.x | Run build script | Pre-installed |

### Output files

| File | Description |
|------|-------------|
| `competitive-analysis-output/config.json` | Requirements config |
| `competitive-analysis-output/research-notes.md` | Raw research data |
| `competitive-analysis-output/competitive-analysis-report.md` | Final Markdown report |
| `competitive-analysis-output/competitive-analysis.xlsx` | Excel workbook (4 sheets) |
| `competitive-analysis-output/build_xlsx.py` | Script to (re)build Excel |

### Excel workbook sheets

| Sheet | Content |
|-------|---------|
| Feature Matrix | Product vs competitors across 6–10 dimensions |
| SWOT | Strengths / Weaknesses / Opportunities / Threats for user's product |
| Positioning Map | Competitors plotted on 2 key axes |
| Recommendations | Immediate + medium-term actions + items to monitor |

### Search strategy by data type

| Data type | Best source | Query pattern |
|-----------|------------|---------------|
| Positioning & tagline | Official site | `site:<domain>` |
| Pricing | Pricing page | `<name> pricing 2025` |
| User reviews | G2 / Capterra | `<name> reviews site:g2.com` |
| Feature list | Docs / product page | `<name> features` |
| Company size / funding | Crunchbase / LinkedIn | `<name> crunchbase` |
| Recent news | Google News | `<name> news 2025` |
| Hiring signals | LinkedIn Jobs | `<name> site:linkedin.com/jobs` |


---

## Phase 7 — Weekly Competitive Monitor

**Setup details:** see `phase7-weekly-monitor.md` (same skill directory).

**Trigger:** Offered in BOOTSTRAP Step 7 after full analysis delivery. Optional — user must opt in.

**Schedule:** Weekly, Monday 09:00, via `schedule_task`.

**What it does:** Re-searches top 3 competitors every week for pricing, feature, funding, and hiring signals. Appends a `## Weekly Update — YYYY-MM-DD` diff section to the existing report. Non-destructive — never overwrites previous content.

---

## HTML Report Generation

After delivering the Markdown report and Excel file, automatically generate an HTML dashboard. Do not wait for the user to ask.

**Template:** `report-template.html` (in this skill directory)

**Replace all `{{VARIABLE}}` placeholders:**

| Variable | Value |
|----------|-------|
| `{{PRODUCT_NAME}}` | User's product name |
| `{{MARKET}}` | Market / industry |
| `{{DATE}}` | Today's date |
| `{{COMPETITOR_COUNT}}` | Number of competitors analyzed |
| `{{TOP_OPPORTUNITY}}` | One-line top opportunity from SWOT |
| `{{TOP_THREAT}}` | One-line top threat from SWOT |
| `{{MARKET_MATURITY}}` | e.g. "Growing", "Mature", "Fragmented" |
| `{{FEATURE_MATRIX_TABLE}}` | Full HTML `<table>` with ✅/❌/⚠️ cells (use class `yes`/`no`/`partial`) |
| `{{POSITIONING_MAP_DESCRIPTION}}` | Text description of positioning (Price vs Features axes) |
| `{{SWOT_STRENGTHS}}` | `<li>` items |
| `{{SWOT_WEAKNESSES}}` | `<li>` items |
| `{{SWOT_OPPORTUNITIES}}` | `<li>` items |
| `{{SWOT_THREATS}}` | `<li>` items |
| `{{RECOMMENDATIONS}}` | HTML blocks using `.rec` div structure |

**Save as:** `competitive-analysis-output/competitive-analysis-report.html`

**Confirm to user:**
```
🌐 HTML Dashboard Generated
━━━━━━━━━━━━━━━━━━━━━━━━━━━
📁 competitive-analysis-output/competitive-analysis-report.html
   Open in browser for visual overview
━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
