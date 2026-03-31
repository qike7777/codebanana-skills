---
name: product-analytics-dashboard
description: |
  Upload or paste a CSV exported from GA4, Mixpanel, Amplitude, or any product analytics tool.
  Agent auto-detects fields, runs Python analysis, and generates an interactive HTML dashboard
  with user growth curves, retention heatmap, conversion funnel, device/channel breakdown.
  No Python or SQL knowledge required.
  Triggers:
  - "product analytics"
  - "product dashboard"
  - "user analytics"
  - "GA4 dashboard"
  - "Mixpanel dashboard"
  - "Amplitude dashboard"
  - "retention analysis"
  - "funnel analysis"
  - "产品数据看板"
  - "用户增长分析"
  - "留存分析"
  - "漏斗分析"
---

# SKILL.md — Product Analytics Dashboard

> CSV in → interactive HTML dashboard out.
> Agent detects your data format, runs analysis, generates a shareable single-file dashboard.

---

## Overview

**Pipeline (4 Phases):**
```
Phase 0 — Ingest      → User uploads CSV; Agent detects source platform + available fields
Phase 1 — Analyze     → python3 analyze.py → metrics computed, anomalies flagged
Phase 2 — Visualize   → HTML dashboard generated (Chart.js, single file, no server needed)
Phase 3 — Deliver     → Markdown report + dashboard path + key insights
```

---

## Phase 0 — Ingest & Field Detection

### 0.1 Supported Source Formats

Agent must auto-detect the source by inspecting column names:

| Platform | Signature Columns |
|----------|-------------------|
| **GA4** | `date`, `activeUsers`, `newUsers`, `sessions`, `screenPageViews`, `bounceRate`, `sessionSource`, `deviceCategory` |
| **Mixpanel** | `Date`, `DAU`, `WAU`, `MAU`, `New Users`, `Event Name`, `$os`, `$browser` |
| **Amplitude** | `date`, `active_users`, `new_users`, `event_type`, `platform`, `country` |
| **Generic** | Any CSV with a date column + at least one numeric metric column |

Detection logic (in order):
1. Check for GA4 signature → set `SOURCE=ga4`
2. Check for Mixpanel signature → set `SOURCE=mixpanel`
3. Check for Amplitude signature → set `SOURCE=amplitude`
4. Fallback → `SOURCE=generic`, ask user to confirm key columns

### 0.2 Field Mapping

Normalize to internal schema regardless of source:

```python
FIELD_MAP = {
    # date
    'date': ['date', 'Date', 'Day', 'day', 'ds'],
    # users
    'active_users': ['activeUsers', 'DAU', 'active_users', 'Active Users'],
    'new_users':    ['newUsers', 'New Users', 'new_users'],
    'sessions':     ['sessions', 'Sessions', 'session_count'],
    # engagement
    'pageviews':    ['screenPageViews', 'pageviews', 'Page Views'],
    'bounce_rate':  ['bounceRate', 'Bounce Rate', 'bounce_rate'],
    # acquisition
    'source':       ['sessionSource', 'utm_source', 'Source', '$referrer'],
    'medium':       ['sessionMedium', 'utm_medium', 'Medium'],
    'channel':      ['sessionDefaultChannelGroup', 'Channel', 'channel'],
    # device
    'device':       ['deviceCategory', 'Device Category', '$os', 'platform'],
    # funnel (if present)
    'funnel_step':  ['step', 'funnel_step', 'event_name', 'Event Name', 'event_type'],
    'funnel_count': ['users', 'count', 'event_count', 'User Count'],
    # retention (if present)
    'cohort_date':  ['cohort_date', 'Cohort Date', 'cohort'],
    'day_n':        ['day_0','day_1','day_7','day_14','day_30'],
}
```

### 0.3 Capability Detection

After field mapping, determine what charts are buildable:

