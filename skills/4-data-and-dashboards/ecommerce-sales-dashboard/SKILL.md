---
name: ecommerce-sales-dashboard
description: |
  Upload an order CSV from Shopify, WooCommerce, 淘宝/天猫, 京东, or any e-commerce platform.
  Agent generates a complete sales dashboard: GMV trend, AOV distribution, repeat purchase rate,
  SKU ranking, geographic distribution, refund analysis — as interactive HTML + Markdown report.
  Designed for indie stores and small e-commerce teams.
  Triggers:
  - "sales dashboard"
  - "ecommerce dashboard"
  - "order analysis"
  - "Shopify analytics"
  - "GMV analysis"
  - "SKU ranking"
  - "repeat purchase"
  - "电商数据"
  - "销售看板"
  - "订单分析"
  - "复购率"
  - "GMV趋势"
  - "SKU排行"
---

# SKILL.md — E-commerce Sales Dashboard

> Order CSV in → full sales intelligence dashboard out.
> GMV trends, AOV, repeat purchase rate, SKU ranking, geographic heatmap.

---

## Overview

**Pipeline (4 Phases):**
```
Phase 0 — Ingest      → Detect platform, normalize order schema
Phase 1 — Analyze     → python3 analyze.py → all sales metrics computed
Phase 2 — Visualize   → HTML dashboard (Chart.js, single file)
Phase 3 — Deliver     → Markdown report + dashboard
```

---

## Phase 0 — Ingest & Platform Detection

### 0.1 Supported Platforms

| Platform | Signature Columns |
|----------|------------------|
| **Shopify** | `Name`, `Email`, `Financial Status`, `Fulfillment Status`, `Total`, `Discount Amount`, `Created at`, `Lineitem name`, `Lineitem quantity`, `Billing City`, `Billing Country` |
| **WooCommerce** | `Order ID`, `Order Status`, `Order Date`, `Order Total`, `Customer Email`, `Billing City`, `Product Name`, `Quantity` |
| **淘宝/天猫** | `订单编号`, `买家`, `商品名称`, `商品数量`, `订单金额`, `付款时间`, `收货地址`, `订单状态` |
| **京东** | `订单号`, `商品名称`, `数量`, `实付金额`, `下单时间`, `收货人`, `省份` |
| **Generic** | order_id + date + amount + (optional: product, customer, location) |

### 0.2 Standard Schema

```python
FIELD_MAP = {
    'order_id':    ['Name','Order ID','订单编号','订单号','order_id'],
    'date':        ['Created at','Order Date','付款时间','下单时间','date'],
    'customer_id': ['Email','Customer Email','买家','customer_id'],
    'revenue':     ['Total','Order Total','订单金额','实付金额','revenue','amount'],
    'discount':    ['Discount Amount','discount','优惠金额'],
    'status':      ['Financial Status','Order Status','订单状态','status'],
    'product':     ['Lineitem name','Product Name','商品名称','product'],
    'quantity':    ['Lineitem quantity','Quantity','商品数量','数量','quantity'],
    'city':        ['Billing City','收货地址','city'],
    'province':    ['Billing Province','省份','province'],
    'country':     ['Billing Country','country'],
    'refund':      ['Refunded Amount','退款金额','refund'],
}
```

### 0.3 Order-Level vs Line-Item-Level

Some exports are one-row-per-order, others one-row-per-line-item. Detect:
```python
# If same order_id appears multiple times → line-item level
is_line_item = df['order_id'].duplicated().any()
# Aggregate to order level for GMV/AOV/repeat purchase analysis
order_df = df.groupby('order_id').agg({'revenue':'sum','date':'first','customer_id':'first',...}).reset_index()
```

---

## Phase 1 — Analysis Script

### analyze.py

