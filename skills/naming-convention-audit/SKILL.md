---
name: naming-convention-audit
description: Audit campaign, ad set/ad group and ad names against a naming convention across Meta, Google Ads, TikTok, Snapchat, LinkedIn, X, Reddit and DV360. Parses names into tokens, validates each token against allowed values, reports compliance percentage, and proposes corrected names. Can also draft a convention from existing names. Use when someone asks to check naming, enforce a taxonomy, clean up names before reporting, or create a naming convention for an agency or client.
---

# Naming convention audit

Make names machine-readable so reporting, pivots and automated rules work. Find every name that breaks the convention, explain why, and propose the corrected name.

## Inputs

1. **The convention.** Delimiter, ordered tokens per level, allowed values per token, and case rules. If the user has none, go to "Draft a convention" below.
2. **Names to audit**: entity ID, level (campaign, ad set, ad), platform, status, spend (to prioritize), and the actual settings when available (objective, country, dates). Settings let you check that a name tells the truth.

## Default convention (if the user wants one)

```
Campaign: {market}_{platform}_{objective}_{funnel}_{product}_{yyyymm}
Ad set:   {audience-type}_{audience}_{placement}_{optimization}
Ad:       {format}_{concept}_{variant}_{language}
```

| Token | Allowed values (examples) |
|---|---|
| market | ISO 3166 alpha-2: KSA as `sa`, `ae`, `eg`, `kw`, `qa`, `bh`, `om`, or `gcc` for multi-country |
| platform | `meta`, `ggl`, `tt`, `snap`, `li`, `x`, `rdt`, `dv360`, `msft` |
| objective | `awr`, `trf`, `eng`, `lead`, `app`, `sales` |
| funnel | `pros`, `rtg`, `ret` (retention) |
| audience-type | `broad`, `int`, `lal`, `cust`, `rtg` |
| format | `vid`, `img`, `car`, `coll`, `story` |
| language | `en`, `ar`, `bil` |

Rules: lowercase, `_` between tokens, `-` inside a token, no spaces, no emojis, ASCII only (Arabic text in names breaks many BI tools), dates `yyyymm` or `yyyymmdd`.

## Steps

1. **Normalize** each name: trim, collapse double delimiters, and record the original.
2. **Tokenize** by the delimiter. Wrong token count is the first failure type.
3. **Validate** each token against its allowed list. Allow case-insensitive matches but flag case errors separately.
4. **Truth check** where settings are available: the market token vs targeted countries, the objective token vs the actual objective, the date token vs the start date, the platform token vs the actual platform. A name that parses but lies is worse than one that fails to parse.
5. **Duplicates**: the same full name on two live entities in the same account.
6. **Score**: compliance % = compliant names / total names. Report it by level, by platform and spend-weighted (share of spend under compliant names).
7. **Propose fixes**: rebuild each failing name from the entity's settings, and mark every token you inferred rather than read.

## Failure types

| Code | Meaning |
|---|---|
| N1 | Wrong number of tokens |
| N2 | Token value not in the allowed list |
| N3 | Case or character violation (spaces, uppercase, non-ASCII) |
| N4 | Name contradicts settings (truth check) |
| N5 | Duplicate live name |
| N6 | Missing required token (for example no market) |

## Draft a convention from existing names

If there is no convention, sample the 200 highest-spend names. Detect the most common delimiter and token positions, cluster recurring values, and propose a convention that keeps what the team already does. Show the coverage: what share of current names would already comply.

## Renaming safely

- Renaming does not change delivery on major platforms, and on Meta a name edit is not a significant edit for learning.
- Reports that pull by name will split history at the rename date. Note the change date so analysts can map old to new names, and keep an old-to-new mapping table.
- Rename in bulk through an editor or bulk upload, never one by one, and do not rename entities in a live flight on reporting day.

## Output

1. Compliance summary: overall %, spend-weighted %, by platform and level.
2. Failures table: platform, level, ID, current name, failure codes, proposed name, inferred tokens.
3. Bulk-rename sheet (ID, old name, new name) as a **draft**.
4. Convention document (if drafted) with token tables and examples.

## Live data (optional)

Works on any list of names. With the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=naming-convention-audit) connected, Claude can read live names and settings across platforms for the truth check, then prepare the renames as dry runs you approve.