```python
capabilities = {
    'growth_curve':   'date' in fields and any(f in fields for f in ['active_users','new_users','sessions']),
    'channel_breakdown': 'source' in fields or 'channel' in fields,
    'device_breakdown':  'device' in fields,
    'funnel':            'funnel_step' in fields and 'funnel_count' in fields,
    'retention_heatmap': 'cohort_date' in fields and any(f.startswith('day_') for f in fields),
}
```

Tell the user which charts will be generated based on their data.

---

## Phase 1 — Analysis Script

### analyze.py

```python
#!/usr/bin/env python3
"""
Product Analytics Dashboard Generator
Usage: python3 analyze.py --input <csv> --output <html> --report <md>
"""
import argparse, json, sys
from pathlib import Path
import pandas as pd

def detect_source(df):
    cols = set(df.columns.str.lower())
    if {'activeusers','newusers','sessions'} & cols: return 'ga4'
    if {'dau','mau','new users'} & {c.lower() for c in df.columns}: return 'mixpanel'
    if {'active_users','event_type'} & cols: return 'amplitude'
    return 'generic'

def normalize_columns(df, source):
    """Rename columns to internal schema"""
    rename = {}
    col_lower = {c.lower(): c for c in df.columns}
    mapping = {
        'date':         ['date','day','ds'],
        'active_users': ['activeusers','dau','active_users','active users'],
        'new_users':    ['newusers','new users','new_users'],
        'sessions':     ['sessions','session_count'],
        'pageviews':    ['screenpageviews','pageviews','page views'],
        'bounce_rate':  ['bouncerate','bounce rate','bounce_rate'],
        'source':       ['sessionsource','utm_source','source','$referrer'],
        'channel':      ['sessiondefaultchannelgroup','channel'],
        'device':       ['devicecategory','device category','$os','platform'],
        'funnel_step':  ['step','funnel_step','event_name','event type','event_type'],
        'funnel_count': ['users','count','event_count','user count'],
        'cohort_date':  ['cohort_date','cohort date','cohort'],
    }
    for target, sources in mapping.items():
        for s in sources:
            if s in col_lower and target not in rename.values():
                rename[col_lower[s]] = target
                break
    return df.rename(columns=rename)

def compute_growth_metrics(df):
    """Compute WoW and MoM growth rates"""
    if 'date' not in df.columns: return {}
    df = df.copy()
    df['date'] = pd.to_datetime(df['date'], errors='coerce')
    df = df.sort_values('date')
    metrics = {}
    for col in ['active_users', 'new_users', 'sessions']:
        if col in df.columns:
            latest = df[col].iloc[-1]
            week_ago = df[col].iloc[-8] if len(df) >= 8 else df[col].iloc[0]
            month_ago = df[col].iloc[-31] if len(df) >= 31 else df[col].iloc[0]
            metrics[col] = {
                'latest': float(latest),
                'wow': round((latest - week_ago) / max(week_ago, 1) * 100, 1),
                'mom': round((latest - month_ago) / max(month_ago, 1) * 100, 1),
            }
    return metrics

def detect_anomalies(df, col, threshold=2.0):
    """Flag dates where metric deviates > threshold std devs from rolling mean"""
    if col not in df.columns or 'date' not in df.columns: return []
    df = df.copy().sort_values('date')
    rolling_mean = df[col].rolling(7, min_periods=3).mean()
    rolling_std  = df[col].rolling(7, min_periods=3).std()
    anomalies = df[abs(df[col] - rolling_mean) > threshold * rolling_std]
    return anomalies[['date', col]].to_dict('records')

def compute_funnel(df):
    """Convert funnel step data to ordered list with drop-off rates"""
    if 'funnel_step' not in df.columns or 'funnel_count' not in df.columns:
        return []
    funnel = df.groupby('funnel_step')['funnel_count'].sum().reset_index()
    funnel = funnel.sort_values('funnel_count', ascending=False)
    steps = funnel.to_dict('records')
    for i, step in enumerate(steps):
        if i == 0:
            step['dropoff'] = 0
            step['rate'] = 100
        else:
            prev = steps[i-1]['funnel_count']
            step['dropoff'] = round((1 - step['funnel_count'] / max(prev, 1)) * 100, 1)
            step['rate'] = round(step['funnel_count'] / max(steps[0]['funnel_count'], 1) * 100, 1)
    return steps

def compute_channel_breakdown(df):
    col = 'channel' if 'channel' in df.columns else 'source'
    if col not in df.columns: return []
    metric = next((c for c in ['active_users','sessions','new_users'] if c in df.columns), None)
    if not metric: return []
    breakdown = df.groupby(col)[metric].sum().sort_values(ascending=False).head(8)
    return [{'label': k, 'value': int(v)} for k, v in breakdown.items()]

def compute_device_breakdown(df):
    if 'device' not in df.columns: return []
    metric = next((c for c in ['active_users','sessions','new_users'] if c in df.columns), None)
    if not metric: return []
    breakdown = df.groupby('device')[metric].sum().sort_values(ascending=False)
    return [{'label': k, 'value': int(v)} for k, v in breakdown.items()]

def compute_retention_heatmap(df):
    """Build cohort retention matrix"""
    if 'cohort_date' not in df.columns: return {}
    day_cols = [c for c in df.columns if c.startswith('day_')]
    if not day_cols: return {}
    matrix = df.set_index('cohort_date')[day_cols]
    # Normalize to percentages of day_0
    if 'day_0' in matrix.columns:
        for col in day_cols:
            matrix[col] = (matrix[col] / matrix['day_0'] * 100).round(1)
    return {
        'cohorts': list(matrix.index.astype(str)),
        'days':    [c.replace('day_', 'Day ') for c in day_cols],
        'values':  matrix.fillna(0).values.tolist(),
    }

def build_timeseries(df):
    if 'date' not in df.columns: return {}
    df = df.copy()
    df['date'] = pd.to_datetime(df['date'], errors='coerce')
    df = df.sort_values('date')
    result = {'labels': df['date'].dt.strftime('%Y-%m-%d').tolist()}
    for col in ['active_users', 'new_users', 'sessions', 'pageviews']:
        if col in df.columns:
            result[col] = df[col].fillna(0).tolist()
    return result

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('--input',  required=True)
    parser.add_argument('--output', default='dashboard.html')
    parser.add_argument('--report', default='report.md')
    args = parser.parse_args()

    df = pd.read_csv(args.input)
    source = detect_source(df)
    df = normalize_columns(df, source)

    data = {
        'source': source,
        'rows': len(df),
        'date_range': [str(df['date'].min()), str(df['date'].max())] if 'date' in df.columns else [],
        'timeseries': build_timeseries(df),
        'growth': compute_growth_metrics(df),
        'funnel': compute_funnel(df),
        'channel': compute_channel_breakdown(df),
        'device':  compute_device_breakdown(df),
        'retention': compute_retention_heatmap(df),
        'anomalies': detect_anomalies(df, 'active_users') if 'active_users' in df.columns else [],
    }

    # Write data JSON for template
    data_json = json.dumps(data, ensure_ascii=False, default=str)

    # Read HTML template and inject data
    tpl_path = Path(__file__).parent / 'dashboard-template.html'
    if tpl_path.exists():
        html = tpl_path.read_text()
        html = html.replace('__DASHBOARD_DATA__', data_json)
    else:
        html = f'<script>window.DASHBOARD_DATA={data_json}</script>'

    Path(args.output).write_text(html)

    # Write minimal report skeleton (Agent fills in analysis)
    report = f"""# Product Analytics Report

**Source:** {source.upper()}
**Data Range:** {data['date_range'][0] if data['date_range'] else 'N/A'} → {data['date_range'][-1] if data['date_range'] else 'N/A'}
**Rows:** {data['rows']}

## Key Metrics
<!-- Agent: fill from growth dict -->

## Growth Trends
<!-- Agent: describe timeseries patterns -->

## Channel Analysis
<!-- Agent: describe channel breakdown -->

## Funnel Analysis
<!-- Agent: describe funnel drop-offs -->

## Anomalies
<!-- Agent: explain flagged dates -->

## Recommendations
<!-- Agent: top 3 actionable recommendations -->
"""
    Path(args.report).write_text(report)
    print(json.dumps({'status': 'ok', 'output': args.output, 'report': args.report, 'data': data}))

if __name__ == '__main__':
    main()
```

