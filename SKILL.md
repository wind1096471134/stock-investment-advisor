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
- **Technical signals**: key support/resistance levels, MA trend (20-day / 50-day / 200-day), RSI or other momentum indicators, 52-week high/low proximity
- **Capital flow / money flow**: recent ~1W institutional flow, north-bound/south-bound flow (for A-shares/HK), margin trading activity, large order flow direction
- **Major events**: past month earnings, products, litigation, etc.
- **Policy / regulatory changes**: past 1–2 months industry-specific policies, tariffs, subsidies, antitrust actions, or geopolitical events affecting the sector
- **Earnings calendar**: upcoming earnings date (if any)
- **Key financials**: revenue growth, PE, gross margin (for peer comparison, max 3 items)
- **Valuation percentile**: PE / PB compared to historical range (e.g., "PE at 5-year XX% percentile")
- **Strategic relationships**: major shareholders, strategic investors, and portfolio companies (cross-sector relationships are easy to miss — search explicitly)

> Note: exact price/change figures may not be precise. Use approximate values labeled "~" with query date. Trend and relative value matter more than precision.

### 2.2 Peer Company Selection (two tiers)

Follow [references/peer-selection.md](references/peer-selection.md) rules. Select peers in **two tiers:**

**Tier 1 — Primary (1–3):** Direct competitors / same-sector leaders
1. Top-3 by market cap in same sector **globally (across A-share/HK/US markets)**
2. Companies with recent or upcoming earnings reports

**Tier 2 — Supplementary (0–3, optional):** Strongly correlated upstream/downstream/investor peers
3. Key upstream suppliers (critical component providers)
4. Key downstream customers (major buyers driving target's orders)
5. **Strategic investors / investees** (shareholding relationships, even across different sectors)

> ⚠️ **Don't limit to the target's listing market.** If analyzing DELL (US), also check Lenovo (HK). If analyzing 腾讯 (HK), also check Meta (US). Use market-agnostic searches like "PC market share ranking" — not "US PC stocks".
>
> ⚠️ **Don't include weak correlations.** Tier 2 is optional — only include if the link is strong and measurable. If no strong supplementary link exists, keep it at Tier 1 only.

Exclude: ST, delisting risk, illiquid small-caps.

### 2.3 Peer Company Deep Dive

For each peer (parallel via SubAgent if multiple):

- ~1–2W price trend + ~1M fundamental changes
- Key financials (revenue growth, gross margin, PE/PB, debt ratio — from latest earnings, max 3 items)
- **⚠️ Business segment decomposition**: search `{peer} revenue breakdown by segment` and `{peer} operating profit by division`. Identify the peer's real profit driver — it may differ from the segment that overlaps with the target. Flag any divergence.
- Product/market dynamics (market share, orders)
- **Policy / regulatory impact**: recent 1–2 months sector-specific policies that affect this peer
- Earnings status: already reported (beat/miss) / upcoming (date)
- **Market**: A-share / HK / US + exchange

### 2.4 Bidirectional Prediction (earnings-centric)

Focus on **earning season signal transmission** between target and peers:

| Direction | Focus | Logic |
|-----------|-------|-------|
| **Peer (已发财报) → Target (将发财报)** | **Peers that already reported** — their results (beat/miss), stock reaction, and guidance changes → signal what to expect for the **target's upcoming earnings**. Most useful when target is about to report. | Peer's earnings beat/miss → infer target's likely outcome. Peer's stock price reaction → infer market sentiment for the sector. |
| **Target (近期变动) → Peer (将发财报)** | **Peers with upcoming earnings** — the target's recent catalysts (product launches, policy wins, demand signals) → provide reference for what peers might report. Most useful when peers are about to report. | Target's strong quarter → suggest peers may also beat. Target's weak guidance → peers likely face same headwinds. |

For **non-earnings periods**, fall back to general correlation logic:
- **Multi-relationship peers** (e.g., competitor + supplier): identify which relationship dominates the peer's stock reaction
- **⚠️ Profit driver divergence**: if the peer's profit driver differs from the overlapping segment with the target, the peer may move independently or even opposite — flag this explicitly.

Each prediction must include:

- **Direction**: Bullish / Bearish / Neutral
- **Confidence**: High / Medium / Low (with 1-line rationale)
- **Key catalyst**: specific event + timing
- **Counter-argument**: at least 1 item
- **Required peer fields**: ticker + market (A-share/HK/US) + earnings date

---

## Step 3: Output Report

Strictly follow [output-template.md](output-template.md). Report structure:

1. **Executive Summary (top)** — recommendation, core logic, catalysts, risks, **probability-weighted scenarios, associated stock predictions**
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
