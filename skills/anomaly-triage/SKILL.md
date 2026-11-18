---
name: anomaly-triage
description: Triage a sudden change in paid media performance (spend spike or drop, CPA or CPM jump, conversions falling to zero, ROAS collapse, delivery stopping) across Meta, Google Ads, TikTok, Snapchat, LinkedIn, X, DV360 and other platforms. Separates data and tracking problems from delivery, change-log, auction and seasonality causes using a fixed decision order, assigns severity, and recommends a contained first response. Use when someone says performance suddenly changed, spend went crazy, conversions dropped, an alert fired, or asks "what happened yesterday".
---

# Anomaly triage

When numbers jump, people change things too quickly and make it worse. Confirm the anomaly is real, find its cause in a fixed order, contain the damage, and only then optimize.

## Inputs

- The metric and entity that moved, and when (with the account time zone).
- Daily data for the last 28 days (hourly for the last 48 hours if available) for the affected entity and its parent account.
- The change history (activity log) for the last 7 days.
- Tracking health: event counts by day from the pixel or server, GA4 sessions and key events, MMP installs.

## Step 1: is it real?

Compare against the **same weekday** average of the previous 4 weeks, not just yesterday.

| Flag | Default threshold |
|---|---|
| Spend | ±50% vs the same-weekday baseline, or above 1.5x the daily budget |
| CPA / CPL | +40% with at least 10 conversions in the baseline |
| CPM | +50% |
| Conversions | -50%, or 0 when the baseline is at least 5 a day |
| CTR | -30% with at least 5,000 impressions a day |

Rule out data artifacts before anything else: today is a partial day, the time zone differs between tools, conversions for recent days are still filling in on long attribution windows, or there is a reporting outage on the platform's status page. If it is an artifact, stop here.

## Step 2: find the cause, in this order

1. **Tracking.** Clicks and landing page sessions are stable but conversions dropped: the pixel or tag was removed or changed, consent banner changes, a checkout redesign, server events failing, dedup broken (conversions suddenly doubled), or a site outage. Check GA4 or the MMP to see whether the business saw the drop too.
2. **Change log.** Any edit in the 72 hours before the anomaly: budget, bid or cap changes, new ads (learning reset), audience or placement changes, paused winners, new campaigns competing for the same audience.
3. **Delivery problems.** Disapproved ads, account or payment holds, a spending limit reached, an end date passed, a lead form or catalog error, an app store link broken.
4. **Auction and seasonality.** CPM moved across the whole account or platform: competitor pressure, holidays and sales events (Ramadan, Eid, White Friday, Black Friday, Q4, end of quarter), or major news. Check whether other accounts in the same market moved too.
5. **Creative.** One ad or concept collapsed in CTR (see creative-fatigue-detector).
6. **Platform-side change.** New automation turned on by default, a placement added, an attribution or reporting change announced.

## Step 3: severity

| Severity | Definition | Response time |
|---|---|---|
| P1 | Money leaving with no results (spend at 1.5x or more of budget with 0 conversions, or tracking broken on a conversion-optimized campaign), or delivery stopped on a key campaign | Within the hour |
| P2 | KPI more than 40% off target for a day on meaningful spend | Same day |
| P3 | Drift that persists 3 or more days | This week |

## Step 4: contain, don't optimize

- **P1 spend runaway or broken tracking**: lower the budget or pause only the affected entities. Do not touch healthy campaigns. Record what was changed so it can be undone.
- **Tracking broken**: fix tracking first. Pausing conversion-optimized campaigns for more than a few days resets learning, and lowering the budget is often safer.
- **Auction or seasonality**: hold. Decide on budgets with the client rather than reacting to one day.
- **Change-log cause**: revert the specific change if it is clearly the cause and was made less than 72 hours ago.

## Output

1. One-paragraph incident summary: what moved, since when, how much money is involved, the most likely cause and the confidence (high, medium or low).
2. Evidence table: hypothesis, check, result.
3. Contained actions taken or proposed (**draft**), each with an undo step.
4. Follow-ups: owner, check time, and the metric that confirms recovery.
5. Prevention: an alert or guardrail that would have caught it earlier.

## Live data (optional)

Works with exports and screenshots of the activity log. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=anomaly-triage) connected, Claude can pull the daily and hourly data and change history live. Whathead's Anomaly Engine runs setup, pacing and performance checks continuously for Meta, Snapchat, TikTok, X and DV360, so the alert often arrives before anyone opens a report.
