---
name: weekly-performance-report
description: Write a weekly paid media performance report across Meta, Google Ads, TikTok, Snapchat, LinkedIn, X, DV360 and other platforms, with headline insights, KPI tables vs last week and vs target, top movers, what changed, attribution caveats and next-week actions with owners. Use when someone asks for a weekly report, client update, performance summary, WoW comparison, or a status email for stakeholders.
---

# Weekly performance report

Write the report a busy client or manager reads in two minutes: what happened, why, and what we will do. Numbers support the story; they are not the story.

## Inputs

- Two complete, equal-length periods, normally this week and last week. Use the account's reporting week (Monday to Sunday, or Sunday to Saturday where the work week starts on Sunday, as in KSA). Exclude today.
- Per platform and campaign: spend, impressions, clicks, conversions (by type), conversion value, and the platform's attribution setting.
- Targets: monthly budget, target CPA/ROAS/CPL, and month-to-date spend for pacing.
- Change log for the week: launches, pauses, budget moves, creative swaps, tracking changes. Ask for it. It explains most movements.
- Optional: GA4 or MMP (Adjust, AppsFlyer) numbers as the neutral source of truth.

## Steps

1. **Validate**: periods are equal length, currencies converted at one rate, no platform missing, and no partial day. Note late data. Conversions for the last 1 to 3 days are often still filling in, especially on longer click windows.
2. **Topline KPIs**: spend, conversions, CPA (or CPL), conversion value, ROAS, CTR and CPM, for this week, last week, change %, target and status. Show both platform-reported and source-of-truth (GA4/MMP) conversions if both exist, labeled.
3. **By platform**: the same KPIs, plus each platform's share of spend and of conversions. A platform whose share of conversions is far below its share of spend is the first place to look.
4. **Movers**: the top 3 improvements and top 3 declines by absolute impact (change in conversions or in CPA x spend), not by percentage, so tiny campaigns don't dominate.
5. **Explain**: link each mover to the change log, seasonality (pay-day weeks, Ramadan, Eid, White Friday, school holidays), auction (CPM change), creative (CTR change) or tracking (conversion rate change with stable clicks).
6. **Pacing**: month-to-date vs plan (see the budget-pacing-check skill).
7. **Actions**: 3 to 5 specific actions for next week, each with an owner and a date, and the metric that will show whether it worked.

## Writing rules

- Lead with a 3-bullet headline: the result vs target, the main driver, and the main action.
- Use absolute numbers with percentages ("CPA 41 SAR, down 12% from 47 SAR").
- Call a change of under 5% on small volumes "flat". Do not narrate noise.
- Never mix attribution models in one number. Label platform-reported figures as such.
- Do not claim causation from correlation. Say "coincided with" unless there is a test.
- Keep the body under one page. Put detailed tables in an appendix.

## Attribution caveats to include when relevant

- Platforms use different default windows (for example Meta 7-day click and 1-day view). Summing platform conversions overstates the total, because the same conversion can be claimed by several platforms.
- iOS app campaigns report through SKAdNetwork or modeled data with delays. Compare them to MMP data, not day by day.
- View-through conversions inflate upper-funnel platforms. Report them separately if they are large.

## Output template

```
Week of <dates> · <client> · prepared <date>

Headline
- <result vs target>
- <main driver>
- <main action>

KPIs (this week · last week · change · target)
<table>

By platform
<table>

What moved and why
1. ...

Pacing (month to date)
<spent> of <plan> (<x%>), projected <y%>

Next week
| Action | Owner | Due | Success metric |

Notes on data
<attribution, late data, tracking changes>
```

## Live data (optional)

Works with any export or sheet. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=weekly-performance-report) connected, Claude can pull both weeks live from every connected platform plus GA4, and Whathead's Google Sheets add-on can keep the same tables refreshed on a schedule.
