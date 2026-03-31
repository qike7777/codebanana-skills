---
name: financial-analyst
description: |
  Build financial models, analyze P&L statements, create investor materials, and produce
  fundraising documents. Covers forecasting, valuation, unit economics, and scenario analysis.
  Use when the user needs financial modeling, business analysis, or investor-facing documents.
  Triggers:
  - "financial model"
  - "P&L"
  - "profit and loss"
  - "cash flow"
  - "valuation"
  - "financial analysis"
  - "unit economics"
  - "CAC"
  - "LTV"
  - "burn rate"
  - "runway"
  - "investor deck"
  - "pitch deck financials"
  - "fundraising"
  - "financial forecast"
  - "财务模型"
  - "财务分析"
  - "融资"
  - "估值"
  - "投资人材料"
---

# Financial Analyst — SKILL.md

> Build financial models and investor materials that are consistent, credible, and easy to defend.

---

## Overview

Good financial analysis is built on:
1. **Transparent assumptions** — every number has a source
2. **Internal consistency** — all statements tie together
3. **Scenario thinking** — base case is just one possibility
4. **Honest uncertainty** — project what you can defend

**Never fabricate numbers.** Only model with data provided by the user.

---

## Phase 1 — Understand the Business

Before building any model:

**Business context:**
- Business model: SaaS / marketplace / services / e-commerce / other?
- Stage: Pre-revenue / early traction / scaling?
- Revenue streams: How does money come in?
- Cost structure: What are the major cost drivers?
- Key metrics already tracked?

**For investor materials also ask:**
- Raise amount and instrument (SAFE, equity, convertible note)?
- Use of funds?
- Current traction (MRR, customers, growth rate)?
- Comparable companies if known?

---

## Phase 2 — Financial Model Structure

### Standard 3-Statement Model

```
Income Statement (P&L)
  ↓ Net Income feeds
Balance Sheet
  ↓ Cash position feeds
Cash Flow Statement
```

### Assumptions Sheet (always first)

```markdown
## Assumptions — [Company Name] — [Date]

### Revenue Assumptions
| Metric | Value | Source |
|--------|-------|--------|
| Starting MRR | $X | Actuals |
| Monthly growth rate (Y1) | X% | Historical avg / target |
| Monthly growth rate (Y2) | X% | Conservative estimate |
| Average contract value | $X | Actuals |
| Churn rate (monthly) | X% | Actuals / benchmark |

### Cost Assumptions
| Category | Value | Notes |
|---------|-------|-------|
| COGS % of revenue | X% | Infrastructure + support |
| Engineering headcount (Y1 end) | X | Planned hires |
| Avg engineering salary | $X | Market rate |
| Sales headcount (Y1 end) | X | |
| Marketing % of revenue | X% | |

### Operating Assumptions
| Metric | Value |
|--------|-------|
| Revenue recognition | Monthly (SaaS) |
| Payment terms | Net 30 |
| Tax rate | X% (or pre-tax model) |
```

---

## Phase 3 — Income Statement (P&L) Template

```
Income Statement — [Company Name]
                        Q1      Q2      Q3      Q4     FY
REVENUE
  MRR (end of period)   $X      $X      $X      $X
  ARR                   $X      $X      $X      $X
  New ARR               $X      $X      $X      $X
  Churned ARR          ($X)    ($X)    ($X)    ($X)
  Total Revenue         $X      $X      $X      $X    $X

COST OF REVENUE
  Infrastructure        $X      $X      $X      $X
  Customer Support      $X      $X      $X      $X
  Total COGS            $X      $X      $X      $X    $X

GROSS PROFIT            $X      $X      $X      $X    $X
Gross Margin %          X%      X%      X%      X%    X%

OPERATING EXPENSES
  Engineering           $X      $X      $X      $X
  Sales & Marketing     $X      $X      $X      $X
  G&A                   $X      $X      $X      $X
  Total OpEx            $X      $X      $X      $X    $X

EBITDA                 ($X)    ($X)    ($X)    ($X)  ($X)
EBITDA Margin           X%      X%      X%      X%    X%
```

---

## Phase 4 — Cash Flow & Runway

### Simplified Cash Flow Model

```
Cash Flow Statement — Monthly
                    Jan     Feb     Mar     Apr
Opening Cash        $X      $X      $X      $X
  + Revenue Inflow  $X      $X      $X      $X
  - Payroll        ($X)    ($X)    ($X)    ($X)
  - Infrastructure ($X)    ($X)    ($X)    ($X)
  - Marketing      ($X)    ($X)    ($X)    ($X)
  - Other Opex     ($X)    ($X)    ($X)    ($X)
Net Cash Flow      ($X)    ($X)    ($X)    ($X)
Closing Cash        $X      $X      $X      $X
Runway (months)     X       X       X       X
```

