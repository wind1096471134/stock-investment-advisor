# Stock Investment Advisor — Claude Code Skill

> **Analyze Chinese and US stocks** using only public web search (WebSearch / WebFetch).
> No third-party API, no paid data source, no Python dependency.

Analyze individual stocks, peer companies, and industry chains — generate 1–2 week directional outlooks based on publicly available information.

**Language**: Chinese (Simplified) — the skill's output and analysis rules are tailored for Chinese-speaking retail investors analyzing A-shares, Hong Kong stocks, and US stocks.

---

## 🚀 Quick Install

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

Or trigger via keywords: `分析股票`, `投资建议`, 板块热点, 财报影响, etc.

---

## ✨ What It Does

| Capability | Description |
|-----------|-------------|
| **Stock recognition** | A-shares (6-digit), HK stocks (5-digit/.HK), US stocks (ticker), Chinese company names |
| **Quick snapshot** | Price, 1W/1M change, sector, market cap, key events, earnings calendar |
| **Peer selection** | Top-3 by market cap in same sector + industrial chain (upstream/downstream/competitor) |
| **Bidirectional prediction** | How peers signal → target stock; how target catalyst → peer ripple |
| **Structured report** | Conclusion-first: recommendation → prediction summary → detailed analysis |

### Supported Markets

- **A股** (China A-shares: Shanghai / Shenzhen)
- **港股** (Hong Kong stocks)
- **美股** (US stocks: NYSE / Nasdaq)

---

## 🧠 How It Works

```
User input (stock codes or names)
  ├─ 1-3 stocks → Analysis pipeline (web search → data collection → report)
  ├─ >3 stocks → Ask to narrow down to 3
  └─ No stock → Guidance flow (market + sector selection)
```

### Data Sources (WebSearch only, no API)

| Source | Coverage | Accessibility |
|--------|----------|--------------|
| EastMoney (东方财富) | A-shares | Best in China |
| Xueqiu (雪球) | All markets | Global |
| Yahoo Finance | US/HK | Global (may be slow in China) |
| SEC EDGAR | US filings | Global |
| HKEX披露易 | HK stocks | Global |

All sources are accessed via Claude Code's built-in **WebSearch** / **WebFetch** — no API keys, no Python packages.

---

## 📄 Report Structure

Output follows this order:

1. **📊 综合结论 (Executive Summary)** — recommendation ★★★, core logic, key catalysts, risks
2. **🔮 预测汇总 (Prediction Summary)** — target + peer predictions with market & earnings dates
3. **📋 目标股票快照 (Target Snapshot)** — price, valuation, recent events
4. **🏢 关联公司深度分析 (Peer Deep Dive)** — peer data + financial comparison table
5. **🔄 双向预测逻辑 (Bidirectional Logic)** — cross-impact analysis
6. **⚠️ 风险提示 (Risk Disclaimer)**

> See [output-template.md](./output-template.md) for full template and example.

---

## 🛠️ Files

```
stock-investment-advisor/
├── SKILL.md                # Skill definition (entry point)
├── output-template.md      # Report output template with example
├── references/
│   ├── data-sources.md     # Search templates & source accessibility
│   └── peer-selection.md   # Peer company selection rules
├── README.md               # This file
├── LICENSE                 # MIT License
└── .gitignore
```

---

## ⚖️ License

MIT — see [LICENSE](./LICENSE).

---

## ⚠️ Disclaimer

**Not financial advice.** This tool analyzes publicly available information for research/reference only. Stock markets involve significant risk. Always make independent decisions and consult licensed professionals.