---

## Phase 2 — HTML Dashboard Template

### dashboard-template.html

Single-file, self-contained HTML. All charts rendered by Chart.js (CDN). No server needed.

**Bento Grid Layout:**
```
┌─────────────────────────────────────────────────────────────┐
│  HEADER: Product Name · Date Range · Source badge           │
│  bg: #f2f2f7, font: -apple-system                           │
├───────────┬───────────┬───────────┬───────────┬─────────────┤
│  DAU      │ New Users │ Sessions  │  Bounce   │  (5th KPI)  │
│ [1×1]     │ [1×1]     │ [1×1]     │ [1×1]     │  [1×1]      │
│ #007aff   │ #34c759   │ #ff9500   │ #ff3b30   │  #af52de    │
│ +12% WoW  │ +8% WoW   │ +5% WoW   │ -2% WoW   │             │
├───────────┴───────────┴───────────┼───────────┴─────────────┤
│  📈 User Growth Curve [2×2]        │  🔽 Funnel [1×2]        │
│  line chart, multi-series          │  bar chart vertical     │
│  #007aff DAU / #34c759 New Users   │  drop-off % labels      │
├───────────────────────────────────┼─────────────────────────┤
│  🥧 Channel Breakdown [1×1]        │  📱 Device [1×1]         │
│  doughnut chart                    │  doughnut chart         │
├───────────────────────────────────┴─────────────────────────┤
│  🔥 Retention Heatmap [full width 4×1]                       │
│  table, green color scale, cohort × day_N                    │
└─────────────────────────────────────────────────────────────┘
```

