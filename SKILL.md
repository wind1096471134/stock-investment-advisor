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
- **语言锁定规则**：
  1. **确定语言**：综合判断用户输入的语言 + 之前对话上下文的语言，确定最终输出语言
  2. **严格对齐**：语言一旦确定，整份报告必须严格使用同一种语言输出（仅允许保留英文专有名词如 ticker、Bullish/Bearish/Neutral、技术术语等）
  3. **禁止混用**：不得在同一报告内混用中英文（例如：英文报告中出现「财报传导」、中文报告中出现 "Executive Summary"）。与语言不匹配的章节标题、标注文字、表格描述都必须替换为对应语言
  4. **例外**：英文 ticker 代码（如 AAPL）、市场标注（A-share/HK/US）以及定向术语（Bullish/Bearish/Neutral）可保留原语言不变
- Language lock rule:
  1. **Determine language**: evaluate both the user's input language and the prior conversation language to lock the output language
  2. **Strict alignment**: once locked, the entire report must use that language consistently (only exceptions are English proper nouns like tickers, Bullish/Bearish/Neutral, technical terms)
  3. **No mixing**: do NOT mix languages within a single report (e.g., Chinese terms like "财报传导" in an English report, or "Executive Summary" in a Chinese report). Section headers, annotations, and table descriptions must all match the chosen language
  4. **Exceptions**: stock tickers (e.g. AAPL), market labels (A-share/HK/US), and directional terms (Bullish/Bearish/Neutral) may stay in their original form
- Data gaps: mark as "Not available" / 「未获取到」, never fabricate
- At most 3 key financial figures per company
- Forbidden: specific buy/sell price levels. Use **Bullish / Bearish / Neutral** instead of Buy / Sell
- **Required peer fields**: every peer stock must include ticker, market (A-share / HK / US), and earnings status (already reported / upcoming + date)
- **Earnings date must be precise**: label with exact date when found (e.g., "Jun 3, 2026"). If exact date not available via public search, mark as "未获取到具体日期" / "Exact date not found". Never fabricate or guess.
- **近期催化剂必须纳入预测**：预测必须搜索并列出最近 1-2 周与该股**强相关**的事件、政策、公告、行业大会、产品发布等，并列明每个事件的具体日期、内容概要及预期影响方向。示例："ASUS GTC Taipei 大会（6/1）发布 RTX Spark PC 芯片——扩展 PC 新市场，利好" / "美国商务部 5/28 对华 AI 芯片新出口限制——限制 Blackwell 销往中国实体，利空"
- **非财报期优先依赖近期催化剂**：在非财报密集期（即目标及同行均无近期财报），近期 1-2 周的事件/政策/公告应取代财报传导成为预测的核心依据，占预测信心的主导权重
- **财报传导标注**: in Associated Stock Predictions, mark each peer based on **chronological relationship** between target and peer earnings dates:

| 时序 | 传导方向 | 标注 |
|------|---------|------|
| **目标先发** (earlier) → **同行后发** (later) | 目标财报结果→同行的领先指标 | ✅ 目标先发, 对同行有传导价值 |
| **同行先发** (earlier) → **目标后发** (later) | 同行财报结果→目标的领先指标 | ✅ 同行先发, 对目标有传导价值 |
| **均无近期财报**或时期完全不重叠 | 无传导链路 | ❌ 暂无强传导 |

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
- **近期核心催化剂（1-2 周）**：识别最近 1-2 周与该股强关联的事件/政策/公告/行业会议/产品发布/重大订单/管理层变动等。搜索 `{stock} {最近事件关键词}`。列表形式呈现，每项标注：日期 + 事件概要 + 预计影响方向（利多/利空）
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

Focus on **earning season signal transmission** between target and peers. Two critical rules:

> **Rule 1 — Same-quarter alignment**: Only compare results from the **same fiscal period**. Don't compare DELL's Q1 with Lenovo's full year — different period lengths, different conclusions. Prioritize peers reporting in the same quarter as the target.
>
> **Rule 2 — Earnings date as reference point**: The reference time for assessing earnings transmission value is the **target stock's earnings date**, NOT the user's current query time.
> - Results that the target did NOT know at its earnings date → cannot be used for prediction, only for post-hoc confirmation
> - Results that the target DID know at its earnings date → strong reference value
> - Example: Lenovo reported May 22, DELL reported May 28. On Lenovo's earnings date (May 22), DELL's results were unknown → "DELL helped predict Lenovo" is incorrect. Correct: **Lenovo reported first (May 22), DELL reported later (May 28), DELL's results confirmed the trend Lenovo revealed.**

The chronological framework:

| Chronology | Target | Peer | Transmission Value |
|------------|--------|------|-------------------|
| **Peer first → Target later** (同季度) | Target about to report | Peer already reported same quarter | ✅ **最强** — peer results are a leading indicator for target |
| **Target first → Peer later** (同季度) | Target already reported | Peer about to report same quarter | ✅ **强** — target results become leading indicator for peer |
| **Cross-quarter** | Different quarters | Different quarters | ⚠️ 弱 — note the mismatch, avoid strong conclusions |
| **Both already reported** | Already reported | Already reported | ❌ 无新传导 — 可做事后验证，不作预测 |

For **non-earnings periods**, fall back to general correlation logic:
- **Multi-relationship peers** (e.g., competitor + supplier): identify which relationship dominates the peer's stock reaction
- **⚠️ Profit driver divergence**: if the peer's profit driver differs from the overlapping segment with the target, the peer may move independently or even opposite — flag this explicitly.

Each prediction must include:

- **Direction**: Bullish / Bearish / Neutral
- **Confidence**: High / Medium / Low (with 1-line rationale)
- **Key catalyst**: specific event + timing
- **Counter-argument**: at least 1 item
- **Required peer fields**: ticker + market (A-share/HK/US) + earnings date

### 2.5 近期催化剂评价（非财报期核心依据）

在非财报密集期，近期催化剂应取代财报传导成为预测的核心依据。按以下维度评估每项催化剂：

| 维度 | 说明 |
|------|------|
| **事件类型** | 行业大会/产品发布/政策法规/重大订单/管理层变动/诉讼/监管行动/外部冲击等 |
| **日期** | 精确日期，距离当前 < 2 周 |
| **影响方向** | 利多 / 利空 / 中性待定 |
| **影响强度** | 强（预期 > ±3% 波动）/ 中（±1%~3%）/ 弱（< ±1%） |
| **可验证性** | 是否有具体数字/里程碑可追踪（如：订单金额、MAU 数据、量产时间表等） |

**判定规则**：
- 如果有 **2 项或以上强利多催化剂** + 整体技术面未转空 → 方向可上调至 Bullish
- 如果有 **1 项强利空催化剂**（如出口限制、反垄断调查、业绩预警）→ 即使其他因素稳健，也需标记为 Neutral 或降低信心至 Low
- 如果无明显近期催化剂 → 标注"近期无明显催化剂"，按基本面和技术面中性判断

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
