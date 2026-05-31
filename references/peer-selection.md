# Peer Company Selection Rules

From the target stock, select 1–3 strongly correlated peer companies for ripple analysis.

---

## Selection Priority

Select in the following order, keeping total at 1–3:

| Priority | Type | Condition |
|----------|------|-----------|
| 1 | Same sector leader | Top-3 by market cap, with abnormal ~1M price/volume vs sector average |
| 2 | Industry chain related | Upstream supplier / downstream customer / direct competitor — sector leader |
| 3 | Earnings catalyst | Recently reported or reporting within 2 weeks — sector leader |

**Weighting**: Companies meeting multiple criteria rank higher. Companies with earnings within 2 weeks get +1 priority.

---

## Defining "Leader"

A company qualifies as a "leader" if it meets any of:

- Top 3 by market cap in its sector/industry
- Universally recognized leader (search `{sector} leading company` — top 3 results consistently point to the same name)
- Largest participant in its industry chain node (e.g., largest supplier, largest customer)

**Exclude**:
- ST / *ST stocks (China)
- Delisting risk
- Illiquid (A-share avg daily turnover < ¥50M, HK/US similarly illiquid)
- Concept stocks with no substantive business link to the target

---

## Relationship Types

| Type | Description | How to identify |
|------|-------------|-----------------|
| Same sector | Same industry/concept sector | Exchange sector classification, financial websites |
| Upstream | Supplier, raw materials, equipment | Search `{company} main suppliers`; annual report top-5 suppliers |
| Downstream | Customer, channel | Search `{company} main customers`; annual report top-5 customers |
| Competitor | Direct competitor | Search `{company} competitors`; market share data |

---

## Industry Chain Identification Flow

1. WebSearch: `{company name} supply chain industry chain`
2. WebSearch: `{company name} main suppliers customers`
3. Extract 2–3 frequently mentioned company names from results
4. Verify they are publicly traded (have a ticker/code)
5. Filter by market cap / industry position to select leaders

---

## Earnings Priority

| Status | Weight | Notes |
|--------|--------|-------|
| Upcoming within 2 weeks | Highest | Short-term catalyst, prioritize analysis |
| Reported within past 2 weeks | High | Earnings beat/miss affects ripple |
| Reported within past month | Medium | Market may have partly priced in |
| Next earnings >1 month away | Low | Limited short-term impact |

**Earnings analysis focus**:
- Revenue/profit vs expectations (beat/miss)
- Management guidance changes
- Gross margin / net margin trends
- Supply chain signals (e.g., customer order cuts, supplier price increases)

---

## Anomaly Detection

The following qualify as "significant movement" — prioritize for peer analysis:

- ~1M price change > 2× sector average (e.g., sector +5%, stock +12% or -8%)
- Single-day volume > 2× 20-day average
- Major announcement (M&A, capital raise, earnings pre-announcement, policy impact)
- Concentrated analyst rating changes (upgrades/downgrades)

WebSearch templates:
```
{company name} stock price anomaly recent month
{company name} major announcement 2026
{company name} analyst rating change 2026
```

---

## Final Checklist

After selecting peers, confirm:

- [ ] 1–3 companies, no more than 3
- [ ] All publicly traded — confirmed ticker/code
- [ ] Substantive business link to target (not pure concept play)
- [ ] At least 1 with a near-term earnings event (already reported or upcoming)
- [ ] No ST / delisting risk
- [ ] Relationship type marked (same sector / upstream / downstream / competitor)
