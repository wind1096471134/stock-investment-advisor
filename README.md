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
| **Peer selection** | Top-3 peers by market cap + industry chain (upstream/downstream/competitor) |
| **Bidirectional prediction** | How peers signal → target stock; how target catalyst → peer ripple |
| **Financial comparison** | Revenue growth, PE, gross margin — target vs peers vs industry average |
| **Structured report** | Conclusion-first: recommendation → prediction summary → detailed analysis |

### Supported Markets

- **A-shares** (Shanghai / Shenzhen, China)
- **Hong Kong stocks**
- **US stocks** (NYSE / Nasdaq)

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
1. 📊 Executive Summary       — Recommendation, core logic, catalysts, risks
2. 🔮 Prediction Summary      — Target + peer predictions with market & earnings dates
3. 📋 Target Stock Snapshot   — Price, valuation, recent events
4. 🏢 Peer Deep Dive          — Financial comparison table
5. 🔄 Bidirectional Logic     — Cross-impact analysis
6. ⚠️ Risk Disclaimer
```

### 🛠️ Files

```
stock-investment-advisor/
├── SKILL.md                # Skill definition (entry point)
├── output-template.md      # Report output template with example
├── references/
│   ├── data-sources.md     # Search templates & source accessibility
│   └── peer-selection.md   # Peer company selection rules
├── README.md               # This file
├── LICENSE                 # MIT
└── .gitignore
```

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
| **关联公司** | 同板块市值 Top3 + 产业链上下游（供应商/客户/竞品） |
| **双向预测** | 关联公司→目标股信号传导；目标股催化剂→关联股联动 |
| **财务对比** | 目标股 vs 关联股 vs 行业均值：营收增速、PE、毛利率 |
| **结构化报告** | 结论先行：建议 → 预测 → 详细分析 |

### 支持的市场

- **A 股**（上海/深圳）
- **港股**
- **美股**（NYSE / Nasdaq）

### 🌐 自动语言检测

| 你的输入 | 数据源 | 输出语言 |
|---------|--------|---------|
| `Analyze AAPL` | Yahoo Finance, SEC EDGAR, CNBC | 英文报告 |
| `分析贵州茅台` | 东方财富, 雪球, 上交所 | 中文报告 |

无需任何配置——系统自动检测你的语言，适配全流程。

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

全部通过 Claude Code 内置的 **WebSearch** / **WebFetch** 获取，无需任何 API Key。

### 📄 报告结构

```
1. 📊 综合结论        — 操作建议、核心逻辑、催化剂、风险
2. 🔮 预测汇总        — 目标股 + 关联股预测（含市场和财报日期）
3. 📋 目标股票快照    — 价格、估值、近期动态
4. 🏢 关联公司深度分析 — 财务对比表
5. 🔄 双向预测逻辑    — 交叉影响分析
6. ⚠️ 风险提示
```

### 🛠️ 文件说明

```
stock-investment-advisor/
├── SKILL.md                # 技能主定义（入口文件）
├── output-template.md      # 报告输出模板及示例
├── references/
│   ├── data-sources.md     # 搜索模板与数据源可达性
│   └── peer-selection.md   # 关联公司筛选规则
├── README.md               # 本文件
├── LICENSE                 # MIT 协议
└── .gitignore
```

### ⚖️ 开源协议

MIT — 详见 [LICENSE](./LICENSE)。

### ⚠️ 风险提示

以上内容基于公开信息整理，仅为研究参考，**不构成投资建议**。股市有风险，短期走势受政策、资金、情绪等多因素影响，预测存在较大不确定性。请独立判断，谨慎决策。
