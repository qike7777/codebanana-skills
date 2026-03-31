---
name: content-analytics-report
description: |
  Paste or upload weekly content performance data from YouTube, 小红书 (Xiaohongshu),
  WeChat Official Account, Instagram, or TikTok. Agent generates a structured weekly report:
  publish volume, views/plays, follower growth, engagement rate, best posting times,
  top/bottom content — with Markdown weekly report + HTML chart dashboard.
  Optional weekly cron for automatic reports.
  Triggers:
  - "content report"
  - "content analytics"
  - "YouTube analytics"
  - "小红书数据"
  - "公众号数据"
  - "content weekly report"
  - "social media analytics"
  - "内容周报"
  - "内容数据分析"
  - "涨粉分析"
  - "最佳发布时间"
---

# SKILL.md — Content Analytics Weekly Report

> Your weekly content performance report, automated.
> Paste data from any platform → Markdown weekly report + HTML dashboard.

---

## Overview

**Pipeline (4 Phases):**
```
Phase 0 — Ingest      → Detect platform, normalize to standard content schema
Phase 1 — Analyze     → python3 analyze.py → content metrics, trends, best time
Phase 2 — Visualize   → HTML dashboard (Chart.js, single file)
Phase 3 — Deliver     → Markdown weekly report + dashboard
```

---

## Phase 0 — Ingest & Platform Detection

### 0.1 Supported Platforms

| Platform | Signature Columns |
|----------|------------------|
| **YouTube Studio** | `Content`, `Video publish time`, `Views`, `Watch time (hours)`, `Subscribers`, `Impressions`, `CTR` |
| **小红书** | `笔记标题`, `发布时间`, `浏览量`, `点赞数`, `收藏数`, `评论数`, `涨粉数` |
| **公众号** | `标题`, `发布时间`, `阅读数`, `在看数`, `分享数`, `关注人数` |
| **TikTok** | `Video Title`, `Post Time`, `Video Views`, `Likes`, `Comments`, `Shares`, `New Followers` |
| **Instagram** | `Description`, `Post Type`, `Publish Time`, `Reach`, `Impressions`, `Likes`, `Comments`, `Shares`, `Follows` |
| **Generic** | date/title + any engagement metrics |

### 0.2 Standard Schema

```python
FIELD_MAP = {
    'title':       ['Content','Video Title','笔记标题','标题','Description','title'],
    'publish_time':['Video publish time','发布时间','Post Time','Publish Time','publish_time'],
    'views':       ['Views','浏览量','阅读数','Video Views','Reach','views'],
    'likes':       ['Likes','点赞数','在看数','likes'],
    'comments':    ['Comments','评论数','comments'],
    'shares':      ['Shares','分享数','shares'],
    'saves':       ['收藏数','Saves','saves'],
    'followers_gained': ['Subscribers','涨粉数','New Followers','Follows','followers_gained'],
    'impressions': ['Impressions','impressions'],
    'watch_time':  ['Watch time (hours)','watch_time'],
    'ctr':         ['CTR','ctr'],
    'post_type':   ['Post Type','Content Type','post_type'],
}
```

### 0.3 Computed Metrics

```python
# Engagement rate = (likes + comments + shares + saves) / views * 100
df['engagement_rate'] = (
    df[['likes','comments','shares','saves']].fillna(0).sum(axis=1)
    / df['views'].replace(0, np.nan) * 100
).round(2)

# Hour of day from publish_time
df['hour'] = pd.to_datetime(df['publish_time'], errors='coerce').dt.hour
df['weekday'] = pd.to_datetime(df['publish_time'], errors='coerce').dt.day_name()
```

---

## Phase 1 — Analysis Script

### analyze.py

