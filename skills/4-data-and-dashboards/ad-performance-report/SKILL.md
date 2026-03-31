---
name: ad-performance-report
description: |
  Upload CSVs exported from Meta Ads, Google Ads, TikTok Ads, or any ad platform.
  Agent generates a standardized daily/weekly performance report: spend, impressions,
  clicks, conversions, ROI — with WoW comparison and automatic anomaly alerts
  (CPA spikes, CTR drops, budget pacing issues). Optional daily cron for auto-reports.
  Triggers:
  - "ad report"
  - "advertising report"
  - "campaign report"
  - "Meta Ads report"
  - "Google Ads report"
  - "TikTok Ads report"
  - "ad performance"
  - "CPA analysis"
  - "ROI report"
  - "广告日报"
  - "投放报告"
  - "广告数据分析"
  - "广告ROI"
---

# SKILL.md — Ad Performance Report

> CSV from any ad platform → standardized daily/weekly report with anomaly alerts.
> Cron-ready: set it once, get your report every morning automatically.

---

## Overview

**Pipeline (4 Phases):**
```
Phase 0 — Ingest      → Detect platform, normalize columns to standard schema
Phase 1 — Analyze     → python3 analyze.py → KPIs, trends, WoW comparison, anomaly detection
Phase 2 — Visualize   → HTML report dashboard (Chart.js, single file)
Phase 3 — Deliver     → Markdown daily/weekly report + dashboard + anomaly alerts
```

---

## Phase 0 — Ingest & Platform Detection

### 0.1 Supported Platforms

| Platform | Signature Columns |
|----------|------------------|
| **Meta Ads** | `Campaign name`, `Impressions`, `Clicks`, `Amount spent`, `Results`, `Cost per result`, `CTR` |
| **Google Ads** | `Campaign`, `Impressions`, `Clicks`, `Cost`, `Conversions`, `Conv. rate`, `CPC` |
| **TikTok Ads** | `Campaign Name`, `Impressions`, `Clicks`, `Cost`, `Conversions`, `CPM`, `CPC` |
| **Generic** | Any CSV with spend + impressions or clicks columns |

### 0.2 Standard Schema (normalized)

```python
FIELD_MAP = {
    'date':         ['Date', 'Day', 'Report Date', 'date'],
    'campaign':     ['Campaign name', 'Campaign', 'Campaign Name', 'campaign'],
    'adset':        ['Ad Set Name', 'Ad Group', 'Ad set name', 'adset'],
    'ad':           ['Ad name', 'Ad Name', 'ad'],
    'impressions':  ['Impressions', 'impressions'],
    'clicks':       ['Clicks', 'Link clicks', 'clicks'],
    'spend':        ['Amount spent', 'Cost', 'Spend', 'spend', 'amount_spent'],
    'conversions':  ['Results', 'Conversions', 'conversions', 'Purchase'],
    'revenue':      ['Revenue', 'Conv. value', 'Purchase value', 'revenue'],
    'ctr':          ['CTR', 'Click-through rate', 'ctr'],
    'cpc':          ['CPC', 'Cost per click', 'cpc'],
    'cpm':          ['CPM', 'Cost per 1,000 impressions', 'cpm'],
    'cpa':          ['Cost per result', 'Cost / conv.', 'CPA', 'cpa'],
    'roas':         ['Purchase ROAS', 'Conv. value/cost', 'roas'],
}
```

### 0.3 Computed Metrics

Always compute these even if not in source data:
```python
df['ctr']  = df['clicks'] / df['impressions'] * 100          # Click-through rate %
df['cpc']  = df['spend'] / df['clicks']                       # Cost per click
df['cpm']  = df['spend'] / df['impressions'] * 1000           # Cost per mille
df['cpa']  = df['spend'] / df['conversions']                  # Cost per acquisition
df['cvr']  = df['conversions'] / df['clicks'] * 100           # Conversion rate %
df['roas'] = df['revenue'] / df['spend']                      # Return on ad spend
```

---

## Phase 1 — Analysis Script

### analyze.py

