---
name: creative-fatigue-detector
description: Detect creative fatigue and audience saturation in paid social and video ads on Meta, TikTok, Snapchat, X, LinkedIn and YouTube by comparing each ad's early performance with its recent performance (CTR, hook rate, CPM, CPA, frequency). Separates true fatigue from auction or tracking changes and recommends refresh, rotation or budget moves. Use when someone asks whether ads are fatigued, why CTR or ROAS is dropping, which creatives to refresh, or how often to rotate creative.
---

# Creative fatigue detector

Decide for each ad whether it is fatigued, saturated, hurt by the auction, or fine, using its own history as the baseline.

## Inputs

Daily rows per ad for at least 21 days (ideally since launch): date, ad ID and name, ad set, spend, impressions, reach (if available), frequency, clicks (link clicks), CTR, CPM, conversions, CPA or ROAS. For video, add 2- or 3-second views (hook) and completions (ThruPlay, 6-second views or 100% views).

## Minimum data

Judge an ad only if it has at least 7 days live, at least 5,000 impressions in each comparison window, and at least 20 clicks overall. Mark others "too early".

## Method

1. **Windows.** Baseline = the ad's first 7 full days after it reached 1,000 impressions per day, or its best 7-day period if it launched before the data starts. Recent = the last 7 full days.
2. **Deltas** for CTR, CPM, CPA (or ROAS), hook rate (3-second views / impressions) and frequency.
3. **Classify** using the rules below, in order.

| Signal pattern (recent vs baseline) | Diagnosis | Typical fix |
|---|---|---|
| CTR down at least 25%, frequency up, CPM flat (within ±15%) | **Creative fatigue**: the same people have seen it enough | New hook or first 3 seconds, new variant of the concept, rotate |
| CTR down, frequency high (prospecting above 3 in 7 days, retargeting above 8) and reach growth slowing | **Audience saturation** | Broaden the audience, add new audiences, or lower the budget |
| CPM up at least 30% across most ads in the account at the same time, CTR stable | **Auction pressure**, not fatigue | Seasonality (Ramadan, White Friday, Q4), competitors; adjust bids and budgets, not creative |
| Conversions drop but CTR and landing page sessions are stable | **Tracking or site problem**, not fatigue | Run utm-tracking-qa and check the site |
| Hook rate down at least 20% with CTR stable | Early fatigue on video | Test a new opening before CTR follows |
| Deltas within ±15% | **Healthy** | Keep running |

4. **Account view.** Share of spend on fatigued ads, average age of the top 10 spenders, and how many new creatives went live in the last 14 days.

## Platform notes

- **TikTok and Snapchat** creatives usually wear out faster than Meta Feed creatives at the same spend. Plan new variants every 1 to 3 weeks at scale.
- **Meta**: check the frequency breakdown by placement. Reels and Stories can saturate while Feed is fine. Dynamic or flexible creative hides per-asset fatigue, so look at the asset breakdown.
- **Retargeting** tolerates higher frequency than prospecting, but CPA should stay within target.
- **Spark Ads / creator posts**: refresh by rotating in new creator posts instead of editing the existing one, so engagement history stays intact.

## Refresh guidance

- Keep winning concepts and change the hook, first frame, text overlay, music or creator. Do not discard the concept.
- Add new ads alongside the fatigued ones before pausing them, then pause once the new ones have delivery, to avoid a delivery drop.
- On Meta, adding ads to an ad set restarts learning. Batch additions into one edit.

## Output

1. Summary: share of spend on fatigued or saturated ads, and the top 3 actions.
2. Table: ad, age (days), spend in the recent window, baseline and recent CTR, CPM, CPA, frequency, hook rate, diagnosis, recommended action.
3. Refresh brief per fatigued concept: what worked (from the baseline), what to change, formats needed per platform.

Thresholds are defaults. Adjust them for the account's normal volatility and state which ones you used.

## Live data (optional)

Works with daily ad exports. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=creative-fatigue-detector) connected, Claude can pull daily ad-level metrics from Meta, TikTok, Snapchat, X and LinkedIn live, and draft pauses or new ad drafts as dry runs.
