---
name: snapchat-tiktok-setup-review
description: Review Snapchat Ads and TikTok Ads account and campaign setup for common mistakes, covering pixel and Conversions/Events API, deduplication, optimization goals, budgets and learning, placements, automation settings (Smart+, ACO, automatic placement, Search toggle), identity and Spark Ads, creative specs, attribution windows and lead forms. Use when someone asks to review or audit a Snapchat or TikTok account, why Snap or TikTok underperforms Meta, how to set up Snap or TikTok campaigns properly, or before scaling spend on either platform, including in GCC and MENA markets where Snapchat is a major channel.
---

# Snapchat and TikTok setup review

Snapchat and TikTok are often built by copying Meta habits, which costs money on both. This review checks the settings that matter most on each platform and explains how they differ from Meta.

## Inputs

- Account-level: pixel(s), server-side integration (Snap Conversions API, TikTok Events API), events received in the last 7 days with counts, attribution settings, and business identity (Snap Public Profile, TikTok identity or Business Center).
- Campaign export: objective, optimization goal and event, bid strategy, budgets, placements, targeting, automation toggles, schedule.
- Ad export: format, dimensions, length, text, CTA, landing URL, identity, Spark or creator authorization.
- 30-day performance by campaign, ad group/ad squad and ad.

## Shared checks (both platforms)

| Check | Pass condition |
|---|---|
| Server-side events | Conversions/Events API is sending the optimization event alongside the pixel, with a shared event ID for deduplication |
| Event volume | The optimization event fires often enough. Each ad group/ad squad should get about 50 conversions a week; if it can't, optimize for an earlier event (add to cart, start checkout) or consolidate. |
| Match keys | Server events include hashed email and phone and the platform click ID where consented |
| Vertical creative | Native 9:16 video at 1080x1920, sound designed in, captions, the hook in the first 2 seconds, text inside safe zones. Repurposed 1:1 or 4:5 Meta assets are a High finding. |
| Creative volume | At least 3 to 5 distinct creatives per ad group/ad squad, with new variants every 1 to 3 weeks at scale |
| Attribution | The attribution windows used for reporting are known and documented, and comparisons with Meta use comparable windows |
| UTMs | Present on all ads. TikTok macros: `__CAMPAIGN_NAME__`, `__AID_NAME__`, `__CID_NAME__`. |
| Budget changes | Moves of at most 20 to 30% a day on scaling ad groups, to protect learning |

## Snapchat-specific

- [ ] **Objective and goal**: Sales or App Promotion campaigns optimize for the pixel or app purchase event, not swipes, when the goal is sales. Swipe optimization is a common default that buys cheap, low-intent traffic.
- [ ] **Public Profile** is attached, and the brand name and headline fit Snap's short limits (about 25 and 34 characters). Text is rewritten for Snap, not truncated.
- [ ] **Placements**: automatic placements unless there is a reason to exclude. If placements are manual, the review says why, for example excluding content placements for brand safety.
- [ ] **Audience size**: audiences big enough for the budget. In smaller GCC markets (Kuwait, Bahrain, Qatar), narrow interest stacks saturate fast, so watch frequency.
- [ ] **Bid strategy**: auto-bid first. Target cost only after stable volume, and not set below the actual CPA.
- [ ] **Lead forms** (Leads objective): the form's fields match what the CRM needs, the privacy policy link is present, and leads are exported or webhooked into the CRM daily, not left in the platform.
- [ ] **Dedup ID** is set on both pixel and Conversions API events.
- [ ] **Arabic creative** where the audience is Arabic-speaking, with right-to-left text checked on device.

## TikTok-specific

- [ ] **Budget minimums**: at least $50 a day per campaign and $20 a day per ad group (USD, or the equivalent in the account currency). Budgets near the minimum rarely exit learning on conversion goals.
- [ ] **Optimization event**: Complete Payment (or the purchase event) for sales. Clicks or landing page views are only for traffic goals.
- [ ] **Smart+ / automation**: if Smart+ campaigns, automatic targeting, Automated Creative Optimization (ACO) or the Search Ads toggle are on, it is a deliberate choice recorded in the plan. Each one moves control to TikTok and changes what reports can show.
- [ ] **Placements**: "TikTok only" vs automatic placements that add partner inventory such as Pangle where available. Partner app inventory can inflate clicks, so check CTR and conversion rate by placement.
- [ ] **Identity and Spark Ads**: Spark Ads use valid authorization codes that will not expire mid-flight. Creator permissions cover the flight dates and paid usage rights.
- [ ] **Comments**: comment settings and moderation are decided (on or off, keyword filters). Unmoderated comments under ads are a brand risk.
- [ ] **Learning**: ad groups are not edited repeatedly during learning. Edits are batched.
- [ ] **Ad text** is at most 100 characters, written natively (no Meta-style long copy), with a clear CTA.

## How these differ from Meta (for teams used to Meta)

- Broad targeting with strong creative usually beats detailed interest stacks on both platforms.
- Creative wears out faster on TikTok and Snapchat. Plan production capacity before scaling budget.
- Platform-reported conversions use different windows and models from Meta. Use GA4 or an MMP as the comparison baseline.

## Output

1. Scorecard per platform: Pass/Fail per check, with severity (Critical, High, Medium, Low).
2. Top 5 fixes by expected impact, each with the exact setting to change.
3. A creative brief if the creative checks fail (formats, number of variants, hook ideas from the best ad).
4. All changes listed as **drafts** for approval.

## Live data (optional)

Works with exports. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=snapchat-tiktok-setup-review) connected, Claude can read Snapchat and TikTok settings, events and performance live, check automation flags across every ad group, and draft fixes as dry runs. Whathead covers Snapchat with full read and write, including lead forms.
