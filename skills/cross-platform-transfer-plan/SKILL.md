---
name: cross-platform-transfer-plan
description: Plan how to move a winning campaign from one ad platform to another (for example Meta to TikTok, Snapchat, Google, X, LinkedIn or Reddit). Maps objective, optimization event, audiences, placements, creative specs, copy limits, budget and tracking, lists what will not carry over, and produces a launch-ready transfer sheet. Use when someone asks to copy, clone, port, replicate or transfer a campaign to another platform, expand a campaign to new channels, or launch the same campaign on several platforms.
---

# Cross-platform transfer plan

Turn "run this Meta campaign on TikTok too" into a field-by-field plan a media buyer can build from, or an agent can execute as paused drafts.

## Inputs

- The source campaign tree: campaign (objective, budget, bid strategy), ad sets/ad groups (optimization event, audiences, locations, ages, placements, schedule), ads (creative files, primary text, headline, CTA, URL, URL parameters).
- Source performance for the last 30 days (to set targets and budgets).
- Destination platform(s), destination account currency and time zone, and which conversion events exist on the destination (pixel or SDK installed?).

## Step 1: objective map

These are the API objective values used by one production transfer engine (Whathead, Nov 2026). Use them as a starting point and confirm in the destination UI, since platforms rename objectives.

| Intent | Meta | TikTok | Snapchat | Google Ads | X | LinkedIn | Reddit |
|---|---|---|---|---|---|---|---|
| Sales / conversions | OUTCOME_SALES | WEB_CONVERSIONS | SALES | PERFORMANCE_MAX | WEBSITE_CONVERSIONS | WEBSITE_CLICKS | CONVERSIONS |
| Leads | OUTCOME_LEADS | LEAD_GENERATION | LEADS | DEMAND_GEN | ENGAGEMENTS | ENGAGEMENTS | CLICKS |
| Traffic | OUTCOME_TRAFFIC | TRAFFIC | TRAFFIC | SEARCH | WEBSITE_CLICKS | WEBSITE_CLICKS | CLICKS |
| App promotion | OUTCOME_APP_PROMOTION | APP_PROMOTION | APP_PROMOTION | PERFORMANCE_MAX (or App campaigns) | APP_INSTALLS | APP_INSTALLS | APP_INSTALLS |
| Awareness / reach | OUTCOME_AWARENESS | REACH | AWARENESS_AND_ENGAGEMENT | DISPLAY | REACH | REACH | IMPRESSIONS |
| Video views | VIDEO_VIEWS (legacy) | VIDEO_VIEWS | AWARENESS_AND_ENGAGEMENT | VIDEO | VIDEO_VIEWS | VIDEO_VIEWS | VIDEO_VIEWABLE_IMPRESSIONS |
| Engagement | OUTCOME_ENGAGEMENT | ENGAGEMENT | AWARENESS_AND_ENGAGEMENT | DISPLAY | ENGAGEMENTS | ENGAGEMENTS | CLICKS |

The engine falls back to a safe generic objective where no exact match exists. Where the destination has a closer native option, prefer it: LinkedIn's Website conversions and Lead generation (with Lead Gen Forms) objectives, and lead objectives on X and Reddit where your account has them.

Rule: map the **optimization event**, not just the objective. A Meta Purchase-optimized ad set should optimize for the destination's purchase event (TikTok Complete Payment, Snapchat Purchase), which requires that event to be firing on the destination pixel first.

## Step 2: audience translation

- **Broad and demographic** targeting (location, age, gender, language) transfers directly. Check age minimums and any restricted categories per market.
- **Interests** do not map one to one. Pick the closest destination interests or behaviors and label them as new tests.
- **Custom audiences** (customer lists, site visitors, engagers) must be rebuilt on the destination: upload the list, create the pixel audience and wait for it to populate before launch.
- **Lookalikes** need a seed on the destination platform. Use the same seed list.
- **Exclusions** (purchasers, existing leads, employees) must be recreated. Leaving them out is the most common transfer mistake.

## Step 3: placements and creative

| Check | Guidance |
|---|---|
| Aspect ratio | TikTok and Snapchat are vertical 9:16 (1080x1920). Meta Feed prefers 4:5 or 1:1, Reels and Stories 9:16. X and LinkedIn take 1:1 or 16:9 for most feed placements. |
| Safe zones | Keep text and logos out of the top and bottom of 9:16 frames, where the app UI overlays them. Check each platform's safe-zone template. |
| Native feel | Meta Feed ads often look out of place on TikTok and Snapchat. Plan at least one native vertical cut (people talking to camera, captions, sound-on). |
| Identity | Snapchat ads run from a Public Profile, and TikTok ads need an identity or a Spark Ads authorization. Set these up before building. |
| Copy limits | TikTok ad text is up to 100 characters. Snapchat headlines are short (about 34 characters) with a brand name of about 25. Meta primary text is truncated after about 125 characters. Rewrite rather than truncate. |
| Formats that don't exist | Meta carousel or collection ads become single videos or images on Snapchat or TikTok (pick the first card or split each card into its own ad). |

## Step 4: budget and bidding

- Start the destination on its own learning budget, not the source's. To get about 50 optimization events a week per ad group: `daily_budget ≈ target_CPA x 50 / 7`.
- Respect platform minimums. TikTok requires at least $50 a day per campaign and $20 a day per ad group (USD, or the equivalent in the account currency). Check other platforms' minimums in the account currency.
- Start with lowest cost or maximize conversions, then add a cost cap once 2 weeks of data exist. Bringing the source's cost cap over usually under-delivers.
- Expect a different CPA. Set the first 2 weeks' target at 1.2 to 1.5x the source CPA, and judge the result after the learning phase ends.

## Step 5: tracking

- Rebuild URL parameters with the destination's macros (see the utm-tracking-qa skill). Set `utm_source` to the destination.
- Confirm the destination pixel or SDK receives the optimization event and that server-side events are deduplicated.
- Align the attribution window you will report with before comparing platforms.

## What does not transfer

- Social proof (likes and comments) unless you use the same post ID (Spark Ads on TikTok, existing posts on Meta)
- Learning history and audience signals
- Platform-only features: Meta Advantage+ creative enhancements, Instant Forms (TikTok and Snapchat have their own lead forms with different fields), dynamic product ads without a catalog on the destination
- Approved policy status. Ads are reviewed again on the destination, and policies differ by market and category.

## Output

1. Transfer sheet: one row per destination entity, with source ID, destination field values, items to create first (pixel events, audiences, identity) and assumptions.
2. Pre-launch checklist (link to campaign-launch-checklist).
3. A 14-day evaluation plan: KPI, target, check dates, kill or scale rules.
4. Build as **paused** drafts. Never launch live in the same step.

## Live data (optional)

Works from exports and a written brief. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=cross-platform-transfer-plan) connected, Claude can read the source tree live and run Whathead's transfer, which copies Meta, TikTok and Snapchat campaigns between each other and into Google, maps the fields and creates the copy paused for your review. See the [transfer docs](https://docs.whathead.com/workflows/cross-platform-transfer?utm_source=github&utm_medium=skills&utm_campaign=cross-platform-transfer-plan).