```python
#!/usr/bin/env python3
"""
E-commerce Sales Dashboard Generator
Usage: python3 analyze.py --input <csv> [--compare <prev_period_csv>] --output <html> --report <md>
"""
import argparse, json
from pathlib import Path
from collections import Counter
import pandas as pd
import numpy as np

FIELD_MAP = {
    'order_id':    ['Name','Order ID','订单编号','订单号','order_id','id'],
    'date':        ['Created at','Order Date','付款时间','下单时间','date','created_at'],
    'customer_id': ['Email','Customer Email','买家','customer_id','email'],
    'revenue':     ['Total','Order Total','订单金额','实付金额','revenue','amount','total'],
    'discount':    ['Discount Amount','discount','优惠金额'],
    'status':      ['Financial Status','Order Status','订单状态','status'],
    'product':     ['Lineitem name','Product Name','商品名称','product','product_name'],
    'quantity':    ['Lineitem quantity','Quantity','商品数量','数量','quantity','qty'],
    'province':    ['Billing Province','省份','province','region'],
    'country':     ['Billing Country','country'],
    'refund':      ['Refunded Amount','退款金额','refund'],
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
    if 'revenue' in df.columns:
        df['revenue'] = pd.to_numeric(
            df['revenue'].astype(str).str.replace(r'[$,¥￥€£]','',regex=True), errors='coerce'
        ).fillna(0)
    if 'quantity' in df.columns:
        df['quantity'] = pd.to_numeric(df['quantity'], errors='coerce').fillna(1)
    if 'date' in df.columns:
        df['date'] = pd.to_datetime(df['date'], errors='coerce')
    return df

def get_order_df(df):
    """Collapse to order level"""
    if 'order_id' not in df.columns: return df
    agg = {'revenue': 'sum', 'date': 'first'}
    if 'customer_id' in df.columns: agg['customer_id'] = 'first'
    if 'province' in df.columns:    agg['province'] = 'first'
    if 'status' in df.columns:      agg['status'] = 'first'
    return df.groupby('order_id').agg(agg).reset_index()

def compute_gmv_trend(order_df):
    if 'date' not in order_df.columns: return {}
    daily = order_df.groupby(order_df['date'].dt.date).agg(
        gmv=('revenue','sum'), orders=('order_id','count')
    ).reset_index()
    daily = daily.sort_values('date')
    return {
        'labels': [str(d) for d in daily['date']],
        'gmv':    daily['gmv'].round(2).tolist(),
        'orders': daily['orders'].tolist(),
    }

def compute_kpis(order_df, compare_df=None):
    kpis = {}
    kpis['total_orders']   = int(len(order_df))
    kpis['total_gmv']      = round(float(order_df['revenue'].sum()), 2)
    kpis['aov']            = round(float(order_df['revenue'].mean()), 2)  # Average Order Value

    # Repeat purchase rate
    if 'customer_id' in order_df.columns:
        customer_orders = order_df.groupby('customer_id').size()
        repeat = (customer_orders > 1).sum()
        total_customers = len(customer_orders)
        kpis['total_customers']   = int(total_customers)
        kpis['repeat_customers']  = int(repeat)
        kpis['repeat_rate']       = round(float(repeat / max(total_customers, 1) * 100), 1)
        kpis['avg_orders_per_customer'] = round(float(customer_orders.mean()), 2)

    # Refund rate
    if 'status' in order_df.columns:
        refunded = order_df['status'].str.lower().isin(['refunded','refund','退款','已退款']).sum()
        kpis['refund_rate'] = round(float(refunded / max(len(order_df), 1) * 100), 1)

    return kpis

def sku_ranking(df, top_n=10):
    if 'product' not in df.columns: return []
    qty_col = 'quantity' if 'quantity' in df.columns else None
    if qty_col:
        sku = df.groupby('product').agg(units=('quantity','sum'), revenue=('revenue','sum')).reset_index()
    else:
        sku = df.groupby('product').agg(units=('order_id','count'), revenue=('revenue','sum')).reset_index()
    sku['revenue'] = sku['revenue'].round(2)
    return sku.sort_values('revenue', ascending=False).head(top_n).to_dict('records')

def aov_distribution(order_df, bins=8):
    if 'revenue' not in order_df.columns: return {}
    vals = order_df['revenue'].dropna()
    counts, edges = np.histogram(vals, bins=bins)
    labels = [f'¥{int(edges[i])}–{int(edges[i+1])}' for i in range(len(edges)-1)]
    return {'labels': labels, 'counts': counts.tolist()}

def geo_breakdown(order_df, top_n=15):
    col = 'province' if 'province' in order_df.columns else 'country' if 'country' in order_df.columns else None
    if not col: return []
    geo = order_df.groupby(col).agg(
        orders=('order_id','count'), gmv=('revenue','sum')
    ).reset_index().sort_values('gmv', ascending=False).head(top_n)
    geo['gmv'] = geo['gmv'].round(2)
    return geo.rename(columns={col: 'region'}).to_dict('records')

def rfm_segments(order_df):
    """Simple RFM segmentation"""
    if not all(c in order_df.columns for c in ['customer_id','date','revenue']):
        return {}
    now = order_df['date'].max()
    rfm = order_df.groupby('customer_id').agg(
        recency=('date', lambda x: (now - x.max()).days),
        frequency=('order_id', 'count'),
        monetary=('revenue', 'sum')
    ).reset_index()
    # Simple scoring: 1–4 quartile
    for col in ['recency','frequency','monetary']:
        rfm[f'{col}_score'] = pd.qcut(rfm[col], 4, labels=[4,3,2,1] if col=='recency' else [1,2,3,4], duplicates='drop')
    rfm['rfm_score'] = rfm[['recency_score','frequency_score','monetary_score']].astype(float).sum(axis=1)
    segments = {
        'champions':      int((rfm['rfm_score'] >= 11).sum()),
        'loyal':          int(((rfm['rfm_score'] >= 8) & (rfm['rfm_score'] < 11)).sum()),
        'at_risk':        int(((rfm['rfm_score'] >= 5) & (rfm['rfm_score'] < 8)).sum()),
        'lost':           int((rfm['rfm_score'] < 5).sum()),
    }
    return segments

def wow_comparison(curr, prev):
    changes = {}
    for k in ['total_gmv','total_orders','aov','repeat_rate']:
        if k in curr and k in prev and prev[k] and prev[k] != 0:
            changes[k] = round((curr[k] - prev[k]) / abs(prev[k]) * 100, 1)
    return changes

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('--input',   required=True)
    parser.add_argument('--compare', default=None)
    parser.add_argument('--output',  default='sales-dashboard.html')
    parser.add_argument('--report',  default='sales-report.md')
    args = parser.parse_args()

    df = normalize(pd.read_csv(args.input))
    order_df = get_order_df(df)
    kpis = compute_kpis(order_df)
    wow = {}
    if args.compare:
        df_prev = normalize(pd.read_csv(args.compare))
        wow = wow_comparison(kpis, compute_kpis(get_order_df(df_prev)))

    date_range = []
    if 'date' in order_df.columns:
        dates = order_df['date'].dropna()
        if len(dates):
            date_range = [str(dates.min().date()), str(dates.max().date())]

    data = {
        'kpis':        kpis,
        'wow':         wow,
        'gmv_trend':   compute_gmv_trend(order_df),
        'sku_ranking': sku_ranking(df),
        'aov_dist':    aov_distribution(order_df),
        'geo':         geo_breakdown(order_df),
        'rfm':         rfm_segments(order_df),
        'date_range':  date_range,
        'rows':        len(df),
    }

    data_json = json.dumps(data, ensure_ascii=False, default=str)
    tpl_path = Path(__file__).parent / 'sales-dashboard-template.html'
    if tpl_path.exists():
        html = tpl_path.read_text().replace('__DASHBOARD_DATA__', data_json)
    else:
        html = f'<script>window.DASHBOARD_DATA={data_json}</script>'
    Path(args.output).write_text(html)

    report_skeleton = f"""# Sales Dashboard Report
**Period:** {date_range[0] if date_range else 'N/A'} → {date_range[-1] if date_range else 'N/A'}

## Core KPIs
| Metric | Value | WoW |
|--------|-------|-----|
| GMV | {kpis.get('total_gmv', 0):,.2f} | {wow.get('total_gmv','—')}% |
| Orders | {kpis.get('total_orders', 0):,} | {wow.get('total_orders','—')}% |
| AOV | {kpis.get('aov', 0):,.2f} | {wow.get('aov','—')}% |
| Customers | {kpis.get('total_customers', 'N/A')} | |
| Repeat Rate | {kpis.get('repeat_rate', 'N/A')}% | {wow.get('repeat_rate','—')}% |
| Refund Rate | {kpis.get('refund_rate', 'N/A')}% | |

## GMV Trend Analysis
<!-- Agent: describe trend, peaks, dips -->

## SKU Performance
<!-- Agent: top sellers, slow movers, revenue concentration risk -->

## Customer Analysis
<!-- Agent: repeat rate commentary, RFM segment distribution -->

## Geographic Insights
<!-- Agent: top regions, concentration analysis -->

## Recommendations
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
│  HEADER: Store name · Period · Order count badge             │
│  bg: #f2f2f7                                                 │
├────────┬────────┬────────┬────────┬──────────────────────────┤
│  GMV   │ Orders │  AOV   │Repeat% │  Refund Rate             │
│ [1×1]  │ [1×1]  │ [1×1]  │ [1×1]  │  [1×1]                  │
│ WoW ↑↓ │ WoW ↑↓ │        │        │  red if > 5%            │
├────────┴────────┴────────┴────────┴──────────────────────────┤
│  📈 GMV Trend [4×1] dual-axis: GMV bar (#007aff) + Orders line (#34c759) │
├───────────────────────┬──────────────────────────────────────┤
│  🏆 SKU Ranking [2×1]  │  🗺️ Geographic Breakdown [2×1]       │
│  horizontal bar        │  ranked list, province + GMV         │
├───────────────────────┼──────────────────────────────────────┤
│  📊 AOV Distribution  │  👥 RFM Segments [1×1]                │
│  [1×1] histogram       │  donut chart                         │
└───────────────────────┴──────────────────────────────────────┘
```