```python
#!/usr/bin/env python3
"""
Ad Performance Report Generator
Usage: python3 analyze.py --input <csv> [--compare <prev_csv>] --output <html> --report <md>
"""
import argparse, json
from pathlib import Path
import pandas as pd
import numpy as np

FIELD_MAP = {
    'date':        ['Date','Day','Report Date','date'],
    'campaign':    ['Campaign name','Campaign','Campaign Name','campaign'],
    'impressions': ['Impressions','impressions'],
    'clicks':      ['Clicks','Link clicks','clicks'],
    'spend':       ['Amount spent','Cost','Spend','spend','amount_spent'],
    'conversions': ['Results','Conversions','conversions','Purchase'],
    'revenue':     ['Revenue','Conv. value','Purchase value','revenue'],
    'ctr':         ['CTR','Click-through rate','ctr'],
    'cpc':         ['CPC','Cost per click','cpc'],
    'cpm':         ['CPM','cpm'],
    'cpa':         ['Cost per result','Cost / conv.','CPA','cpa'],
    'roas':        ['Purchase ROAS','Conv. value/cost','roas'],
}

def normalize(df):
    rename = {}
    col_lower = {c.lower().strip(): c for c in df.columns}
    for target, sources in FIELD_MAP.items():
        for s in sources:
            if s.lower() in col_lower and target not in rename.values():
                rename[col_lower[s.lower()]] = target
                break
    df = df.rename(columns=rename)
    # Clean numeric columns
    for col in ['impressions','clicks','spend','conversions','revenue']:
        if col in df.columns:
            df[col] = pd.to_numeric(df[col].astype(str).str.replace(r'[$,€£%]','',regex=True), errors='coerce').fillna(0)
    return df

def compute_kpis(df):
    totals = {}
    for col in ['impressions','clicks','spend','conversions','revenue']:
        if col in df.columns:
            totals[col] = float(df[col].sum())
    imp = totals.get('impressions', 0)
    clk = totals.get('clicks', 0)
    spd = totals.get('spend', 0)
    cvn = totals.get('conversions', 0)
    rev = totals.get('revenue', 0)
    totals['ctr']  = round(clk / max(imp, 1) * 100, 2)
    totals['cpc']  = round(spd / max(clk, 1), 2)
    totals['cpm']  = round(spd / max(imp, 1) * 1000, 2)
    totals['cpa']  = round(spd / max(cvn, 1), 2)
    totals['cvr']  = round(cvn / max(clk, 1) * 100, 2)
    totals['roas'] = round(rev / max(spd, 1), 2)
    return totals

def wow_comparison(current, previous):
    """Compute WoW change for each KPI"""
    changes = {}
    for k in current:
        if k in previous and previous[k] != 0:
            changes[k] = round((current[k] - previous[k]) / abs(previous[k]) * 100, 1)
        else:
            changes[k] = None
    return changes

def detect_anomalies(df):
    """
    Flag campaigns/days where key metrics deviate significantly.
    Returns list of anomaly dicts with severity and description.
    """
    anomalies = []
    if 'campaign' not in df.columns: return anomalies

    campaign_data = df.groupby('campaign').agg({
        'spend': 'sum', 'clicks': 'sum', 'impressions': 'sum',
        'conversions': 'sum'
    }).reset_index()

    # Recompute derived metrics per campaign
    campaign_data['cpa']  = campaign_data['spend'] / campaign_data['conversions'].replace(0, np.nan)
    campaign_data['ctr']  = campaign_data['clicks'] / campaign_data['impressions'].replace(0, np.nan) * 100
    campaign_data['cvr']  = campaign_data['conversions'] / campaign_data['clicks'].replace(0, np.nan) * 100

    for metric, label, direction in [
        ('cpa',  'CPA',  'high'),
        ('ctr',  'CTR',  'low'),
        ('cvr',  'CVR',  'low'),
    ]:
        if metric not in campaign_data.columns: continue
        vals = campaign_data[metric].dropna()
        if len(vals) < 2: continue
        mean, std = vals.mean(), vals.std()
        for _, row in campaign_data.iterrows():
            val = row.get(metric)
            if pd.isna(val): continue
            z = (val - mean) / max(std, 0.001)
            if (direction == 'high' and z > 2) or (direction == 'low' and z < -2):
                severity = '🔴 Critical' if abs(z) > 3 else '🟡 Warning'
                anomalies.append({
                    'campaign': row['campaign'],
                    'metric': label,
                    'value': round(float(val), 2),
                    'mean': round(float(mean), 2),
                    'severity': severity,
                    'description': f"{row['campaign']}: {label} is {round(float(val),2)} vs avg {round(float(mean),2)} ({'+' if z>0 else ''}{round(z,1)}σ)"
                })
    return anomalies

def build_trend(df):
    if 'date' not in df.columns: return {}
    df = df.copy()
    df['date'] = pd.to_datetime(df['date'], errors='coerce')
    daily = df.groupby('date').agg({k: 'sum' for k in ['spend','clicks','impressions','conversions','revenue'] if k in df.columns}).reset_index()
    daily = daily.sort_values('date')
    result = {'labels': daily['date'].dt.strftime('%Y-%m-%d').tolist()}
    for col in ['spend','clicks','conversions','revenue']:
        if col in daily.columns:
            result[col] = daily[col].fillna(0).round(2).tolist()
    return result

def campaign_breakdown(df):
    if 'campaign' not in df.columns: return []
    agg = df.groupby('campaign').agg({
        k: 'sum' for k in ['spend','clicks','impressions','conversions'] if k in df.columns
    }).reset_index()
    agg['cpa'] = (agg['spend'] / agg['conversions'].replace(0, np.nan)).round(2)
    agg['ctr'] = (agg['clicks'] / agg['impressions'].replace(0, np.nan) * 100).round(2)
    return agg.sort_values('spend', ascending=False).head(10).fillna(0).to_dict('records')

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('--input',   required=True)
    parser.add_argument('--compare', default=None, help='Previous period CSV for WoW comparison')
    parser.add_argument('--output',  default='ad-report.html')
    parser.add_argument('--report',  default='ad-report.md')
    args = parser.parse_args()

    df = normalize(pd.read_csv(args.input))
    current_kpis = compute_kpis(df)

    wow = {}
    if args.compare:
        df_prev = normalize(pd.read_csv(args.compare))
        wow = wow_comparison(current_kpis, compute_kpis(df_prev))

    data = {
        'kpis':       current_kpis,
        'wow':        wow,
        'trend':      build_trend(df),
        'campaigns':  campaign_breakdown(df),
        'anomalies':  detect_anomalies(df),
        'date_range': [str(df['date'].min()), str(df['date'].max())] if 'date' in df.columns else [],
        'rows':       len(df),
    }

    data_json = json.dumps(data, ensure_ascii=False, default=str)
    tpl_path = Path(__file__).parent / 'ad-report-template.html'
    if tpl_path.exists():
        html = tpl_path.read_text().replace('__DASHBOARD_DATA__', data_json)
    else:
        html = f'<script>window.DASHBOARD_DATA={data_json}</script>'
    Path(args.output).write_text(html)

    report_skeleton = f"""# Ad Performance Report
**Period:** {data['date_range'][0] if data['date_range'] else 'N/A'} → {data['date_range'][-1] if data['date_range'] else 'N/A'}

## Summary KPIs
| Metric | Value | WoW |
|--------|-------|-----|
| Spend | ${current_kpis.get('spend',0):,.2f} | {wow.get('spend','—')}% |
| Impressions | {current_kpis.get('impressions',0):,.0f} | {wow.get('impressions','—')}% |
| Clicks | {current_kpis.get('clicks',0):,.0f} | {wow.get('clicks','—')}% |
| CTR | {current_kpis.get('ctr',0):.2f}% | {wow.get('ctr','—')}% |
| Conversions | {current_kpis.get('conversions',0):,.0f} | {wow.get('conversions','—')}% |
| CPA | ${current_kpis.get('cpa',0):,.2f} | {wow.get('cpa','—')}% |
| ROAS | {current_kpis.get('roas',0):.2f}x | {wow.get('roas','—')}% |

## Anomalies
{"\\n".join([f"- {a['severity']} {a['description']}" for a in data['anomalies']]) or "None detected."}

## Campaign Breakdown
<!-- Agent: describe top/bottom performers -->

## Trend Analysis
<!-- Agent: describe spend/conversion trend -->

## Recommendations
<!-- Agent: top 3 actions -->
"""
    Path(args.report).write_text(report_skeleton)
    print(json.dumps({'status': 'ok', 'output': args.output, 'report': args.report, 'data': data}))

if __name__ == '__main__':
    main()
```