**Runway formula**: `Closing Cash / Average Monthly Burn`

### Scenario Analysis

Always present 3 scenarios:

| Scenario | Revenue Growth | Burn | Runway |
|---------|--------------|------|--------|
| Bear | X% MoM | $X/mo | X months |
| Base | X% MoM | $X/mo | X months |
| Bull | X% MoM | $X/mo | X months |

---

## Phase 5 — Unit Economics

### SaaS Unit Economics

```
CAC (Customer Acquisition Cost)
= Total Sales & Marketing Spend / New Customers Acquired

LTV (Lifetime Value)
= ARPU / Monthly Churn Rate
  (where ARPU = Average Revenue Per User per month)

LTV:CAC Ratio
= LTV / CAC
  Target: > 3x (ideally > 5x)

CAC Payback Period
= CAC / (ARPU × Gross Margin %)
  Target: < 12 months (enterprise: < 18 months)

Gross Margin
= (Revenue - COGS) / Revenue
  SaaS benchmark: 70-85%

Net Revenue Retention (NRR)
= (Starting MRR + Expansion - Contraction - Churn) / Starting MRR
  Target: > 100% (best-in-class: > 120%)
```

### Unit Economics Summary Template

```markdown
## Unit Economics Summary — [Date]

| Metric | Value | Benchmark |
|--------|-------|-----------|
| ARPU (monthly) | $X | — |
| Gross Margin | X% | 70-85% (SaaS) |
| Monthly Churn | X% | < 2% (SMB), < 0.5% (Enterprise) |
| CAC | $X | — |
| LTV | $X | — |
| LTV:CAC | Xx | > 3x |
| CAC Payback | X months | < 12 months |
| NRR | X% | > 100% |

**Interpretation**: [2-3 sentences on what these numbers mean and what to improve]
```

---

## Phase 6 — Investor Materials

### Pitch Deck Financial Slides

**Slide: Business Model**
- How you make money (pricing model, tiers)
- Revenue recognition
- Key economic drivers

**Slide: Traction**
- MRR/ARR growth chart (ideally 12-18 months)
- Customer count growth
- Key milestones with dates

**Slide: Financial Projections**
- 3-year forecast (quarters for Y1, annual for Y2-3)
- Revenue, gross margin, EBITDA, cash
- Path to profitability (or next raise)

**Slide: Unit Economics**
- LTV:CAC, payback period, gross margin
- NRR if available

**Slide: Use of Funds**
```markdown
## Use of Funds: $[X]M Raise

| Category | Amount | % | Purpose |
|---------|--------|---|---------|
| Engineering | $X | X% | [X hires for Y] |
| Sales & Marketing | $X | X% | [GTM expansion] |
| G&A | $X | X% | [Legal, ops] |
| Working Capital | $X | X% | [Buffer] |
| **Total** | **$X** | **100%** | |

**Runway**: [X months] to [key milestone]
```

### Investor One-Pager Template

```markdown
# [Company Name] — Investment Overview

**[Tagline — what you do in one line]**

## The Problem
[2-3 sentences on the pain point and market gap]

## The Solution
[2-3 sentences on your approach and why it's better]

## Traction
- $[X]K MRR / [X]% MoM growth
- [X] paying customers
- [Key proof point — notable customer, partnership, award]

## Market
- TAM: $[X]B | SAM: $[X]B | SOM: $[X]M
- [Source]

## Business Model
- [Pricing model, ACV, sales motion]
- Gross margin: [X%]

## Unit Economics
- LTV:CAC [Xx] | CAC Payback [X months]

## Team
- [Founder name]: [Relevant background]
- [Co-founder]: [Relevant background]

## The Ask
Raising $[X]M [instrument] at $[X]M [pre/post] valuation
Use of funds: [2-line summary]

[contact] | [website] | [deck link]
```

---

## Formatting Standards

- **Currency**: Always specify ($, €, £, ¥) — don't assume
- **Percentages**: One decimal (e.g., 23.4%)
- **Large numbers**: Use K/M/B (e.g., $2.3M, not $2,300,000)
- **Multiples**: Use x format (e.g., 3.2x LTV:CAC)
- **Negatives**: Use parentheses (e.g., ($450K) not -$450K)
- **Projections**: Label clearly "Projected" or "Forecast"
- **Actuals**: Label clearly "Actuals" with date range