```python
#!/usr/bin/env python3
"""
Content Analytics Weekly Report Generator
Usage: python3 analyze.py --input <csv> [--compare <prev_week_csv>] --output <html> --report <md>
"""
import argparse, json
from pathlib import Path
import pandas as pd
import numpy as np

FIELD_MAP = {
    'title':            ['Content','Video Title','笔记标题','标题','Description','title'],
    'publish_time':     ['Video publish time','发布时间','Post Time','Publish Time','publish_time'],
    'views':            ['Views','浏览量','阅读数','Video Views','Reach','views'],
    'likes':            ['Likes','点赞数','在看数','likes'],
    'comments':         ['Comments','评论数','comments'],
    'shares':           ['Shares','分享数','shares'],
    'saves':            ['收藏数','Saves','saves'],
    'followers_gained': ['Subscribers','涨粉数','New Followers','Follows','followers_gained'],
    'impressions':      ['Impressions','impressions'],
    'watch_time':       ['Watch time (hours)','watch_time'],
    'ctr':              ['CTR','ctr'],
    'post_type':        ['Post Type','Content Type','post_type'],
}

def detect_platform(df):
    cols = set(df.columns)
    if '笔记标题' in cols or '浏览量' in cols: return 'xiaohongshu'
    if '阅读数' in cols or '在看数' in cols:   return 'wechat'
    if 'Watch time (hours)' in cols:           return 'youtube'
    if 'Video Views' in cols:                  return 'tiktok'
    return 'generic'

def normalize(df):
    rename = {}
    col_lower = {c.lower().strip(): c for c in df.columns}
    for target, sources in FIELD_MAP.items():
        for s in sources:
            if s.lower() in col_lower and target not in rename.values():
                rename[col_lower[s.lower()]] = target
                break
    df = df.rename(columns=rename)
    for col in ['views','likes','comments','shares','saves','followers_gained','impressions']:
        if col in df.columns:
            df[col] = pd.to_numeric(df[col].astype(str).str.replace(',',''), errors='coerce').fillna(0)
    return df

def compute_summary(df):
    summary = {'post_count': len(df)}
    for col in ['views','likes','comments','shares','saves','followers_gained']:
        if col in df.columns:
            summary[f'total_{col}'] = int(df[col].sum())
            summary[f'avg_{col}']   = round(float(df[col].mean()), 1)
    # Engagement rate
    eng_cols = [c for c in ['likes','comments','shares','saves'] if c in df.columns]
    if eng_cols and 'views' in df.columns:
        df['_eng'] = df[eng_cols].sum(axis=1)
        summary['avg_engagement_rate'] = round(
            float((df['_eng'] / df['views'].replace(0, np.nan)).mean() * 100), 2
        )
    return summary

def best_posting_time(df):
    """Find hours and weekdays with highest avg views"""
    if 'publish_time' not in df.columns or 'views' not in df.columns:
        return {}
    df = df.copy()
    df['_dt'] = pd.to_datetime(df['publish_time'], errors='coerce')
    df['_hour']    = df['_dt'].dt.hour
    df['_weekday'] = df['_dt'].dt.day_name()
    by_hour    = df.groupby('_hour')['views'].mean().round(0).sort_values(ascending=False)
    by_weekday = df.groupby('_weekday')['views'].mean().round(0).sort_values(ascending=False)
    return {
        'best_hours':    by_hour.head(3).reset_index().rename(columns={'_hour':'hour','views':'avg_views'}).to_dict('records'),
        'best_weekdays': by_weekday.head(3).reset_index().rename(columns={'_weekday':'weekday','views':'avg_views'}).to_dict('records'),
        'hour_heatmap':  by_hour.reindex(range(24), fill_value=0).tolist(),
    }

def top_content(df, n=5):
    if 'views' not in df.columns: return [], []
    df = df.copy()
    top = df.nlargest(n, 'views')[['title','views'] + [c for c in ['likes','shares','followers_gained','engagement_rate'] if c in df.columns]]
    bot = df.nsmallest(n, 'views')[['title','views'] + [c for c in ['likes','shares','followers_gained'] if c in df.columns]]
    return top.fillna(0).to_dict('records'), bot.fillna(0).to_dict('records')

def content_type_breakdown(df):
    if 'post_type' not in df.columns: return []
    breakdown = df.groupby('post_type').agg(
        count=('title','count'),
        avg_views=('views','mean')
    ).round(1).reset_index()
    return breakdown.to_dict('records')

def follower_trend(df):
    if 'publish_time' not in df.columns or 'followers_gained' not in df.columns:
        return {}
    df = df.copy()
    df['_date'] = pd.to_datetime(df['publish_time'], errors='coerce').dt.date
    daily = df.groupby('_date')['followers_gained'].sum().reset_index()
    daily = daily.sort_values('_date')
    return {
        'labels': [str(d) for d in daily['_date']],
        'values': daily['followers_gained'].tolist(),
    }

def wow_comparison(curr, prev):
    changes = {}
    for k in curr:
        if k in prev and prev[k] and prev[k] != 0:
            changes[k] = round((curr[k] - prev[k]) / abs(prev[k]) * 100, 1)
    return changes

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('--input',   required=True)
    parser.add_argument('--compare', default=None)
    parser.add_argument('--output',  default='content-report.html')
    parser.add_argument('--report',  default='content-report.md')
    args = parser.parse_args()

    df = normalize(pd.read_csv(args.input))
    # Compute engagement rate
    eng_cols = [c for c in ['likes','comments','shares','saves'] if c in df.columns]
    if eng_cols and 'views' in df.columns:
        df['engagement_rate'] = (df[eng_cols].sum(axis=1) / df['views'].replace(0, np.nan) * 100).round(2)

    platform = detect_platform(pd.read_csv(args.input))
    summary  = compute_summary(df)
    top, bot = top_content(df)

    wow = {}
    if args.compare:
        df_prev = normalize(pd.read_csv(args.compare))
        wow = wow_comparison(summary, compute_summary(df_prev))

    data = {
        'platform':      platform,
        'summary':       summary,
        'wow':           wow,
        'best_time':     best_posting_time(df),
        'top_content':   top,
        'bottom_content':bot,
        'content_types': content_type_breakdown(df),
        'follower_trend':follower_trend(df),
        'date_range':    [],
    }
    if 'publish_time' in df.columns:
        dates = pd.to_datetime(df['publish_time'], errors='coerce').dropna()
        if len(dates):
            data['date_range'] = [str(dates.min().date()), str(dates.max().date())]

    data_json = json.dumps(data, ensure_ascii=False, default=str)
    tpl_path = Path(__file__).parent / 'content-report-template.html'
    if tpl_path.exists():
        html = tpl_path.read_text().replace('__DASHBOARD_DATA__', data_json)
    else:
        html = f'<script>window.DASHBOARD_DATA={data_json}</script>'
    Path(args.output).write_text(html)

    report_skeleton = f"""# Content Weekly Report
**Platform:** {platform.upper()}  
**Period:** {data['date_range'][0] if data['date_range'] else 'N/A'} → {data['date_range'][-1] if data['date_range'] else 'N/A'}

## 本周概览
- 发布内容：{summary.get('post_count', 0)} 篇
- 总浏览量：{summary.get('total_views', 0):,}
- 平均互动率：{summary.get('avg_engagement_rate', 0):.2f}%
- 总涨粉：{summary.get('total_followers_gained', 0):,}

## 最佳发布时间
<!-- Agent: describe best hours and weekdays -->

## 爆款内容 Top 5
<!-- Agent: analyze what the top posts have in common -->

## 待优化内容
<!-- Agent: what do low-performing posts share -->

## 内容类型表现
<!-- Agent: compare post types -->

## 本周洞察与建议
<!-- Agent: 3 actionable recommendations -->
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
│  HEADER: Platform badge · Week · Post Count                  │
│  bg: #f2f2f7                                                 │
├──────────┬──────────┬──────────┬──────────┬─────────────────┤
│  Views   │  Likes   │  Shares  │  Saves   │  Followers      │
│  [1×1]   │  [1×1]   │  [1×1]   │  [1×1]   │  [1×1] +XXX    │
│  WoW ↑↓  │          │          │          │  #34c759        │
├──────────┴──────────┴──────────┴──────────┴─────────────────┤
│  📈 Follower Growth Trend [4×1] area chart                   │
├────────────────────────┬─────────────────────────────────────┤
│  🕐 Best Posting Time  │  🏆 Top 5 Content [1×2]              │
│  [1×2] 24h bar chart   │  ranked list: title + views + badge │
├────────────────────────┴─────────────────────────────────────┤
│  📊 Content Type Breakdown [4×1] grouped bar                 │
└──────────────────────────────────────────────────────────────┘
```

