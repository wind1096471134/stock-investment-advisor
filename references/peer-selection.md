# Peer Company Selection Rules

From the target stock, select peers for ripple analysis. **Two tiers:**

- **Tier 1 (Primary, 1–3):** Direct competitors / same-sector leaders — the most directly comparable companies.
- **Tier 2 (Supplementary, 0–3):** Strongly correlated upstream suppliers, downstream customers, or strategic investors/investees from a **different subsector**. Only include if they have a substantive and measurable link to the target's near-term outlook.

> **Total: 1–6 companies.** If no strong supplementary link exists, don't force it.

---

## Selection Priority (Tier 1 — Primary)

Select **1–3** in the following order:

| Priority | Type | Condition |
|----------|------|-----------|
| 1 | Same sector leader | Top-3 by market cap **across all markets (A-share/HK/US)**, with abnormal ~1M price/volume vs sector average |
| 2 | Direct competitor | Same subsector, comparable scale, recent catalyst |
| 3 | Earnings catalyst | Recently reported or reporting within 2 weeks — sector leader |

**Weighting**: Companies meeting multiple criteria rank higher. Companies with earnings within 2 weeks get +1 priority.

> ⚠️ **Priority 1 note**: "Same sector" means **across all markets**. For example, if analyzing DELL (US), competitors include Lenovo (HK) and HPE (US). Don't limit searches to the target's listing market.

## Selection Priority (Tier 2 — Supplementary)

**Only if a strong and measurable link exists.** Select **0–3**:

| Priority | Type | Condition |
|----------|------|-----------|
| 4 | Key upstream supplier | Critical component provider whose pricing/volume materially affects target's margins |
| 5 | Key downstream customer | Major buyer whose capex/spending directly drives target's orders |
| 6 | **Strategic investor / cross-ownership** | **Major shareholder (≥5%) or invested company — public company** |

> ⚠️ **Priority 6 note**: Investment relationships often cut across sectors (e.g., a tech giant investing in a space company). These are easy to miss if you only search within the same sector. **Always search for strategic investors / major shareholders during peer identification.**
>
> ⚠️ **Tier 2 filter**: Only include a Tier 2 company if:
> - It is in the **top 3** of its category (supplier/customer/investor) by revenue or ownership %, AND
> - Its recent stock price moves, earnings, or business updates have a **clear and direct** impact on the target.
> - If the link is weak or indirect → don't include.

---

## Defining "Leader"

A company qualifies as a "leader" if it meets any of:

- Top 3 by market cap in its **global** sector/industry (across A-share / HK / US markets)
- Universally recognized leader (search `{sector} leading company` or `{sector} 龙头` — top 3 results consistently point to the same name)
- Largest participant in its industry chain node (e.g., largest supplier, largest customer)

> 🌐 **Cross-market**: Leaders can be in different markets. Use market-agnostic searches (e.g., "PC manufacturers market share ranking" instead of "US PC stocks") to find peers listed elsewhere.

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
| **Investor** | **Strategic shareholder (holds stake in target or target holds stake in them)** | **Search `{company} major shareholders strategic investors`; `{company} invested in`; check S-1 / annual report "related party transactions"** |
| **Investee** | **Company that the target has invested in** | **Search `{company} strategic investments portfolio`; `{company} venture arm`** |

> ⚠️ **Multi-relationship**: A peer can have **multiple simultaneous relationships** with the target. E.g., Samsung competes with Apple in smartphones AND supplies Apple with memory chips (upstream). **Always decompose the peer's business segments before making a directional judgment — the segment most relevant to the target may not be the peer's profit driver.**

---

## Peer Business Segment Decomposition

Before analyzing any peer, **determine which business segments drive its profits, not just which segments overlap with the target.**

| Step | Search Query | Purpose |
|------|-------------|---------|
| 1 | `{peer company} revenue breakdown by segment 2026` | Get segment-level revenue/profit split |
| 2 | `{peer company} operating profit by division` | Identify the real profit driver (may differ from revenue share) |
| 3 | Compare: **segment overlapping with target** vs **segment driving peer's profit** | If they differ, the peer's stock may move counter to the target |

**Example**: Samsung competes with Apple in phones (~20% of Samsung revenue), but its semiconductor division drives ~50%+ of profit. A memory upcycle → Samsung stock rallies even as phone market shrinks.

**Rule**: **Don't assume the overlapping segment is the peer's profit driver. Always check the full segment breakdown before making a directional prediction.**

---

## Industry Chain & Investment Identification Flow

1. WebSearch: `{company name} industry chain main competitors` (market-agnostic — don't add market qualifier)
2. WebSearch: `{sector/industry} top companies global market share ranking` — find global leaders across all markets
3. WebSearch: `{company name} main suppliers customers`
4. **WebSearch: `{company name} major shareholders strategic investors`** — find who owns or has invested in the company
5. **WebSearch: `{company name} strategic investments portfolio subsidiaries`** — find who the company has invested in
6. Extract 2–3 frequently mentioned company names from results
7. Verify they are publicly traded (have a ticker/code) — **note their listing market**
8. Filter by market cap / industry position to select leaders, **preferring a mix of markets when equally matched**

### After selecting peers, decompose each peer's business:

9. WebSearch: `{peer company} revenue breakdown by segment` — identify the peer's true profit driver
10. Compare: does the peer's profit driver align with the segment overlapping the target, or is it something different?
    - **If aligned** → the overlap segment moves the peer's stock → standard bidirectional logic applies
    - **If different** → the peer may move independently or opposite to the target → **flag this explicitly in the prediction**

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
- **Recent policy/regulatory change** affecting the sector (tariffs, subsidies, antitrust, new laws — even if stock price hasn't reacted yet)

WebSearch templates:
```
{company name} stock price anomaly recent month
{company name} major announcement 2026
{company name} analyst rating change 2026
```

---

## Final Checklist

After selecting peers, confirm:

- [ ] **Tier 1**: 1–3 primary competitors/direct peers
- [ ] **Tier 2** (optional): 0–3 supplementary upstream/downstream/investor peers **only if strongly correlated**
- [ ] Total ≤ 6
- [ ] All publicly traded — confirmed ticker/code
- [ ] **At least 1 peer from a different market than the target** (if a global leader exists elsewhere)
- [ ] Substantive business link to target (not pure concept play)
- [ ] At least 1 with a near-term earnings event (already reported or upcoming)
- [ ] No ST / delisting risk
- [ ] Relationship type marked (same sector / upstream / downstream / competitor / **investor / investee**)
- [ ] **Investment relationships searched** — did not limit to same-sector peers only
- [ ] **Cross-market search done** — searched `{industry} global top companies ranking` not just `{market} {sector} stocks`
- [ ] **⚠️ Business segment decomposed** — searched `{peer} revenue breakdown by segment` for each peer; confirmed whether the overlapping segment is the peer's real profit driver
- [ ] **Multi-relationship flagged** — if peer has multiple relationships with target (e.g., competitor + supplier), all are noted
- [ ] **Tier 2 filter applied** — no weak/indirect links included just to fill space
