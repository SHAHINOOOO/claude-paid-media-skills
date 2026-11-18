---
name: campaign-launch-checklist
description: Run a pre-launch QA checklist on new paid media campaigns (Meta, Google Ads, TikTok, Snapchat, LinkedIn, X, Reddit, DV360, Microsoft Ads) against the brief or media plan, covering objective, tracking, budget, dates, targeting, exclusions, placements, creative specs, copy, policy, URLs and naming, plus first-72-hour monitoring triggers. Use when someone is about to launch, asks to QA a campaign, check a build against a brief or media plan, or wants a go/no-go before spend starts.
---

# Campaign launch checklist

Catch build errors before money is spent. Compare what was built with what was approved, item by item, and give a clear go or no-go.

## Inputs

- The brief or media plan row(s): platform, objective, KPI, budget, flight dates, markets, audiences, placements, creatives, landing URLs.
- The built entities (export or screenshots): campaign, ad sets/ad groups, ads, with every setting.
- Tracking setup: the optimization event and where it fires.

## Checklist

Mark each item Pass, Fail or N/A, and add evidence for every Fail.

**A. Goal and measurement**
- [ ] Objective matches the brief (sales, leads, app, traffic, awareness)
- [ ] Optimization event is the one in the brief, exists on this pixel or app, and fired in the last 24 hours
- [ ] Purchase events send value and currency
- [ ] Pixel and server events are deduplicated
- [ ] Attribution setting matches the client's reporting standard

**B. Budget and schedule**
- [ ] Budget type (daily or lifetime) and amount match the plan, in the account currency
- [ ] Start and end dates and times are right in the **account time zone**. A Riyadh client in a UTC account starts 3 hours off.
- [ ] Budget clears platform minimums, and can reach about 50 optimization events a week per ad set (`daily ≈ target CPA x 50 / 7`), or the plan accepts a learning-limited start
- [ ] Bid strategy and any caps match the plan. Caps are not below recent actual CPA.
- [ ] Account spending limit and payment method will not stop delivery mid-flight

**C. Targeting**
- [ ] Locations match. On Google, check "presence" vs "presence or interest".
- [ ] Age, gender and languages match. Arabic-language targeting is set if the creative is Arabic.
- [ ] Audiences are the approved ones. Lookalike and custom audiences are populated (not "too small").
- [ ] Exclusions are in place: purchasers or existing leads (in prospecting), employees, other active campaigns' audiences where the plan separates them
- [ ] Special ad categories are declared where required (Meta: financial products and services, employment, housing, social issues, elections or politics)

**D. Placements and creative**
- [ ] Placements match the plan (automatic or manual)
- [ ] Every placement has an asset in the right ratio (9:16 for Stories, Reels, TikTok and Snapchat; 1:1 or 4:5 for feeds; 16:9 for in-stream). No letterboxed or stretched assets.
- [ ] Text and logos sit inside the safe zones
- [ ] Videos have captions and sound designed for sound-on placements
- [ ] Identity is set: Meta Page and Instagram account, TikTok identity or Spark authorization, Snapchat Public Profile, X handle
- [ ] Copy is within limits, free of typos, in the right language and dialect, with the right CTA button

**E. URLs and naming**
- [ ] Final URLs return 200, load on mobile and match the ad's offer and market
- [ ] UTMs are present and use macros (see utm-tracking-qa)
- [ ] Names follow the convention (see naming-convention-audit)

**F. Policy and approvals**
- [ ] Claims (prices, discounts, "best", health or financial claims) match the landing page and local rules
- [ ] Client approval of the final creatives and copy is recorded
- [ ] All ads are approved, or none are in a rejected state at go time

## Go / no-go

- **No-go** if any item in A fails, if dates or time zone are wrong, or if a URL fails.
- **Go with notes** if only D or E items fail and the fixes are scheduled before significant spend.
- Launch active only after go. Until then, keep entities paused or in draft.

## First 72 hours: watch triggers (defaults)

| Time | Trigger | Likely cause |
|---|---|---|
| 2 h | No impressions | Not approved, scheduling, payment, audience too small, bid cap too low |
| 24 h | Spend below 50% of daily budget | Delivery constraint (see budget-pacing-check) |
| 24 h | CPM above 2x the account's 30-day median | Narrow audience, competitive auction, placement restriction |
| 24 to 48 h | Clicks but zero landing page sessions in GA4 | Broken URL or redirect, missing UTMs, consent blocking |
| 48 to 72 h | Spend at least 2x target CPA with no conversions on a high-volume event | Tracking, or the wrong optimization event |
| 72 h | Frequency above 2.5 on prospecting | Audience too small for the budget |

Avoid optimizing on day 1 to 2 data except for the defects above.

## Output

1. Go / no-go with the blocking items.
2. Checklist with Pass, Fail and N/A, and evidence for every Fail.
3. Fix list (entity, setting, current, should be), marked **draft**.
4. The 72-hour watch list with dates and times in the account time zone.

## Live data (optional)

Works with exports and screenshots. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=campaign-launch-checklist) connected, Claude can read the built campaigns live and compare them with the media plan. Whathead's workspace can also build the campaigns from the plan as drafts in the first place.