Cards: `border-radius: 22px`, `background: #fff`, `box-shadow: 0 4px 16px rgba(0,0,0,0.06)`

**Best posting time — 24h bar chart (iOS colors):**
```js
labels = Array.from({length: 24}, (_, i) => `${i}:00`)
barColors = hourData.map((v, i) => top3Hours.includes(i) ? '#007aff' : '#e5e5ea')
```

**Design tokens (Apple / Bento Grid):**
```css
--bg: #f2f2f7; --surface: #ffffff; --accent: #007aff;
--accent-2: #34c759; --danger: #ff3b30; --text: #1c1c1e;
--text-muted: #8e8e93; --radius: 22px;
--font: -apple-system, 'SF Pro Display', 'Helvetica Neue', sans-serif;
```

**Chart colors (iOS system):**
```js
const COLORS = ['#007aff','#34c759','#ff9500','#ff3b30','#af52de','#5ac8fa','#ff2d55','#ffcc00']
```

---

## Phase 3 — Agent Analysis Layer

### Best Time Insight
- State the top 3 hours and top 2 weekdays by avg views
- Note the sample size (caveat if < 10 posts)

### Top Content Pattern
- What do the top 5 posts have in common? (topic, format, length, hook type)
- What's the engagement rate vs account average?

### Low-Performer Pattern
- What do the bottom posts share? (topic fatigue? wrong format? weak hook?)
- Is there a post type that consistently underperforms?

