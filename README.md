# Stock Investment Advisor — Claude Code Skill

> Analyze stocks (A-shares / HK / US) using WebSearch only — no API keys, no Python packages, no paid data sources. Provides 1–2 week directional outlooks based on publicly available information.

---

**English** · [中文版](#中文说明)

---

## English

### 🚀 Quick Install

```bash
# Clone into your Claude Code skills directory
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/<your-user>/stock-investment-advisor.git

# Or symlink if already cloned elsewhere
ln -s /path/to/stock-investment-advisor ~/.claude/skills/
```

Then invoke in Claude Code:

```
/stock-investment-advisor
```

Or let the skill auto-trigger on stock-related queries — works in both English and Chinese.

### ✨ Features

| Capability | Description |
|-----------|-------------|
| **Stock recognition** | A-shares (6-digit), HK stocks (5-digit/.HK), US stocks (ticker), Chinese company names |
| **Quick snapshot** | Price, ~1W/~1M change, sector, market cap, key events, earnings calendar |
| **Multi-dimension analysis** | Fundamentals, **technical signals (MA/RSI)**, **capital flow**, **valuation percentile**, policy impact |
| **Peer selection** | Top-3 peers **globally across all markets** + industry chain + **strategic investors/investees** |
| **Business segment decomposition** | For each peer, identifies profit driver vs overlapping segment — **prevents wrong predictions** |
| **Probability-weighted scenarios** | Bullish/base/bearish with probabilities, expected ranges, and triggers |
| **Bidirectional prediction** | How peers signal → target stock; how target catalyst → peer ripple; flags when profit driver diverges |
| **Financial comparison** | Revenue growth, PE, gross margin — target vs peers vs industry average |
| **Structured report** | Conclusion-first: recommendation → predictions → detailed analysis |

### Supported Markets

- **A-shares** (Shanghai / Shenzhen, China)
- **Hong Kong stocks**
- **US stocks** (NYSE / Nasdaq)
- **Cross-market peer search** (e.g., DELL → also finds Lenovo in HK)

### 🌐 Auto Language Detection

| Your input | Search sources | Output |
|-----------|---------------|--------|
| `Analyze AAPL` | Yahoo Finance, SEC EDGAR, CNBC | English report |
| `分析贵州茅台` | 东方财富, 雪球, 上交所公告 | 中文报告 |

No configuration needed — the skill detects your language and adapts the full workflow.

### 🧠 How It Works

```
User input (stock codes or names)
  ├─ 1–3 stocks → Analysis pipeline (web search → data collection → report)
  ├─ >3 stocks → Ask to narrow to 3
  └─ No stock → Guidance flow (market + sector selection)
```

### Data Sources (WebSearch only)

| Source | Coverage | Accessibility |
|--------|----------|--------------|
| EastMoney (东方财富) | A-shares | Best in China |
| Xueqiu (雪球) | All markets | Global |
| Yahoo Finance | US/HK | Global |
| SEC EDGAR | US filings | Global |
| HKEX披露易 | HK stocks | Global |

All accessible via Claude Code's **WebSearch** / **WebFetch** — no API keys needed.

### 📄 Report Structure

```
1. 📊 Executive Summary
   ├─ Recommendation (★★★ / ★★ / ★)
   ├─ Direction, Confidence, Catalyst, Technical, Capital Flow, Policy, Risk
   ├─ Rationale (7 bullets: Fundamentals, Catalyst, Technical, Flow, Policy, Peers, Valuation, Risk)
   ├─ Associated Stock Predictions (with market & earnings dates)
   └─ Probability-Weighted Scenario Analysis (bullish/base/bearish)

2. 🔮 Prediction Summary       — Target + peer predictions with market & earnings dates

3. 📋 Target Stock Snapshot    — Price, support/resistance, valuation percentile, recent events

4. 🏢 Peer Deep Dive           — Business segment decomposition, financial comparison table

5. 🔄 Bidirectional Logic      — Peer→target and target→peer with profit driver divergence flag

6. ⚠️ Risk Disclaimer
```

### Sample Output (MU - Micron Technology)

```
## 📊 Executive Summary

### Recommendation: ⭐⭐ Neutral

> Micron (MU) Q2 rev +196%, gross margin 74.9% record. Memory supercycle confirmed.
  UBS tripled PT to $1,625. But at $970 the stock has rallied +46% from the $663 low.
  June 24 FQ3 earnings is the key catalyst.

| Dimension | Judgment | Rationale |
|-----------|----------|-----------|
| Direction | Neutral | Supercycle fundamentals strong (Q3 guide $33.5B/81% GM), but +46% short-term rally priced in |
| Confidence | Medium | Forward FY27 PE only 9x on ~$102 EPS, but $1T mkt cap has priced in much optimism |
| Key Catalyst | Jun 24 FQ3 earnings; HBM4 progress; MATCH Act | Q3 guide $33.5B rev/81% GM/$19.15 EPS |
| Technical | Neutral | RSI cooled from 85 to 57 (healthy); $775 resistance broken; next resistance $981 ATH |
| Capital Flow | Bullish | Norway SWF opened $6.4B position; AQR +412%; Wellington +20% |
| Policy | Bullish | MATCH Act targets China memory makers CXMT/YMTC; MU lobbying for tighter curbs |
| Primary Risk | Memory cycle peaking; excluded from HBM4 Rubin; Samsung/SK Hynix capacity catch-up | If Q3 misses or memory prices roll over, high multiple contracts |

### Associated Stock Predictions

| Stock | Market | Relationship | Direction | Catalyst | Earnings | Conf. |
|-------|--------|-------------|-----------|----------|----------|-------|
| Samsung 005930 | Korea(KOSPI) | Competitor+Supplier | Bullish | Memory upcycle boosts semi profit; HBM share recovering | Already out | Med |
| SK Hynix 000660 | Korea(KOSPI) | Competitor(HBM leader) | Bullish | HBM share 50-55%; Rubin allocation 70% | Already out | Med |
| NVDA NVIDIA | US(NASDAQ) | Downstream customer | Bullish | $630B+ AI infra spend drives HBM demand | Already out | High |

### Probability-Weighted Scenario Analysis

| Scenario | Prob. | Range | Trigger | Rationale |
|----------|-------|-------|---------|-----------|
| Bullish | ~30% | +10-25% | Q3 beat+raise; HBM4 enters Rubin; MATCH passes | Supercycle acceleration |
| Base | ~50% | -5 to +8% | Q3 in-line; DRAM growth moderates Q4 | Normal cycle progression |
| Bearish | ~20% | -10-20% | Q3 miss; DRAM price peak; HBM share loss | Cycle peak narrative |

> Expected value: Base → Neutral. Jun 24 earnings is the pivot point.
```

### 🛠️ Files

```
stock-investment-advisor/
├── SKILL.md                # Skill definition (entry point) — all analysis rules & flow
├── output-template.md      # Report output template with example
├── references/
│   ├── data-sources.md     # Search templates, source accessibility, bilingual queries
│   └── peer-selection.md   # Peer selection rules, business segment decomposition
├── README.md               # This file
├── LICENSE                 # MIT
└── .gitignore
```

### Key Innovations

| Feature | Problem Solved |
|---------|---------------|
| **Business segment decomposition** | Samsung isn't just a phone competitor — its semi division drives 50%+ profit. Wrong if you only see the overlap. |
| **Cross-market peer search** | Analyzing DELL? Must also check Lenovo (HK). Analyzing 腾讯? Compare with Meta (US). |
| **Technical + capital flow** | For 1-2 week outlooks, these often matter more than fundamentals. |
| **Valuation percentile** | PE 22x is meaningless alone. "PE at 5-year 35% percentile" gives context. |
| **Scenario analysis** | One direction is too simple. Bullish/base/bearish with probabilities is professional-grade. |
| **Language detection** | Same skill, two languages. Type in Chinese → full Chinese workflow. Type in English → English. |

### ⚖️ License

MIT — see [LICENSE](./LICENSE).

### ⚠️ Disclaimer

**Not financial advice.** This tool analyzes publicly available information for research reference only. Stock markets involve significant risk. Always make independent decisions and consult licensed professionals.

---

<div id="中文说明"></div>

## 中文说明

### 🚀 快速安装

```bash
# 克隆到 Claude Code 技能目录
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/<你的用户名>/stock-investment-advisor.git

# 或者手动创建软链接
ln -s /path/to/stock-investment-advisor ~/.claude/skills/
```

然后在 Claude Code 中调用：

```
/stock-investment-advisor
```

或直接输入股票相关的问题自动触发，支持中英文。

### ✨ 功能一览

| 功能 | 说明 |
|------|------|
| **股票识别** | A 股 6 位代码、港股 5 位/.HK、美股字母代码、中文公司名 |
| **快速快照** | 股价、近 1 周/1 月涨跌、板块、市值、重大事件、财报日历 |
| **多维度分析** | 基本面 + **技术面（均线/RSI）** + **资金流向** + **估值百分位** + 政策影响 |
| **关联公司** | **跨市场全球 Top3** + 产业链 + **战略投资/交叉持股** |
| **业务分拆** | 分拆关联公司各板块利润来源，防止分析三星只当手机股看的错误 |
| **概率场景** | 乐观/基准/悲观三档概率加权预测 |
| **双向预测** | 关联→目标信号传导；目标→关联联动；**利润来源背离时明确标注** |
| **财务对比** | 目标股 vs 关联股 vs 行业均值：营收增速、PE、毛利率 |
| **结构化报告** | 结论先行：建议 → 预测 → 详细分析 |

### 支持的市场

- **A 股**（上海/深圳）
- **港股**
- **美股**（NYSE / Nasdaq）
- **跨市场关联搜索**（如 DELL 同时找联想 HK）

### 🌐 自动语言检测

| 你的输入 | 数据源 | 输出语言 |
|---------|--------|---------|
| `Analyze AAPL` | Yahoo Finance, SEC EDGAR, CNBC | 英文报告 |
| `分析贵州茅台` | 东方财富, 雪球, 上交所 | 中文报告 |

### 🧠 工作流程

```
用户输入（股票代码或名称）
  ├─ 1～3 只 → 分析流程（搜索 → 数据采集 → 报告生成）
  ├─ >3 只 → 要求缩减至 3 只
  └─ 无股票 → 引导流程（选择市场 + 板块）
```

### 数据来源（仅 WebSearch）

| 来源 | 覆盖 | 可达性 |
|------|------|--------|
| 东方财富 | A 股 | 国内最佳 |
| 雪球 | 全市场 | 全球可达 |
| Yahoo Finance | 美股/港股 | 全球可达 |
| SEC EDGAR | 美股财报 | 全球可达 |
| 港交所披露易 | 港股 | 全球可达 |

### 📄 报告结构

```
1. 📊 综合结论
   ├─ 操作建议（★★★ / ★★ / ★）
   ├─ 方向、置信度、催化剂、技术面、资金流向、政策、风险
   ├─ 理由（基本面、催化剂、技术面、资金流、政策、同行、估值、风险）
   ├─ 关联股预测（含市场、财报日期、置信度）
   └─ 概率加权场景分析（乐观/基准/悲观）

2. 🔮 预测汇总
3. 📋 目标股票快照    — 含支撑/阻力位、估值历史百分位
4. 🏢 关联公司深度分析 — 业务分拆确认 + 财务对比表
5. 🔄 双向预测逻辑    — 利润来源背离时标注
6. ⚠️ 风险提示
```

### 输出示例：美光科技（MU）

```
## 📊 综合结论

### 操作建议：⭐⭐ 观望

> 美光 Q2 营收+196%、毛利率 74.9% 创纪录，存储超级周期确认。
  UBS 目标价三倍上调至 $1,625。但股价从 $663 低点已反弹 +46%，
  6月24日 Q3 财报是分水岭。

| 维度 | 判断 | 依据 |
|------|------|------|
| 方向 | 观望 | 超级周期极强（Q3 指引 $335 亿/毛利率 81%），但短线涨幅已大 |
| 置信度 | 中 | 远期 FY27 PE 仅 9x（EPS ~$102），但 $1 万亿已定价大量乐观预期 |
| 关键催化剂 | 6/24 Q3 财报；HBM4 进展；MATCH 法案 | Q3 指引 $335 亿/EPS $19.15 |
| 技术面 | 中性 | RSI 从 85 回落至 57（健康）；$775 已突破；下一阻力 $981 |
| 资金流向 | 偏多 | 挪威主权基金开 $64 亿仓位；AQR +412% |
| 政策 | 偏多 | MATCH 法案限制中国存储厂 CXMT/YMTC |
| 主要风险 | 存储周期见顶；HBM4 被排除在 Rubin 外 | 若 Q3 不及预期高估值回调 |

### 关联股预测

| 股票 | 市场 | 关系 | 方向 | 催化剂 | 财报 | 置信度 |
|------|------|------|------|--------|------|--------|
| 三星 005930 | 韩国(KOSPI) | 竞品+供应商 | 偏多 | 存储涨价周期；HBM 份额追赶 | 已发 | 中 |
| SK 海力士 000660 | 韩国(KOSPI) | 竞品 | 偏多 | HBM 50-55% 领先；Rubin 70% 分配 | 已发 | 中 |
| NVDA 英伟达 | 美股(NASDAQ) | 下游客户 | 偏多 | AI 基建 $6300 亿+ 驱动 HBM | 已发 | 高 |

### 概率加权场景分析

| 场景 | 概率 | 预期区间 | 触发条件 |
|------|------|---------|---------|
| 乐观 | ~30% | +10-25% | Q3 beat+raise；HBM4 入 Rubin；MATCH 通过 |
| 基准 | ~50% | -5 至 +8% | Q3 符合预期；DRAM 增速 Q4 放缓 |
| 悲观 | ~20% | -10-20% | Q3 miss；DRAM 价格见顶 |
```

### 核心创新点

| 特性 | 解决了什么问题 |
|------|--------------|
| **业务分拆** | 三星不只有手机——半导体贡献 50%+ 利润，存储涨价反而利好啊！ |
| **跨市场关联** | 分析戴尔不找联想？分析腾讯不找 Meta？现在已经自动跨市场搜索 |
| **技术面+资金流** | 1-2 周短线研判，这些比基本面更敏感 |
| **估值百分位** | "PE 22x" 没有上下文，"PE 处于 5 年 35% 分位"才有意义 |
| **概率场景** | 一个方向太简单。三档概率加权才是专业研报水准 |
| **语言检测** | 输入中文→中文全流程；输入英文→英文全流程，无需配置 |

### 🛠️ 文件说明

```
stock-investment-advisor/
├── SKILL.md                # 技能主定义（入口文件）— 所有分析规则与流程
├── output-template.md      # 报告输出模板及示例
├── references/
│   ├── data-sources.md     # 搜索模板、数据源可达性、中英双语查询
│   └── peer-selection.md   # 关联公司筛选规则、业务分拆方法
├── README.md               # 本文件
├── LICENSE                 # MIT 协议
└── .gitignore
```

### ⚖️ 开源协议

MIT — 详见 [LICENSE](./LICENSE)。

### ⚠️ 风险提示

以上内容基于公开信息整理，仅为研究参考，**不构成投资建议**。股市有风险，短期走势受政策、资金、情绪等多因素影响，预测存在较大不确定性。请独立判断，谨慎决策。
