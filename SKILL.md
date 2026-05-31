---
name: stock-investment-advisor
description: >
  Analyze stocks (A-shares/HK/US) using WebSearch only — no API keys,
  no Python packages, no paid data sources. Provides 1-2 week directional
  outlooks based on publicly available information.
---

# Stock Investment Advisor

Analyze listed companies using **public web information only** (WebSearch / WebFetch).
Deliver a 1–2 week directional outlook. No third-party API, no paid data source, no Python library (yfinance, akshare, etc.).

---

## Data Source Principles

1. **WebSearch / WebFetch only** — obtain everything from public web pages. No third-party financial API.
2. **Plain text search** — no Python packages, no proprietary data interfaces.
3. **Regional accessibility** — different sources have different reachability depending on network region. Note alternatives (see [references/data-sources.md](references/data-sources.md)).
4. **Honest labeling** — if data cannot be obtained via public search, mark it as "Not available." Never guess, fabricate, or fall back to an API.

---

## Interaction Rules

- Guidance: short phrases + numbered options, at most 2 questions per turn
- Analysis: **conclusion first** (executive summary → predictions → detailed analysis), no repetition
- Language detection:
  - User writes in Chinese → full Chinese workflow (Chinese search queries + Chinese output)
  - User writes in English → full English workflow (English search queries + English output)
- Data gaps: mark as "Not available" / 「未获取到」, never fabricate
- At most 3 key financial figures per company
- Forbidden: specific buy/sell price levels. Use **Bullish / Bearish / Neutral** instead of Buy / Sell
- **Required peer fields**: every peer stock must include ticker, market (A-share / HK / US), and earnings status (already reported / upcoming + date)

---

## Flow Overview

```
User input
  ├─ 1–3 stocks given → Analysis flow
  ├─ >3 stocks → Ask to narrow to 3
  └─ No stock → Guidance flow → Analysis flow
```

---

## Step 0: Entry Routing

**Stock identification**:
- A-shares: 6-digit code (e.g. `600519`)
- HK stocks: 5-digit code or `.HK` suffix (e.g. `00700`, `09988.HK`)
- US stocks: 1–5 letter ticker (e.g. `AAPL`, `NVDA`)
- Chinese company name (e.g. `贵州茅台`, `腾讯`)

| Condition | Action |
|-----------|--------|
| 1–3 stocks | Skip guidance, enter analysis |
| >3 stocks | Ask user to narrow to 3 |
| No stock | Enter guidance flow |

---

## Step 1: Guidance Flow

**Goal**: Determine 1–3 stocks to analyze. At most 2 rounds.

### 1.1 Round 1 (AskUserQuestion)