Cards: `border-radius: 22px`, `background: #fff`, `box-shadow: 0 4px 16px rgba(0,0,0,0.06)`
KPI cards: large number top (`SF Pro Display`, bold, 32px), WoW badge bottom (green↑/red↓)
Hover: `scale(1.02)` + deeper shadow

**Design tokens (Apple / Bento Grid):**
```css
/* === Apple Bento Grid Design System === */
--bg: #f2f2f7;           /* iOS system background */
--surface: #ffffff;       /* card surface */
--surface-2: #f2f2f7;    /* secondary surface */
--accent: #007aff;        /* iOS system blue */
--accent-2: #34c759;      /* iOS system green */
--accent-3: #ff9500;      /* iOS system orange */
--danger: #ff3b30;        /* iOS system red */
--warning: #ffcc00;       /* iOS system yellow */
--purple: #af52de;        /* iOS system purple */
--text: #1c1c1e;          /* iOS label */
--text-secondary: #3c3c43; /* iOS secondary label */
--text-muted: #8e8e93;    /* iOS tertiary label */
--separator: rgba(60,60,67,0.12); /* iOS separator */
--radius-sm: 12px;
--radius: 22px;           /* iOS large widget radius */
--font: -apple-system, 'SF Pro Display', 'SF Pro Text', 'Helvetica Neue', sans-serif;
--shadow: 0 4px 16px rgba(0,0,0,0.06);
--shadow-hover: 0 8px 24px rgba(0,0,0,0.10);
/* 8pt grid: all spacing multiples of 8 */
--gap: 16px;              /* 8×2 */
--padding: 20px;          /* ~8×2.5 */
```