### Growth Commentary
- Week's follower gain vs previous week (WoW)
- Identify which posts drove the most followers (cross-reference top content)

### Recommendations (3)
```
1. **发布时间** 把下周内容集中在 <X>:00–<Y>:00 发布，历史数据显示此时段均值浏览量高 X%
2. **内容形式** <类型A> 互动率是 <类型B> 的 X 倍，建议下周增加 <类型A> 比例
3. **选题方向** Top 5 中有 X 篇涉及 <主题>，建议深挖该主题系列内容
```

---

## Phase 4 — Weekly Cron (Optional)

After first run, offer weekly automation:

```
Would you like a weekly report every Monday morning?
Drop your weekly export CSV into <workspace>/content-data/ each Monday,
and I'll generate the report automatically.
```

Cron schedule: `weekly: 1:08:00` (Monday 08:00)

Cron message:
```
Read the most recently modified CSV in <workspace>/content-data/.
Run: python3 <workspace>/scripts/analyze.py --input <latest> --compare <prev_week> --output <workspace>/reports/content-YYYY-WNN.html --report <workspace>/reports/content-YYYY-WNN.md
Summarize: total views, follower gain, top post, avg engagement rate, top recommendation.
```

---

## Implementation Notes

**Dependencies:** `pandas>=1.5`, `numpy>=1.21`

**Multi-language support:** Column headers in Chinese are explicitly handled in FIELD_MAP. Report output language follows user's language.

**Data input flexibility:** User can also paste raw data as a table in chat → Agent writes to temp CSV → runs script.