---

## Phase 2 — HTML Dashboard Layout

**Bento Grid Layout:**
```
┌──────────────────────────────────────────────────────────────┐
│  HEADER: Period · Platform badge · Anomaly count badge       │
│  bg: #f2f2f7                                                 │
├────────┬────────┬────────┬────────┬────────┬────────┬───────┤
│ Spend  │ Impr.  │ Clicks │  CTR   │  CVR   │  CPA   │ ROAS  │
│ [1×1]  │ [1×1]  │ [1×1]  │ [1×1]  │ [1×1]  │ [1×1]  │ [1×1] │
│ WoW ↑↓ │        │        │        │        │        │       │
├────────┴────────┴────────┴────────┼────────┴────────┴───────┤
│  📈 Daily Spend & Conversions [2×2]│  🔴 Anomaly Panel [1×2] │
│  dual-axis line chart              │  alert cards stack      │
│  spend: #007aff / conv: #34c759    │  red left border cards  │
├───────────────────────────────────┤                         │
│  📊 Campaign Performance [2×1]     │                         │
│  horizontal bar, sorted by spend   │                         │
└───────────────────────────────────┴─────────────────────────┘
```

Cards: `border-radius: 22px`, `background: #fff`
KPI cards: large bold number, colored WoW badge (green `#34c759` / red `#ff3b30`)
Anomaly cards: white bg, `border-left: 4px solid #ff3b30`, red label text

