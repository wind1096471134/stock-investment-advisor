# Public Data Sources & Search Templates

Use **WebSearch** and **WebFetch** only — from public web pages. No third-party API calls.
Always label data with the query date.

> ⚠️ **Regional Accessibility**: The reachability of each source varies by network region.
> If a source is unreachable in your current environment, try the alternative source listed, or mark the data as "Not available."

> **Language selection**: When the user writes in English, prefer English search templates (below) and English-language sources (Yahoo Finance / SEC EDGAR / CNBC / MarketWatch). When the user writes in Chinese, prefer Chinese search templates and Chinese-language sources (EastMoney / Xueqiu / Tonghuashun). The templates below cover both languages.

---

## Regional Accessibility Overview

| Source | A-shares | HK | US | China-reachable | Global-reachable | Notes |
|--------|----------|-----|-----|-----------------|------------------|-------|
| EastMoney (eastmoney.com) | ✅ | ❌ | ❌ | ✅ Fast | ⚠️ Slow/restricted | Best for A-shares |
| Xueqiu (xueqiu.com) | ✅ | ✅ | ✅ | ✅ | ✅ | Discussions, opinions |
| Tonghuashun (10jqka.com.cn) | ✅ | ❌ | ❌ | ✅ | ⚠️ Slow | Sector flows |
| SH/SZ Exchange announcements | ✅ | ❌ | ❌ | ✅ | ✅ | Official filings |
| AASTOCKS (aastocks.com) | ❌ | ✅ | ❌ | ✅ | ✅ | HK stock quotes |
| HKEX披露易 (hkexnews.hk) | ❌ | ✅ | ❌ | ✅ | ✅ | HK official filings |
| Yahoo Finance | ❌ | ✅ | ✅ | ⚠️ Unstable | ✅ | Global quotes & news |
| SEC EDGAR | ❌ | ❌ | ✅ | ✅ | ✅ | US earnings filings |
| MarketWatch | ❌ | ❌ | ✅ | ⚠️ Slow | ✅ | Sector news |
| CNBC | ❌ | ❌ | ✅ | ✅ | ✅ | Sector news |
| Finviz (finviz.com) | ❌ | ❌ | ✅ | ⚠️ Unstable | ✅ | Sector screener |

**Strategy**: Prioritize sources with good regional reachability. If search returns no useful data, try the alternative source. If still none, mark as "Not available."

---

## A-shares (China)

### Hot sectors / concepts (past month)

Chinese:
```
A股 热门板块 近一个月 {年份}
A股 概念板块 涨幅排名 近一月
site:eastmoney.com 板块涨幅 近一个月
A股 资金流向 热门行业 {月份}
```

English:
```
China A-share best performing sectors past month {year}
top concepts A-share market recent month
site:xueqiu.com A-share hot sectors
```

Alternative (overseas): `site:xueqiu.com 热门板块`

### Top stocks within a sector

Chinese:
```
{板块名} A股 龙头 市值排名
{板块名} 概念股 龙头 2026
site:xueqiu.com {板块名} 龙头
```

English:
```
{sector} China A-share leading stocks market cap ranking
largest {sector} companies A-share market
```

### Individual stock

Chinese:
```
{公司名/代码} 最新股价 涨跌幅
{公司名} 最新财报 业绩 2026
{公司名} 重大公告 近一个月
{公司名} 所属行业 板块
site:eastmoney.com {代码}
```

English:
```
{code/ticker} stock price A-share
{company name} earnings report 2026 last quarter
{company name} recent news last month
```

### Recommended sources

| Source | Use | Regional note |
|--------|-----|---------------|
| EastMoney (eastmoney.com) | Sector rankings, quotes, earnings | Slow overseas, fallback to Xueqiu |
| Xueqiu (xueqiu.com) | Discussions, sector views | Global reachable |
| SH/SZ Exchange | Official announcements | Global reachable |
| Tonghuashun (10jqka.com.cn) | Sector flows, concepts | Slow overseas |

---

## Hong Kong Stocks

### Hot sectors

Chinese:
```
港股 热门板块 近一个月 {年份}
港股 行业涨幅 排名 近一月
site:aastocks.com 行业表现
```

English:
```
Hong Kong stock best performing sectors past month {year}
Hang Seng index sector performance ranking
```

Alternative: `site:xueqiu.com 港股 热门`

### Top stocks within a sector

Chinese:
```
港股 {板块名} 龙头 市值
{板块名} 港股 概念股 龙头
site:xueqiu.com 港股 {板块名}
```

English:
```
{sector} Hong Kong stocks market cap leaders
largest {sector} companies in Hong Kong stock market
```

### Individual stock

Chinese:
```
{公司名/代码} 港股 最新股价
{公司名} 港股 财报 业绩 2026
{公司名} 公告 近一个月
{代码}.HK 最新行情
```

English:
```
{code}.HK stock price
{company name} Hong Kong earnings report 2026
{code}.HK earnings date
```

### Recommended sources

| Source | Use | Regional note |
|--------|-----|---------------|
| AASTOCKS (aastocks.com) | Quotes, sectors | Global reachable |
| Xueqiu | Discussions | Global reachable |
| HKEX披露易 (hkexnews.hk) | Official filings | Global reachable |
| Yahoo Finance | Price, earnings dates | May be unstable in China |

