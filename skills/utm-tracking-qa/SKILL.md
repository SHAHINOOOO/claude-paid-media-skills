---
name: utm-tracking-qa
description: QA tracking on paid media before and after launch, covering UTM parameters, dynamic URL macros, landing page redirects, pixel and Conversions API events, deduplication, conversion values and currency, and how GA4 will classify the traffic into default channel groups. Works for Meta, Google Ads, TikTok, Snapchat, LinkedIn, X, Reddit, Microsoft Ads and DV360. Use when someone asks to check UTMs, tracking, pixels, CAPI, why GA4 shows traffic as Unassigned or Referral, why platform and GA4 numbers differ, or to build a UTM template.
---

# UTM and tracking QA

Make sure every paid click arrives tagged, lands on a working page and is counted once, and that GA4 puts it in the right channel.

## Inputs

- Active ads with final/landing URLs and URL parameter fields (Meta `url_tags`, Google tracking template and final URL suffix, TikTok/Snap/LinkedIn URL fields).
- The UTM standard if one exists.
- Conversion setup: events per platform (pixel, Conversions API / Events API, offline), the dedup key used, and GA4 key events.
- Optional: GA4 report of sessions by session source / medium / campaign for paid traffic, and platform-reported conversions for the same window.

## Part 1: UTM checks

Every paid ad should resolve to all five of: `utm_source`, `utm_medium`, `utm_campaign`, `utm_content` and `utm_term` (or `utm_id`).

| Check | Rule |
|---|---|
| Present | All five keys present after macros resolve |
| Medium is "paid" for GA4 | GA4 puts traffic in Paid Search, Paid Social, Paid Video or Paid Other only if `utm_medium` matches `^(.*cp.*\|ppc\|retargeting\|paid.*)$`. `cpc`, `ppc` and `paid_social` work. `social` alone lands in Organic Social. |
| Source matches GA4's list | Paid Social needs a source GA4 recognizes as social (for example `facebook`, `instagram`, `tiktok`, `snapchat`, `linkedin`, `twitter`/`x`, `reddit`). Invented sources such as `fb_ads` fall into Paid Other. |
| Consistent case | GA4 values are case-sensitive: `Facebook` and `facebook` are two rows. Use lowercase. |
| No spaces or raw Arabic | Encode or replace them. Names with spaces break in some redirects and BI tools. |
| Macros, not hard-coded names | Use dynamic parameters so renames flow through (table below) |
| No duplicate keys | A UTM in both the final URL and the URL-parameters field produces `?utm_source=a&utm_source=b` |
| Google Ads auto-tagging | Keep auto-tagging on (`gclid`). Add UTMs through the final URL suffix if other tools need them. |

**Dynamic parameters**

| Platform | Campaign | Ad set / ad group | Ad |
|---|---|---|---|
| Meta | `{{campaign.name}}` / `{{campaign.id}}` | `{{adset.name}}` / `{{adset.id}}` | `{{ad.name}}` / `{{ad.id}}`; also `{{placement}}`, `{{site_source_name}}` |
| Google Ads (ValueTrack) | `{campaignid}` | `{adgroupid}` | `{creative}`; also `{keyword}`, `{matchtype}`, `{network}`, `{device}` |
| TikTok | `__CAMPAIGN_NAME__` / `__CAMPAIGN_ID__` | `__AID_NAME__` / `__AID__` | `__CID_NAME__` / `__CID__`; also `__PLACEMENT__` |
| Snapchat, LinkedIn, X, Reddit | Check the platform's current list of URL macros in its help center. Where none exist, hard-code the IDs from the name convention. | | |

Suggested template (Meta example):
`utm_source=facebook&utm_medium=paid_social&utm_campaign={{campaign.name}}&utm_content={{ad.name}}&utm_term={{adset.name}}&utm_id={{campaign.id}}`

## Part 2: Landing page checks

For each unique final URL, run `curl -sIL "<url-with-params>"` (or open it in a browser) and check:

- Final status 200. Any 4xx or 5xx is Critical.
- At most 2 redirects, and the query string survives every hop (http to https and apex to www redirects must keep it).
- The page is the one the ad promises: right market, language and product, and in stock.
- Mobile load is reasonable. Flag pages that are visibly slow on a phone connection.
- Consent banners do not block the pixel when consent is given. Google Consent Mode v2 is required for EEA traffic.

## Part 3: Conversion event checks

| Check | Rule |
|---|---|
| Event fires once | Pixel plus server events share one dedup key (`event_id` or the platform's equivalent). Without it, conversions are double-counted. |
| Right trigger | Purchase fires on the confirmation step only, not on page reload or the cart page |
| Value and currency | Purchase events carry `value` and `currency`, and the currency matches the store, not the ad account, unless conversion is intended |
| Match quality | Server events include hashed email and phone, external ID, IP and user agent, and click IDs (`fbc`/`fbp`, `ttclid`, Snap click ID) where consented. On Meta, Event Match Quality rated Poor or OK on key events is a finding. |
| Optimization event health | The event the campaign optimizes for has fired in the last 24 hours |
| Test traffic | Exclude internal and test hosts (staging domains, QA orders) from GA4 and platform events |

## Part 4: Reconcile platform vs GA4 (if data is given)

Expect differences. Explain them rather than "fix" them:

- Attribution windows: Meta defaults to 7-day click and 1-day view, and other platforms use their own defaults. GA4 uses its own attribution model and lookback.
- View-through conversions appear in platforms, not in GA4.
- Cross-device journeys, ad blockers and consent refusals reduce GA4 counts.

Flag as a likely tracking defect: GA4 sessions from paid sources below 70% of platform clicks, or platform conversions above 2x GA4 key events for the same channel on click-only attribution. Investigate both.

## Output

1. Pass/fail summary per platform.
2. Defects table: severity, platform, entity, URL or event, issue, fix.
3. Corrected URL-parameter strings per platform, ready to paste (**draft**).
4. A one-page UTM standard if the team has none.

## Live data (optional)

Works with exports and URL lists. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=utm-tracking-qa) connected, Claude can read live ad URLs and URL parameters across platforms and GA4 source/medium reports, then draft URL fixes as dry runs for approval.