**Design tokens (Apple / Bento Grid):**
```css
/* === Apple Bento Grid Design System === */
--bg: #f2f2f7;
--surface: #ffffff;
--accent: #007aff;        /* iOS system blue */
--accent-2: #34c759;      /* iOS system green */
--accent-3: #ff9500;      /* iOS system orange */
--danger: #ff3b30;        /* iOS system red */
--warning: #ffcc00;       /* iOS system yellow */
--text: #1c1c1e;
--text-muted: #8e8e93;
--radius: 22px;
--font: -apple-system, 'SF Pro Display', 'Helvetica Neue', sans-serif;
--shadow: 0 4px 16px rgba(0,0,0,0.06);
--gap: 16px;
```

**Bento Grid layout:**
```css
.bento-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; padding: 16px; }
.bento-2x2 { grid-column: span 2; grid-row: span 2; }
.bento-2x1 { grid-column: span 2; }
.card { background: #fff; border-radius: 22px; padding: 20px; box-shadow: 0 4px 16px rgba(0,0,0,0.06); transition: transform 0.2s, box-shadow 0.2s; }
.card:hover { transform: scale(1.02); box-shadow: 0 8px 24px rgba(0,0,0,0.10); }
```

**Chart colors (iOS system):**
```js
const COLORS = ['#007aff','#34c759','#ff9500','#ff3b30','#af52de','#5ac8fa','#ff2d55','#ffcc00']
```

**WoW badge logic:**
```js
const positiveMetrics = ['clicks','impressions','conversions','ctr','cvr','roas']
const negativeMetrics = ['spend','cpc','cpm','cpa']
// green badge: #34c759 background, white text
// red badge:   #ff3b30 background, white text
```

**Anomaly cards:**
```html
<div class="anomaly-card" style="background:#fff0ef; border-radius:12px; padding:12px 16px; border-left:4px solid #ff3b30;">
  <span style="color:#ff3b30; font-weight:600;">🔴 Critical</span>
  <p style="color:#1c1c1e; margin:4px 0 0;">Campaign X: CPA is $45.20 vs avg $18.30 (+2.5σ)</p>
</div>
```

---

## Phase 3 — Agent Analysis Layer

### KPI Commentary
- Lead with the most important number (usually ROAS or CPA)
- Mention WoW direction for each key metric
- Flag if any metric crossed a threshold (CPA > 2× historical avg, CTR < 0.5%, ROAS < 1)

### Anomaly Explanation
- For each flagged campaign: hypothesize cause (audience fatigue? budget exhausted? creative worn out?)
- Prioritize by severity (Critical first)
- Always end with a concrete action: "Pause / Scale / Investigate creative"

### Recommendations (always 3)
```
1. **[Immediate]** Pause <campaign> — CPA is 2.5× above average, likely audience fatigue
2. **[This week]** Scale <campaign> — Highest ROAS at <X>x, still has budget headroom
3. **[Structural]** Add UTM parameters to <channel> — conversion attribution is missing
```

---

## Phase 4 — Cron Setup (Optional)

### Daily Auto-Report

After first successful run, offer to set up a daily cron:

```
Would you like this report to run automatically every morning?
I'll set up a daily task that:
1. Reads the latest CSV from <folder> (you export and drop it there each day)
2. Compares against yesterday's CSV automatically
3. Generates report.html + report.md
4. Notifies you with the top anomalies

Say "yes, set up daily report" to enable.
```

Cron message template:
```
Read the latest CSV file in <workspace>/ad-data/ (the most recently modified .csv file).
Run: python3 <workspace>/scripts/analyze.py --input <latest_csv> --compare <previous_csv> --output <workspace>/reports/ad-report-$(date +%Y-%m-%d).html --report <workspace>/reports/ad-report-$(date +%Y-%m-%d).md
Then read the generated report.md and summarize: total spend, ROAS, top anomaly (if any), and top-performing campaign.
```

---

## Implementation Notes

### Dependencies
```
pandas>=1.5
numpy>=1.21
```

### Usage
```bash
# Single file
python3 analyze.py --input today.csv --output report.html --report report.md

# With WoW comparison
python3 analyze.py --input today.csv --compare last_week.csv --output report.html --report report.md
```

### Data Quality
- Strip currency symbols ($, €, £) and commas before numeric parsing
- Handle percentage columns (strip %, divide by 100 if > 1.0 after strip)
- Rows with spend=0 and impressions=0 → filter out (likely paused campaigns with no activity)