---

## Quality Standards

**Every financial model must have:**
- A dedicated assumptions section with sources
- All three scenarios (bear/base/bull)
- Runway calculation
- Internal consistency check (revenue in P&L = cash inflows in CF statement)

**Every investor document must have:**
- A clear use of funds breakdown
- Traction with dates
- Path to profitability or next milestone
- Honest risk section (investors respect candor)

---

## Phase 7 — HTML Financial Dashboard

**After delivering the financial model or analysis, automatically proceed to generate the HTML dashboard.** Do not wait for the user to ask. Use the template at `dashboard-template.html` (in this `skill/` directory), replace all `{{VARIABLE}}` placeholders with the financial data just produced, and deliver the `.html` file in the same response.

### Design
**Style**: Bento Grid (布局10) + Neumorphism (风格#12)
- Sidebar navigation + main content area
- Soft extruded shadows on all cards (`#E0E5EC` base, light/dark shadow pair)
- Color coding: Blue (revenue/growth) · Green (profit) · Red (burn) · Yellow (runway)
- Circular ring progress indicators, SVG bar charts, scenario tabs

### Variable Reference

| Variable | Description |
|----------|-------------|
| `{{COMPANY_NAME}}` | Company name (sidebar logo + title) |
| `{{PERIOD}}` | Reporting period (e.g. "Q1 2025") |
| `{{LAST_UPDATED}}` | Date string |
| `{{MRR_VALUE}}` | MRR figure (e.g. "$42K") |
| `{{MRR_DELTA}}` | MoM change (e.g. "12%") |
| `{{MRR_PCT}}` | Ring fill % (0-100) |
| `{{MRR_RING_OFFSET}}` | SVG dashoffset = `125.6 × (1 - pct/100)` |
| `{{ARR_VALUE}}` / `{{BURN_VALUE}}` / `{{RUNWAY_MONTHS}}` | Other key metrics |
| `{{ARR_*}}` / `{{BURN_*}}` / `{{RUNWAY_*}}` | Same pattern as MRR |
| `{{GROSS_MARGIN}}` / `{{GM_DELTA}}` / `{{COGS_VALUE}}` | Gross margin card |
| `{{NRR_VALUE}}` / `{{NRR_DELTA}}` / `{{NRR_COLOR}}` / `{{NRR_ARROW}}` | NRR card |
| `{{B1_Y}}` – `{{B7_Y}}` | Bar chart Y positions (from top, pixels) |
| `{{B1_H}}` – `{{B7_H}}` | Bar chart heights (pixels, max ~110) |
| `{{G1_Y}}` – `{{G7_Y}}` / `{{G1_H}}` – `{{G7_H}}` | Gross profit bars |
| `{{M1}}` – `{{M7}}` | Month labels (e.g. "Jan", "Feb"…) |
| `{{ENG_AMT}}` / `{{SMK_AMT}}` / `{{GA_AMT}}` / `{{INF_AMT}}` | Expense amounts |
| `{{ENG_PCT}}` / `{{SMK_PCT}}` / `{{GA_PCT}}` / `{{INF_PCT}}` | Progress bar widths (0-100) |
| `{{ENG_REV_PCT}}` etc | % of revenue |
| `{{SC_GROWTH}}` / `{{SC_BURN}}` / `{{SC_RUNWAY}}` / `{{SC_BREAKEVEN}}` / `{{SC_ARR}}` | Scenario values (shown for Bear by default) |

### Bar Chart Calculation
```
Bar height (H) = (value / max_value) × 110
Bar Y position = 130 - H   (SVG viewBox height 140, bottom margin 10)
```

### Delivery
1. Read the template file
2. Calculate all SVG positions and ring offsets from user's numbers
3. Replace all `{{VARIABLE}}` placeholders
4. Save as `dashboard-[company].html`
5. Deliver standalone HTML file — no server required

---

## Phase 0 — Benchmark Research (Web Search)

**Trigger:** During Phase 1, when the user cannot provide industry benchmarks.  
**Action:** Proactively search for relevant benchmarks before building the model. Do not ask the user to supply numbers you can look up yourself.

### What to search

| Missing Data | Search Query |
|-------------|-------------|
| SaaS gross margin benchmark | `"SaaS gross margin benchmark {year} industry average"` |
| CAC / LTV benchmark by segment | `"SaaS CAC LTV ratio benchmark {stage} company {year}"` |
| Churn rate benchmark | `"SaaS monthly churn rate benchmark {segment} {year}"` |
| NRR benchmark | `"net revenue retention benchmark SaaS {year}"` |
| Valuation multiples | `"SaaS revenue multiple valuation {year} {stage}"` |
| Comparable company metrics | `"{competitor} revenue ARR growth rate {year}"` |
| Market size | `"{market} TAM SAM size {year} report"` |

### Rules
- Search **before** presenting the assumptions sheet, not after
- Label all benchmark data clearly: `[Source: X, Year: Y]`
- If benchmark range is wide, use **conservative end** for bear case, **median** for base
- Never present a benchmark as fact without citing the source

---

## Phase 8 — Excel Financial Model Export

**Trigger:** Automatically after Phase 7 (HTML Dashboard). Do not wait for user to ask.

### Run
```bash
python .codebanana/.skills/financial-analyst/export_financials.py
```

Reads from `financial-output/model-data.json` (agent writes this file after completing the model).

### model-data.json structure
```json
{
  "company": "Company Name",
  "periods": ["Q1","Q2","Q3","Q4","FY"],
  "months": ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],
  "income_statement": {
    "Revenue": { "Total Revenue": [42000, 50400, 60480, 72576, 225456] },
    "Cost of Revenue": { "Total COGS": [6300, 7560, 9072, 10887, 33819], "Gross Margin %": [0.85,0.85,0.85,0.85,0.85] },
    "Operating Expenses": { "EBITDA": [-19700,-9740,10312,18174,-954] }
  },
  "cash_flow": {
    "Opening Cash": [500000,480300,470560,480872,null],
    "Net Cash Flow": [-19700,-9740,10312,18174,null],
    "Runway (months)": [24.4,48.3,99,99,null]
  },
  "unit_economics": { "ltv_cac": "18.5x", "cac_payback_months": "3.6 mo", "nrr": "108%", "gross_margin": "85%" },
  "scenarios": {
    "Bear":  {"Revenue Growth MoM":"12%","Monthly Burn":"$25K","Runway":"19 mo"},
    "Base":  {"Revenue Growth MoM":"20%","Monthly Burn":"$15K","Runway":"33 mo"},
    "Bull":  {"Revenue Growth MoM":"30%","Monthly Burn":"$8K", "Runway":"62 mo"}
  }
}
```

**Agent writes this JSON after completing Phase 4 (Cash Flow) + Phase 5 (Unit Economics).**

### Output workbook — 4 sheets

| Sheet | Contents |
|-------|---------|
| **Income Statement** | P&L by quarter, section headers, margin rows highlighted |
| **Cash Flow & Runway** | Monthly cash in/out, closing cash, runway (color-coded: green=healthy, red=burn) |
| **Unit Economics** | Metric · Value · Benchmark · Status (✅/⚠️) |
| **Scenario Analysis** | Bear / Base / Bull side-by-side comparison |

Design: Navy headers, alternating row shading, amber summary rows, green/red status indicators.

### Delivery
```
📊 Excel Financial Model Exported
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📁 financial-output/financial-model-<company>.xlsx
   4 sheets: Income Statement · Cash Flow · Unit Economics · Scenarios
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```


---

## Phase 9 — Monthly Financial Check-in (Cron)

**Trigger:** Offered in BOOTSTRAP after full model delivery. Optional — user must opt in.

**Schedule:** Monthly, 1st of each month at 09:00, via `schedule_task`.

### What it does each month

1. Reads `financial-output/model-data.json` for current model state
2. Presents a **Monthly Status Card** to the user:

```
📊 Monthly Financial Check-in — [Month Year]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Last month snapshot:
  MRR:         $[X]   (Δ [+/-X%] vs prior month)
  Burn rate:   $[X]/mo
  Runway:      [X] months
  Gross margin:[X]%

👉 Please share your updated actuals for [current month]:
   - Actual revenue / MRR
   - Actual expenses (breakdown if available)
   - Any major changes to assumptions?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

3. User replies with updated numbers → agent:
   - Updates `financial-output/model-data.json`
   - Re-runs `export_financials.py` → new xlsx
   - Regenerates HTML dashboard (replaces `{{VARIABLES}}` with new data)
   - Delivers: "✅ Dashboard + Excel updated for [Month]. Runway is now [X] months."

### Rules
- Never overwrite actuals with forecasts
- If user provides partial data, update whats available and flag whats missing
- Keep the check-in message concise — one card, not a wall of text
- Always run the script after updating JSON; never skip the xlsx regeneration step