**Bento Grid layout:**
```css
.bento-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: var(--gap);
    padding: var(--gap);
}
.bento-2x2 { grid-column: span 2; grid-row: span 2; }
.bento-2x1 { grid-column: span 2; }
.bento-1x2 { grid-row: span 2; }
.card {
    background: var(--surface);
    border-radius: var(--radius);
    padding: var(--padding);
    box-shadow: var(--shadow);
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}
.card:hover {
    transform: scale(1.02);
    box-shadow: var(--shadow-hover);
}
```

**Chart color palette** (iOS system colors):
```js
const COLORS = ['#007aff','#34c759','#ff9500','#ff3b30','#af52de','#5ac8fa','#ff2d55','#ffcc00']
```

**Retention heatmap color scale (iOS green ramp):**
```js
function retentionColor(v) {
    if (v >= 80) return '#34c759'
    if (v >= 60) return '#30d158'
    if (v >= 40) return '#a8e6b8'
    if (v >= 20) return '#d1f5db'
    if (v > 0)   return '#f0faf2'
    return '#f2f2f7'
}
```

**Anomaly markers on growth chart:**
```js
anomalies.forEach(a => {
    const idx = labels.indexOf(a.date)
    if (idx >= 0) pointBackgroundColors[idx] = '#ff3b30' // iOS red
})
```

---

## Phase 3 — Agent Analysis Layer

After `analyze.py` runs, Agent reads the JSON output and writes the report:

### Growth Trends
- Compare latest value to WoW and MoM baselines
- Identify trend direction (accelerating / stable / declining)
- Call out any spikes or drops

### Channel Analysis
- Identify top 3 channels by volume
- Flag channels with unusually low share (potential underinvestment)
- If organic vs paid split is visible, comment on balance

### Funnel Analysis
- Identify the step with the highest drop-off rate
- Quantify: "X% of users drop between Step A and Step B"
- Suggest hypothesis for the biggest leak

### Retention Analysis (if data available)
- Day 1 / Day 7 / Day 30 retention rates
- Compare early vs recent cohorts (improving or degrading?)
- Flag if Day 7 retention < 20% (industry concern threshold)

### Anomaly Explanation
- For each flagged date: what was the metric value vs expected?
- Cross-reference with other metrics (did sessions spike too? or just users?)
- Suggest possible causes (campaign, seasonality, data issue)

### Recommendations (always exactly 3)
Format:
```markdown
1. **[Priority]** [Action] — [Expected outcome]
2. **[Priority]** [Action] — [Expected outcome]
3. **[Priority]** [Action] — [Expected outcome]
```

---

## Phase 4 — Deliver

```markdown
## ✅ Product Analytics Dashboard Ready

**Source:** <GA4 / Mixpanel / Amplitude / Generic>
**Data Range:** <start> → <end>  (<N> days)
**Charts generated:** <list based on capabilities detected>

### 📊 Dashboard
`dashboard.html` — open in any browser, fully self-contained

### 📝 Report
`report.md`

### Key Findings
1. <most important insight>
2. <second insight>
3. <third insight>

### Anomalies Detected
<list or "None detected">

### Next Steps
- Upload a more complete dataset to unlock [missing chart]
- Compare with next week's export to track trend direction
- Share `dashboard.html` with your team — no server needed
```

---

## Implementation Notes

### Dependencies
```
pandas>=1.5
```
Install: `pip install pandas`

### Running
```bash
python3 analyze.py --input ga4_export.csv --output dashboard.html --report report.md
```

### Data Quality Handling
- Missing columns → skip that chart section, note in report
- Non-numeric values in metric columns → coerce to NaN, warn user
- Dates not parseable → try common formats (`%Y-%m-%d`, `%m/%d/%Y`, `%Y/%m/%d`)
- Fewer than 7 rows → skip anomaly detection, note insufficient data
- Empty CSV → abort with clear error message

### Privacy Note
All processing is local. CSV data never leaves the user's machine.