Cards: `border-radius: 22px`, `background: #fff`, `box-shadow: 0 4px 16px rgba(0,0,0,0.06)`

**Design tokens (Apple / Bento Grid):**
```css
--bg: #f2f2f7; --surface: #ffffff; --accent: #007aff;
--accent-2: #34c759; --danger: #ff3b30; --warning: #ffcc00;
--text: #1c1c1e; --text-muted: #8e8e93; --radius: 22px;
--font: -apple-system, 'SF Pro Display', 'Helvetica Neue', sans-serif;
```

**RFM segment colors (iOS system):**
```js
const RFM_COLORS = {
    champions: '#34c759',  // iOS green
    loyal:     '#007aff',  // iOS blue
    at_risk:   '#ff9500',  // iOS orange
    lost:      '#ff3b30',  // iOS red
}
```

**Chart colors (iOS system):**
```js
const COLORS = ['#007aff','#34c759','#ff9500','#ff3b30','#af52de','#5ac8fa','#ff2d55','#ffcc00']
```

---

## Phase 3 — Agent Analysis Layer

### GMV Commentary
- Overall trend: growing / stable / declining
- Identify peak days (weekend effect? campaign days?)
- Revenue concentration: what % of GMV came from top 20% of orders?

### SKU Intelligence
- Top 3 by revenue vs top 3 by volume (they're often different)
- Revenue concentration risk: if top SKU = >50% of GMV, flag dependency
- Slow movers: bottom 20% of SKUs by revenue — candidate for discontinuation

### Customer Health
- Repeat rate vs industry benchmark (~15-30% for typical e-commerce)
- RFM segment commentary: what % are Champions? Is At-risk pool large?
- If repeat rate < 15%: flag acquisition cost concern, recommend retention actions

### Geographic Insights
- Top 3 provinces/regions by GMV
- Any region with high order count but low AOV (potential price-sensitive segment)

### Recommendations (3)
```
1. **Retention** 复购率 X% 低于行业均值，建议对"At Risk"客群（N人）发送召回优惠
2. **SKU策略** [SKU名] 占 GMV X%，集中度过高，建议推广 [第2名SKU] 分散风险
3. **地域机会** [省份] 订单量第X但 AOV 最高，建议针对该地域加大推广投入
```

---

## Implementation Notes

**Dependencies:** `pandas>=1.5`, `numpy>=1.21`

**Currency handling:** Strip ¥, $, €, £, ￥, commas before parsing. Detect currency from column headers or first value.

**Date formats handled:** `%Y-%m-%d`, `%Y/%m/%d`, `%m/%d/%Y`, `%Y-%m-%d %H:%M:%S`, `%Y年%m月%d日`

**Shopify specifics:** Shopify exports one row per line item; `order_id` = `Name` column (e.g., `#1234`). Must aggregate to order level before computing AOV/repeat rate.

**Privacy note:** Customer emails are used only for repeat purchase calculation, never displayed in output.
