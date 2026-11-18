---
name: budget-pacing-check
description: Check whether paid media budgets are pacing to plan across Meta, Google Ads, TikTok, Snapchat, LinkedIn, X, DV360 and other platforms for a month, flight or insertion order. Computes expected vs actual spend, projected end-of-period spend and the daily budget needed to land on target, with weekday and seasonality adjustments (including GCC weekends and Ramadan). Use when someone asks about pacing, over- or underspend, whether a budget will be spent by month end, or how to adjust daily budgets to hit a monthly plan.
---

# Budget pacing check

Tell the user, per campaign or line item, whether spend will land on plan, by how much it will miss, and the exact daily budget that fixes it.

## Inputs

- **Plan**: planned budget per campaign, platform or IO, with flight start and end dates. A media plan sheet works.
- **Actuals**: spend to date per day (daily rows are best), in the plan's currency, through yesterday.
- **Current daily or lifetime budget settings** per entity.
- Market(s) and any known events in the flight (sales, holidays, Ramadan, launches).

## Steps

1. **Align the data.** Use the account time zone and exclude today, which is a partial day. Convert currencies at one stated rate. Note platform reporting lags. Spend for the last 24 hours can still change, and DV360 and some reporting APIs settle later.
2. **Expected spend to date.**
   - Straight line: `expected = plan x elapsed_days / total_days`.
   - Weighted (preferred when 4 or more weeks of history exist): give each remaining day a weight from the account's own day-of-week spend share over the last 8 weeks. Then `expected = plan x sum(weights of elapsed days) / sum(weights of all days)`.
   - Seasonality: if the flight includes a planned peak (for example the last 10 nights of Ramadan, White Friday or Black Friday, or a product launch), use the plan's phasing if it has one. Otherwise ask before assuming a flat curve.
3. **Pacing ratio** = `actual_to_date / expected_to_date`.
4. **Projection** = `actual_to_date + current_daily_run_rate x remaining_days`. Use the average of the last 3 complete days as the run rate, not the configured budget. Platforms do not spend budgets exactly: Meta can spend up to 75% above a daily budget on a given day while keeping the week within 7x daily, and Google Ads can spend up to 2x on a day while capping the month at about 30.4x the average daily budget.
5. **Required daily budget** = `(plan - actual_to_date) / remaining_days`. If daily weighting is used, spread it by the remaining days' weights.
6. **Classify** each entity.

## Status rules (defaults)

| Status | Pacing ratio | Action |
|---|---|---|
| On track | 0.95 to 1.05 | None |
| Watch | 0.90 to 0.95 or 1.05 to 1.10 | Re-check in 2 days |
| Under-pacing | below 0.90 | Raise the budget toward the required daily, check delivery limits |
| Over-pacing | above 1.10 | Lower the budget to the required daily; check for runaway ad sets |
| Critical | below 0.75 or above 1.25, or projected to exhaust before flight end | Fix today |

Tighten the bands in the last 7 days of a flight. A 10% miss with 3 days left cannot be recovered without sharp changes.

## Diagnose under-pacing before raising budgets

Raising the budget does not help if delivery is constrained. Check:

- Bid caps or cost caps below the market (look for low impression share or an "auction" constraint)
- Audiences that are too small or exhausted (high frequency)
- Disapproved ads, account spending limits, payment failures
- Learning-limited ad sets
- Schedules or end dates that already passed
- On Google: "Limited by budget" shows a budget constraint, while lost impression share to rank shows a bid or quality constraint

## Safe adjustment guidance

- Change budgets in steps of 20 to 30% per day on Meta, TikTok and Snapchat to avoid resetting learning. If a larger change is needed, split it across days or move budget to a duplicate.
- On lifetime budgets, change the end date or lifetime amount rather than adding daily budgets.
- Shift budget between platforms only when the receiving campaign has headroom (CPA at or under target and no delivery constraint).

## Output

1. Headline: total plan, spent, projected landing and gap, for example "Projected 92% of plan. Short by 18,400 SAR".
2. Table: entity, plan, spent, expected to date, pacing ratio, projection, required daily, current daily, status, note.
3. Proposed budget changes as a **draft** list (entity, from, to, reason), sized to the step limits above.
4. Assumptions: weighting method, run-rate window, FX rate, events considered.

## Live data (optional)

Works with any export or media plan sheet. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=budget-pacing-check) connected, Claude can read live spend and budgets across platforms and prepare the budget changes as dry runs for your approval.