Collect in one go (auto-fill from context, don't re-ask):

1. **Market**: A-shares / HK / US / no preference
2. **Focus**: Specific sector / concept / industry chain / not sure

If user directly gives stock names/codes → end guidance immediately.

### 1.2 Round 2 (only if "not sure")

1. WebSearch for hot sectors/concepts in that market over the past month (search templates in [references/data-sources.md](references/data-sources.md))
2. Present **5–8 options** (sector name + 1-line reason for popularity)
3. User picks a sector → list **5–10 top stocks** for them to choose 1–3

### 1.3 Confirmation

One-line confirmation, e.g.: "Analysis target: 600519 贵州茅台, 002594 比亚迪. Proceed?"

---

## Parallel Strategy & SubAgent Usage

**Core principle**: Use SubAgents (Agent tool) to parallelize data collection when analyzing multiple stocks or peer companies.

### SubAgent Applicability

| Scenario | Parallelism | Description |
|----------|-------------|-------------|
| Target stock snapshot (per stock) | 1 SubAgent each | Parallel: price, events, earnings date |
| Peer company deep dive (per peer) | 1 SubAgent each | Parallel: fundamentals, price action, news |
| Sector/industry chain search | 1 SubAgent | Can search upstream/downstream independently |

### SubAgent Workflow

```
Main Agent (me)
  ├─ SubAgent 1: Target stock A data (WebSearch)
  ├─ SubAgent 2: Target stock B data (WebSearch)
  ├─ SubAgent 3: Sector/industry chain search
  └─ SubAgent 4: Peer company data
```

- **Each SubAgent prompt must be explicit**: what keywords to search, what fields to extract, where to search
- **All SubAgents launch and collect in parallel**; Main Agent waits for all to finish before synthesis
- Each SubAgent returns a structured summary (not a full report)
- If a SubAgent encounters data that cannot be obtained via public search, mark it as missing — never fabricate

### Notes

- SubAgents **cannot** nest further SubAgents (1 level only)
- Simple 1–2 stock analysis does not need SubAgents — serial WebSearch is fine
- **SubAgents only when ≥2 target stocks or multiple peer companies**

---

## Step 2: Analysis Flow

### 2.1 Target Stock Snapshot

For each target stock (parallel via SubAgent if multiple):

- **Price data**: current price, ~1W / ~1M change (search stock price pages)
- **Basic info**: sector, market cap range
- **Major events**: past month earnings, policy, products, litigation, etc.
- **Earnings calendar**: upcoming earnings date (if any)
- **Key financials**: revenue growth, PE, gross margin (for peer comparison, max 3 items)

> Note: exact price/change figures may not be precise. Use approximate values labeled "~" with query date. Trend and relative value matter more than precision.

### 2.2 Peer Company Selection (1–3 peers)

Follow [references/peer-selection.md](references/peer-selection.md) rules:

1. Top-3 by market cap in same sector, with abnormal ~1M price/volume
2. Industry chain leaders (suppliers / customers / competitors)
3. Companies with recent or upcoming earnings reports

Exclude: ST, delisting risk, illiquid small-caps.

### 2.3 Peer Company Deep Dive

For each peer (parallel via SubAgent if multiple):

- ~1–2W price trend + ~1M fundamental changes
- Key financials (revenue growth, gross margin, PE/PB, debt ratio — from latest earnings, max 3 items)
- Product/market dynamics (market share, orders, policy impact)
- Earnings status: already reported (beat/miss) / upcoming (date)
- **Market**: A-share / HK / US + exchange

### 2.4 Bidirectional Prediction (1–2 week window)

| Direction | Logic |
|-----------|-------|
| **Peer → Target** | Peer's recent price moves, earnings beats/misses, sector sentiment → infer target follow/diverge/lag |
| **Target → Peer** | Target catalyst → infer which peers may be affected; flag those with upcoming earnings |

Each prediction must include:

- **Direction**: Bullish / Bearish / Neutral
- **Confidence**: High / Medium / Low (with 1-line rationale)
- **Key catalyst**: specific event + timing
- **Counter-argument**: at least 1 item
- **Required peer fields**: ticker + market (A-share/HK/US) + earnings date

---

## Step 3: Output Report

Strictly follow [output-template.md](output-template.md). Report structure:

1. **Executive Summary (top)** — recommendation, core logic, catalysts, risks
2. **Prediction Summary** — target + peer predictions with market & earnings dates
3. **Target Stock Snapshot** — price, valuation, recent events
4. **Peer Deep Dive** — peer data + financial comparison table
5. **Bidirectional Logic** — peer→target and target→peer analysis

The report **must** end with this risk disclaimer (verbatim):

> This report is compiled from publicly available information for research reference only and **does not constitute investment advice**. Stock markets involve significant risk. Short-term price movements are influenced by policies, capital flows, sentiment, and many other factors. Predictions carry substantial uncertainty. Please exercise independent judgment and make cautious decisions.

---

## Reference Files

- [output-template.md](output-template.md) — report template with example
- [references/data-sources.md](references/data-sources.md) — public data sources, search templates, regional accessibility
- [references/peer-selection.md](references/peer-selection.md) — peer company selection rules
