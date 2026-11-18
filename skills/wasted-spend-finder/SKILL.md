---
name: wasted-spend-finder
description: Find and quantify wasted ad spend across Meta, Google Ads, TikTok, Snapchat, LinkedIn, X, Microsoft Ads and DV360, covering zero-conversion spenders, irrelevant search terms, poor placements, frequency waste, geo and schedule leakage, and broken landing pages. Returns a ranked list with the money involved and a safe fix for each item. Use when someone asks where budget is being wasted, how to cut spend without losing results, which ads or keywords to pause, or why CPA went up.
---

# Wasted spend finder

Find spend that is very unlikely to produce results, put a number on it, and propose the smallest safe fix. Recommend only; pausing and excluding are the user's decision.

## Inputs

- Target CPA (or target ROAS) per campaign. If none is given, use the account's trailing 30-day CPA and say so.
- 30 days (minimum 14) of performance at the most granular level available: ad, ad set/ad group, keyword and search term (Google, Microsoft), placement/publisher (Meta Audience Network, TikTok Pangle, DV360 sites/apps, Google Display/PMax placement reports), geo, hour of day, device.
- Landing URLs for active ads.

Any source works: platform exports, a sheet, or an MCP connection.

## Checks

Run every check that the data allows. For each, record rows flagged, spend, conversions and the rule applied.

| # | Check | Default rule | Notes |
|---|---|---|---|
| 1 | Zero-conversion spenders | Spend at least 2x target CPA with 0 conversions over the window | Use 3x for high-CPA B2B or long sales cycles. Skip entities younger than 7 days. |
| 2 | Chronic over-target | CPA above 2x target with at least 3 conversions over 30 days | Fix may be bid or budget, not pause. |
| 3 | Irrelevant search terms (Google, Microsoft) | Terms with spend at least 1x target CPA and 0 conversions, plus terms that are off-intent (jobs, free, competitor support, wrong product) at any spend | Propose negatives at the right level (campaign vs shared list) and match type. Never negate your brand. |
| 4 | Placement waste | Placements, apps or sites with CTR above 3x account average and conversion rate under 0.25x (accidental clicks), or spend at least 1x target CPA with 0 conversions | Common on in-app inventory. Check before excluding whole networks. |
| 5 | Frequency waste | Prospecting 7-day frequency above 4, retargeting above 10, with CTR falling week over week | Cap or refresh; see creative-fatigue-detector. |
| 6 | Geo leakage | Spend in locations outside the brief, or Google location option set to "presence or interest" for a local business | Check country and city reports. |
| 7 | Schedule waste | Hours or days with CPA above 2x the account average and at least 10% of spend | Account for time zone and local weekends (for example Friday to Saturday in KSA, Egypt and Kuwait; Saturday to Sunday in the UAE). Do not daypart on less than 4 weeks of data. |
| 8 | Device or OS mismatch | Device with CPA above 2x average, or app-install campaigns serving to the wrong OS | |
| 9 | Broken or slow landing pages | Final URL returns 4xx/5xx, redirects more than twice, or drops UTM parameters | Critical: 100% of that ad's spend counts as waste until fixed. |
| 10 | Audience overlap | Two or more prospecting ad sets with the same audience in the same account | Wasted through self-competition; report as risk, do not count as dollars. |
| 11 | Untracked spend | Campaigns with spend and no conversion event configured, or an event that stopped firing | Report separately as "unmeasured", not "wasted". |

## Quantify

- **Flagged spend** = total spend of rows matched by checks 1, 3, 4, 6, 7, 8 and 9. De-duplicate rows matched by more than one check.
- **Recoverable estimate** = flagged spend x 0.7 by default. Some of it will move to other inventory, and some conversions are attributed late. Say that this is an estimate.
- Express it as both money and share of total spend for the window, and annualize it only if asked.

## Fixes, safest first

1. Exclusions (negative keywords, placement exclusions, location exclusions). Low risk and reversible.
2. Bid or budget reductions of 20 to 30% on chronic over-target entities.
3. Pausing zero-conversion entities that have had a fair test.
4. Structural changes (consolidation, new campaigns). Hand these over to the ad-account-audit skill.

On Meta, pausing an ad or ad set and large budget changes can restart learning. Batch changes and make them once, not daily.

## Output

1. One-line headline: "About X (Y% of spend) in the last N days went to entities very unlikely to convert. The biggest source is ...".
2. Table: check, entity, spend, conversions, CPA vs target, rule, proposed fix, risk.
3. A ready-to-apply list per platform (negatives, exclusions, pauses), marked **draft**.
4. What was not checked because data was missing.

## Live data (optional)

Works with any export. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=wasted-spend-finder) connected, Claude can pull ad, keyword, placement and geo breakdowns live and prepare the exclusions as dry runs. Nothing changes until you confirm.
