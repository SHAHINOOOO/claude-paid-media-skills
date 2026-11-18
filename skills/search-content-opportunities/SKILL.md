---
name: search-content-opportunities
description: Find SEO content opportunities by combining Google Search Console query and page data with GA4 landing page engagement and key events, covering striking-distance queries (positions 4 to 20), titles with low CTR for their position, decaying pages, keyword cannibalization, high-traffic pages that don't convert, and queries you pay for in Google Ads but already rank for. Returns a prioritized action list (rewrite title, refresh, expand, consolidate, new page). Use when someone asks for SEO quick wins, content ideas from Search Console, why organic traffic dropped, which pages to update, or how to align SEO with paid search.
---

# Search Console and GA4 content opportunity finder

Turn Search Console and GA4 exports into a short, prioritized list of page-level actions with the expected gain for each.

## Inputs

- **Search Console** (Performance report): query, page, clicks, impressions, CTR and position, for the last 28 days and the previous 28 days, or 3 months vs the prior 3 months for small sites. Export with both the query and page dimensions.
- **GA4**: landing page, sessions, engaged sessions, engagement rate, key events and revenue, filtered to session default channel group = Organic Search, for the same period. Filter `hostName` to the production domain so staging or test hosts don't pollute the data.
- Optional: Google Ads search terms with cost and conversions (for the paid and organic overlap check).
- Site context: priority products or pages, markets, languages.

## Prepare

1. Normalize URLs: lowercase the host, strip tracking parameters and fragments, and treat trailing-slash variants as one page. GA4 landing pages often include query strings, so strip them.
2. Exclude branded queries into their own group, using a brand regex. Branded and non-branded queries behave differently.
3. Build the site's own CTR curve: median CTR by position bucket (1, 2, 3, 4 to 5, 6 to 10, 11 to 20) from non-branded queries with at least 100 impressions. Use this curve, not an industry average, as the expected CTR.

## Opportunity checks

| # | Opportunity | Rule (defaults) | Action |
|---|---|---|---|
| 1 | Striking distance | Non-branded query, average position 4 to 20, at least 100 impressions in 28 days, page is relevant | Improve the ranking page: answer the query in a heading and the first 100 words, add internal links from strong pages, refresh facts and dates |
| 2 | Low CTR for position | Query or page CTR below 50% of the site curve for its position bucket, with at least 500 impressions | Rewrite the title and meta description, add the query wording, check rich result eligibility |
| 3 | Decay | Page clicks down at least 30% vs the previous period, with at least 50 clicks before | Check whether rankings dropped (refresh content) or impressions dropped (demand or seasonality, so do nothing) |
| 4 | Cannibalization | Two or more of your pages get impressions for the same query, neither ranks in the top 3, and positions swap over time | Pick one primary page. Consolidate, redirect or differentiate the others. |
| 5 | Traffic that doesn't convert | GA4 organic landing page in the top 20% of sessions, with an engagement rate or key event rate below 50% of the site median | Fix intent mismatch: add a CTA, offer or next step, check page speed and above-the-fold content |
| 6 | Content gaps | Queries with impressions where the ranking page is only loosely related (for example the homepage ranks for a how-to query), or questions people ask with no dedicated page | New page or new section |
| 7 | Paid and organic overlap | Google Ads search terms with spend where the site already ranks in organic position 1 to 3 for a non-branded term | Candidates for a holdout test that lowers paid bids. Test, don't just cut, and watch total clicks and conversions. |

## Prioritize

Score each item and sort descending:

- Snippet fixes (check 2): `impressions x (expected CTR - actual CTR)` = extra clicks a month if CTR reaches the curve.
- Ranking fixes (check 1): `impressions x (CTR at target position - current CTR)`, with the target position = current bucket minus one.
- Decay (check 3): clicks lost.
- Conversion fixes (check 5): `sessions x (median key event rate - page key event rate)` = extra key events.
- Multiply by 1.5 for priority pages (money pages, priority markets).

Keep the top 15 to 25 actions. A long list doesn't get done.

## Caveats

- Search Console hides some rare queries for privacy, so query-level totals are lower than page-level totals.
- Average position is impression-weighted and moves with how many queries a page ranks for. Read it with impressions.
- Correlation between a title change and a click change needs at least 2 to 4 weeks of data to read.

## Output

1. Summary: total estimated extra clicks a month, extra key events, and the top 5 actions.
2. Action table: priority, page, query or queries, check, current metrics, expected gain, action, effort (S/M/L).
3. Title and meta rewrites for the top snippet fixes (**draft**, within about 60 and 155 characters).
4. Briefs for new pages or sections: target query cluster, intent, outline, internal links.

## Live data (optional)

Works with Search Console and GA4 exports. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=search-content-opportunities) connected, Claude can query Search Console search analytics and GA4 reports live, add Google Ads data when the account is connected, and run the joins directly.