---

## US Stocks

### Hot sectors

English:
```
US stock hot sectors last month {year}
best performing sectors US stock market past month
{sector} stocks trending 2026
```

### Top stocks within a sector

English:
```
{sector} US stocks market cap leaders
{sector} stock earnings calendar next 2 weeks
largest {sector} companies US 2026
```

### Individual stock

English:
```
{code/ticker} stock price 1 week 1 month
{company name} earnings report latest 2026
{code} earnings date next
{company name} news last month
site:finance.yahoo.com {code}
site:marketwatch.com {code}
```

Chinese (alternative):
```
{公司名} 美股 最新股价 财报
{代码} 美股 分析师评级
```

### Recommended sources

| Source | Use | Regional note |
|--------|-----|---------------|
| Yahoo Finance | Price, earnings, news | May be unstable in China |
| SEC EDGAR | Official earnings | Global reachable |
| MarketWatch / CNBC | Sector news | Global reachable |
| Finviz (finviz.com) | Sector screener | May be unstable in China |

---

## General-purpose Searches

### Industry chain

Chinese:
```
{公司名} 上游 供应商 龙头
{公司名} 下游 客户 主要
{公司名} 竞争对手 行业龙头
```

English:
```
{company name} supply chain major partners
{company name} main suppliers customers
{company name} vs competitors market share
```

### Policy & regulatory changes

> 🔑 **Critical**: Policy changes can have outsized impact on stock prices in regulated industries (energy, auto, healthcare, telecom, finance, space, defense). Search specifically for recent 1–2 month policy developments.

Chinese:
```
{行业} 政策 最新 2026
{公司名} 政策影响 监管
{行业} 补贴 关税 法规 近两个月
```

English:
```
{industry/sector} regulation policy change 2026
{company name} regulatory impact tariff
{industry} subsidy tax policy update
```

### Strategic investment / cross-ownership

> ⚠️ **Important**: Investment relationships often cross sectors (e.g., a tech giant investing in a space/auto company). Search broadly — don't limit to the same industry.

Chinese:
```
{公司名} 主要股东 战略投资
{公司名} 投资了哪些公司 参股
{公司名} 股权结构 大股东
```

English:
```
{company name} major shareholders strategic investors
{company name} invested in portfolio companies
{company name} equity ownership structure
```

### Technical analysis

> 🔑 **Key for short-term (1–2 week) outlook**: technical signals often matter more than fundamentals for this timeframe.

Chinese:
```
{代码} 技术分析 支撑位 压力位 均线
{代码} RSI MACD KDJ
{代码} 52周最高最低
```

English:
```
{ticker} technical analysis support resistance moving average
{ticker} RSI MACD 2026
{ticker} 52 week high low
```

### Capital flow / institutional flow

Chinese:
```
{代码} 主力资金 净流入 近一周
{代码} 北向资金 持仓 变动
{代码} 融资融券 余额 变化
```

English:
```
{ticker} institutional ownership change 13F
{ticker} short interest ratio latest
{stock} insider buying selling recent
```

### Valuation percentile

Chinese:
```
{代码} 市盈率 历史分位 近五年
{代码} PE PB 估值百分位
```

English:
```
{ticker} PE percentile 5 year range
{ticker} valuation historical percentile
{stock} current valuation vs historical average
```

### Earnings calendar

Chinese:
```
{公司名} 财报 发布日期 2026
{代码} 财报日期
```

English:
```
{code} earnings date Q1 Q2 2026
{ticker} earnings report date
{sector} earnings calendar next 2 weeks
```

### Hot topics (guidance phase)

Chinese:
```
{市场} 投资 热点 近一个月
site:xueqiu.com 热榜
{市场} 热门股票 近一个月 涨幅
```

English:
```
trending stocks last month {market}
{market} stock market hot sectors recent
```

---

## Search Strategy

1. **Parallel search**: target stocks, peers, and sector heat can be searched in parallel with WebSearch. Use SubAgents for parallel collection when analyzing multiple stocks.
2. **Region-aware**: prioritize sources with good reachability for your current network region. Only mark as missing after trying alternatives.
3. **Cross-verify**: use at least 2 consistent sources before reporting a data point. Note "Data discrepancy" if results conflict.
4. **Recency**: prioritize results from the past month. Label data with the query date.
5. **No login required**: only use publicly accessible pages.
6. **Honest reporting**: mark unobtainable data as "Not available." Never fabricate or fall back to any third-party API.
7. **Search beyond same sector**: always search for strategic investors / major shareholders / portfolio companies — these relationships often cross industry boundaries and are easy to miss.
8. **Include technical & capital flow signals**: for short-term (1–2 week) outlooks, technical indicators (support/resistance, MA, RSI) and capital flow data (institutional flow, north-bound flow) are often more useful than fundamentals.
9. **Check valuation percentile**: absolute PE is less informative than PE relative to historical range — search for "PE at 5-year XX% percentile" to gauge valuation context.
10. **Precision**: use approximate values ("~+2%", "~down 5 points") and label with the query date. Trend and relative value matter more than exact figures.
