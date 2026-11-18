---
name: ad-account-audit
description: Run a structured health audit of a paid media ad account (Meta, Google Ads, TikTok, Snapchat, LinkedIn, X, Reddit, DV360, Microsoft Ads) and return a scored findings list with fixes ranked by impact. Use when someone asks to audit an ad account, review account structure, check account health, prepare for a client handover or new-client onboarding, or find out why an account underperforms.
---

# Ad account audit

Produce a prioritized, evidence-backed audit of one ad account. Every finding must cite the rows it came from and estimate the money or risk involved. Recommend changes only; never apply them.

## Inputs

Ask for whatever is missing, in this order:

1. **Platform, account name, currency, time zone, business goal** (sales, leads, installs, awareness) and the target KPI (target CPA, ROAS or CPL).
2. **Last 30 days of performance** at campaign, ad set/ad group and ad level: spend, impressions, clicks, conversions (the optimization event), conversion value, frequency where available. Also the previous 30 days if possible.
3. **Settings export**: objective, optimization goal/event, bid strategy, budget type and amount, schedule, locations, ages, languages, placements, audiences (included and excluded), attribution setting, status.
4. **Tracking**: which conversion events exist and whether they fire (pixel/tag diagnostics, Events Manager, Google Ads conversion status).

Any source works: CSV exports from the ads manager, a Google Sheet, a BI extract, or an MCP connection. If data is partial, audit what you have and list what is missing as a finding.

## Steps

1. **Sanity-check the data.** Same date range everywhere, one currency, account time zone noted. Remove today's partial day. Flag reporting gaps.
2. **Measurement first.** If the optimization event is missing, firing on the wrong page, duplicated, or has no value on purchase events, stop and report it as Critical. Every other number depends on it.
3. **Structure.** Count campaigns, ad sets and ads with spend. Flag fragmentation: ad sets that individually cannot reach the platform's learning volume (on Meta, about 50 optimization events in 7 days per ad set; TikTok asks for about 50 conversions per ad group). Flag duplicated targeting across ad sets competing in the same auction.
4. **Objective and optimization fit.** The objective and optimization event should match the business goal (a sales goal optimized for link clicks is a Critical finding).
5. **Budget and bidding.** Budget concentration (share of spend in top 3 campaigns), budgets too small to exit learning (daily budget below roughly target CPA x 50 / 7), bid caps or cost caps set below recent actual CPA (causes under-delivery), "limited by budget" campaigns with CPA under target (scaling opportunity).
6. **Targeting.** Overly narrow audiences, missing exclusions (existing customers in prospecting, converters in retargeting), location settings (Google: "presence or interest" vs "presence"), language and age mismatches with the brief.
7. **Creative.** Number of active ads per ad set (fewer than 3 limits testing; more than 10 to 15 per ad set fragments delivery), formats per placement (9:16 for Stories/Reels/TikTok/Snap), ads older than 60 days with declining CTR (hand over to the creative-fatigue-detector skill).
8. **Waste.** Run the checks from the wasted-spend-finder skill: zero-conversion spend, bad placements, irrelevant search terms, frequency waste.
9. **Hygiene.** Naming convention compliance, UTMs on every ad, disapproved or limited ads, payment or account-level warnings.
10. **Score and rank.**

## Severity and scoring

| Severity | Rule of thumb | Examples |
|---|---|---|
| Critical | Breaks measurement or wastes more than 10% of spend | No or duplicated conversion event; wrong optimization goal; spend on broken landing pages |
| High | Costs 3 to 10% of spend or blocks scaling | Learning-limited structure; bid caps below actual CPA; missing customer exclusions |
| Medium | Efficiency or clarity loss under 3% | Creative gaps on some placements; inconsistent naming |
| Low | Hygiene | Missing UTMs on a few ads; stale paused objects |

Account score: start at 100 and subtract 20 per Critical, 8 per High, 3 per Medium and 1 per Low, with a floor of 0. Show the score with the counts so it can be compared month to month.

## Output

1. **Summary (5 lines max):** score, the 3 changes with the largest expected impact, and the money at stake.
2. **Findings table:** severity, area, finding, evidence (entity names and metrics), estimated impact, recommended fix, effort (S/M/L).
3. **Quick wins this week** (fixes doable in under an hour).
4. **Data gaps** that limited the audit.
5. A note that every change should be made as a draft or dry run and reviewed before going live.

Thresholds here are starting defaults. Tune them per account and say which ones you used.

## Live data (optional)

The skill works with any export. If you connect the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=ad-account-audit), Claude can pull settings and 30-day performance live for Meta, Google Ads, TikTok, Snapchat, LinkedIn, X, Reddit, DV360, Microsoft Ads, Apple Ads, Amazon Ads and Criteo, then draft fixes as dry runs that you approve.
