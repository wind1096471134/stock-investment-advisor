# 公开信息源与搜索模板

仅使用 **WebSearch** 和 **WebFetch** 从公开网页获取信息。不调用任何第三方 API。
所有数据标注查询日期。

> ⚠️ **区域可达性说明**：以下来源在不同网络区域的访问情况不同。
> 如果某个来源在当前环境下无法访问，尝试其替代来源，或标注「该数据未获取到」。

> **语言选择**：用户用英文提问时，优先使用英文搜索模板（English query templates below）和英文来源（Yahoo Finance / SEC EDGAR / CNBC / MarketWatch）；用户用中文提问时，优先使用中文搜索模板和中文来源（东方财富 / 雪球 / 同花顺）。

---

## 区域可达性总览

| 来源 | A 股 | 港股 | 美股 | 国内可达 | 海外可达 | 说明 |
|------|------|------|------|----------|----------|------|
| 东方财富 (eastmoney.com) | ✅ | ❌ | ❌ | ✅ 优 | ⚠️ 慢/受限 | A 股最全 |
| 雪球 (xueqiu.com) | ✅ | ✅ | ✅ | ✅ | ✅ | 讨论、观点 |
| 同花顺 (10jqka.com.cn) | ✅ | ❌ | ❌ | ✅ | ⚠️ 慢 | 板块资金 |
| 上交所/深交所公告 | ✅ | ❌ | ❌ | ✅ | ✅ | 官方公告 |
| 阿斯达克 (aastocks.com) | ❌ | ✅ | ❌ | ✅ | ✅ | 港股行情 |
| 港交所披露易 (hkexnews.hk) | ❌ | ✅ | ❌ | ✅ | ✅ | 港股官方 |
| Yahoo Finance | ❌ | ✅ | ✅ | ⚠️ 不稳定 | ✅ | 全球行情 |
| SEC EDGAR | ❌ | ❌ | ✅ | ✅ | ✅ | 美股财报 |
| MarketWatch | ❌ | ❌ | ✅ | ⚠️ 慢 | ✅ | 行业动态 |
| CNBC | ❌ | ❌ | ✅ | ✅ | ✅ | 行业动态 |
| Finviz (finviz.com) | ❌ | ❌ | ✅ | ⚠️ 不稳定 | ✅ | 板块筛选 |

**策略**：搜索时优先选择当前区域可达性优的来源；如搜索结果无有效数据，尝试替代来源；仍无则标注缺失。

---

## A 股

### 热门板块/概念（近 1 个月）

```
A股 热门板块 近一个月 {年份}
A股 概念板块 涨幅排名 近一月
site:eastmoney.com 板块涨幅 近一个月
A股 资金流向 热门行业 {月份}
```

替代源（海外）：`site:xueqiu.com 热门板块`

### 板块内头部股票

```
{板块名} A股 龙头 市值排名
{板块名} 概念股 龙头 2026
site:xueqiu.com {板块名} 龙头
```

### 单只股票

```
{公司名/代码} 最新股价 涨跌幅
{公司名} 最新财报 业绩 2026
{公司名} 重大公告 近一个月
{公司名} 所属行业 板块
site:eastmoney.com {代码}
```

替代源（海外）：`site:xueqiu.com {代码}`

### 推荐公开来源

| 来源 | 用途 | 区域注意 |
|------|------|----------|
| 东方财富 (eastmoney.com) | 板块涨幅、个股行情、财报 | 海外可能慢，可换雪球 |
| 雪球 (xueqiu.com) | 热门讨论、行业观点 | 国内外均可 |
| 上交所/深交所公告 | 官方重大事项 | 全球可达 |
| 同花顺 (10jqka.com.cn) | 板块资金、概念 | 海外可能慢 |

---

## 港股

### 热门板块

```
港股 热门板块 近一个月 {年份}
港股 行业涨幅 排名 近一月
site:aastocks.com 行业表现
Hang Seng index sector performance

替代源：`site:xueqiu.com 港股 热门`
```

### 板块内头部股票

```
港股 {板块名} 龙头 市值
{板块名} 港股 概念股 龙头
site:xueqiu.com 港股 {板块名}
```

### 单只股票

```
{公司名/代码} 港股 最新股价
{公司名} 港股 财报 业绩 2026
{公司名} 公告 近一个月
{代码}.HK earnings date
{代码}.HK stock price
```

替代源：`site:aastocks.com {代码}`

### 推荐公开来源

| 来源 | 用途 | 区域注意 |
|------|------|----------|
| 阿斯达克 (aastocks.com) | 港股行情、行业 | 全球可达 |
| 雪球 | 讨论热度 | 全球可达 |
| 港交所披露易 (hkexnews.hk) | 官方公告 | 全球可达 |
| Yahoo Finance | 价格、财报日期 | 国内可能不稳定，作为备选 |

---

## 美股

### 热门板块

```
US stock hot sectors last month {year}
best performing sectors US stock market past month
{sector} stocks trending 2026
```

### 板块内头部股票

```
{sector} US stocks market cap leaders
{sector} stock earnings calendar next 2 weeks
largest {sector} companies US 2026
```

### 单只股票

```
{公司名/代码} stock price 1 week 1 month
{公司名} earnings report latest 2026
{代码} earnings date next
{公司名} news last month
site:finance.yahoo.com {代码}
site:marketwatch.com {代码}
```

### 推荐公开来源

| 来源 | 用途 | 区域注意 |
|------|------|----------|
| Yahoo Finance | 价格、财报、新闻 | 国内可能不稳定 |
| SEC EDGAR | 官方财报 | 全球可达 |
| MarketWatch / CNBC | 行业动态 | 全球可达 |
| Finviz (finviz.com) | 板块筛选 | 国内不稳定，可换 CNBC |

---

## 通用搜索

### 产业链关系

```
{公司名} 上游 供应商 龙头
{公司名} 下游 客户 主要
{公司名} 竞争对手 行业龙头
{公司名} supply chain major partners
{公司名} vs competitors market share
```

### 财报日历

```
{公司名} 财报 发布日期 2026
{代码} earnings date Q1 Q2 2026
{行业} 财报季 时间表 2026
earnings calendar {sector} next 2 weeks
```

### 热门话题（引导阶段）

```
{市场} 投资 热点 近一个月
site:xueqiu.com 热榜
{市场} 热门股票 近一个月 涨幅
```

---

## 搜索策略

1. **并行搜索**：目标股、关联公司、板块热度可并行 WebSearch；多标的时使用 SubAgent 并行采集
2. **区域感知**：优先选择当前区域可达性优的来源；尝试替代源后才标注缺失
3. **交叉验证**：同一数据至少 2 个来源一致再采用；冲突时标注「数据存疑」
4. **时效性**：优先近 1 个月结果；标注数据日期
5. **不依赖登录**：仅使用无需账号的公开页面
6. **如实标注**：无法获取的数据标注「未获取到」，不编造、不降级到任何第三方 API
7. **精度要求**：股价涨跌幅等数值以近似值表达（「+约 2%」「下跌约 5 元」），标注「约」和查询日期，趋势比精确数值更重要
