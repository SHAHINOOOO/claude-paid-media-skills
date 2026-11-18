# Paid media skills for Claude

Twelve free, practical [Agent Skills](https://agentskills.io) for performance marketers and agencies: account audits, wasted spend, pacing, naming, tracking QA, creative fatigue, cross-platform transfers, weekly reports, launch QA, Snapchat and TikTok setup, anomaly triage, and Search Console content opportunities.

Each skill is a `SKILL.md` file with clear steps, checklists and default thresholds that a media buyer would actually use. They work with any data: CSV exports from ads managers, Google Sheets, a BI extract, or a live MCP connection. Every skill recommends changes; none of them applies a change without your approval.

MIT licensed. Built and maintained by [Whathead](https://www.whathead.com/?utm_source=github&utm_medium=skills&utm_campaign=readme).

## The skills

| Skill | What it does | Platforms |
|---|---|---|
| [`ad-account-audit`](./skills/ad-account-audit/SKILL.md) | Scored health audit: measurement, structure, objective fit, budgets, targeting, creative, waste and hygiene, with fixes ranked by impact | All |
| [`wasted-spend-finder`](./skills/wasted-spend-finder/SKILL.md) | 11 checks (zero-conversion spenders, search terms, placements, frequency, geo, schedule, broken URLs) with the money involved | All, plus search terms for Google and Microsoft |
| [`budget-pacing-check`](./skills/budget-pacing-check/SKILL.md) | Expected vs actual spend, projection and required daily budget, with weekday weighting, GCC weekends and Ramadan phasing | All |
| [`naming-convention-audit`](./skills/naming-convention-audit/SKILL.md) | Parses names into tokens, checks them against allowed values and the actual settings, proposes renames, or drafts a convention | All |
| [`utm-tracking-qa`](./skills/utm-tracking-qa/SKILL.md) | UTMs and macros, redirects, pixel and CAPI deduplication, value and currency, and the GA4 channel-grouping rules that decide Paid Social vs Unassigned | All, plus GA4 |
| [`creative-fatigue-detector`](./skills/creative-fatigue-detector/SKILL.md) | Separates fatigue from saturation, auction pressure and tracking problems, using each ad's own baseline | Meta, TikTok, Snapchat, X, LinkedIn, YouTube |
| [`cross-platform-transfer-plan`](./skills/cross-platform-transfer-plan/SKILL.md) | Objective map (Meta, TikTok, Snapchat, Google, X, LinkedIn, Reddit), audiences, creative specs, copy limits, budgets, tracking, and what does not carry over | Cross-platform |
| [`weekly-performance-report`](./skills/weekly-performance-report/SKILL.md) | A two-minute weekly report: headline, KPI tables, movers with causes, pacing and next-week actions with owners | All |
| [`campaign-launch-checklist`](./skills/campaign-launch-checklist/SKILL.md) | Build vs brief QA with go/no-go rules and first-72-hour watch triggers | All |
| [`snapchat-tiktok-setup-review`](./skills/snapchat-tiktok-setup-review/SKILL.md) | The Snapchat and TikTok settings that cost the most when copied from Meta: events, budgets, automation toggles, identity, creative | Snapchat, TikTok |
| [`anomaly-triage`](./skills/anomaly-triage/SKILL.md) | Is it real, what caused it (fixed decision order), severity, and a contained first response with undo steps | All |
| [`search-content-opportunities`](./skills/search-content-opportunities/SKILL.md) | Search Console plus GA4: striking-distance queries, low-CTR titles, decay, cannibalization, non-converting pages, paid/organic overlap | Search Console, GA4, Google Ads |

## Install

### Claude Code (as a plugin)

```
/plugin marketplace add SHAHINOOOO/claude-paid-media-skills
/plugin install paid-media-skills@whathead-paid-media
```

### Claude Code (copy the folders)

```bash
git clone https://github.com/SHAHINOOOO/claude-paid-media-skills.git
mkdir -p ~/.claude/skills
cp -R claude-paid-media-skills/skills/* ~/.claude/skills/
```

Use `.claude/skills/` inside a project instead of `~/.claude/skills/` to share the skills with your team through the repo.

### Claude.ai and Claude Desktop

Zip a skill folder (for example `skills/ad-account-audit`) and upload it under **Settings → Capabilities → Skills**. Skills require a paid plan with code execution enabled.

### Other agents

The skills follow the open Agent Skills format (`SKILL.md` with `name` and `description` frontmatter), so agents that read that format can load them too. Otherwise, paste the file into your prompt.

## How to use

Ask in plain language. The skill triggers from its description, for example:

- "Audit this Meta account. Export attached." (ad-account-audit)
- "Where are we wasting money on Google Ads this month?" (wasted-spend-finder)
- "Will the KSA Snapchat budget land by month end?" (budget-pacing-check)
- "Plan moving this Meta campaign to TikTok and Snapchat." (cross-platform-transfer-plan)
- "Write the weekly report for the client from these exports." (weekly-performance-report)

Give the skill data in any form: CSV or XLSX exports, a Google Sheet, pasted tables or screenshots. Each skill lists the inputs it needs and says what it could not check when data is missing.

## Thresholds are defaults

Every number in these skills (2x target CPA, frequency 3, ±10% pacing bands, and so on) is a starting default chosen by practitioners. It is not a platform rule. Skills say which thresholds they used, so you can tune them per account. Platform facts (budget minimums, delivery rules, GA4 channel definitions) were checked in November 2026. Platforms change, so open an issue if one is out of date.

## Live data with Whathead MCP (optional)

These skills don't need Whathead. If you want Claude to pull the data itself instead of reading exports, connect the [Whathead MCP server](https://docs.whathead.com/ai-assistant/connect?utm_source=github&utm_medium=skills&utm_campaign=readme) (`https://www.whathead.com/mcp`). It gives Claude live read access, and approval-gated write access, to Meta, Google Ads, TikTok, Snapchat, LinkedIn, Reddit, X, Microsoft Ads, DV360, Criteo, Apple Ads and Amazon Ads, plus GA4 and Search Console. Writes default to a dry run and need your confirmation.

```bash
claude mcp add --transport http whathead https://www.whathead.com/mcp
```

Docs: [what the assistant can do](https://docs.whathead.com/ai-assistant/what-it-can-do?utm_source=github&utm_medium=skills&utm_campaign=readme) · Pricing: [whathead.com/pricing](https://www.whathead.com/pricing?utm_source=github&utm_medium=skills&utm_campaign=readme) (14-day free trial)

## Contributing

Issues and pull requests are welcome, especially platform facts that changed, better default thresholds with reasoning, and new skills that follow the same structure (Inputs, Steps, Rules or thresholds, Output). Keep skills tool-agnostic: they must work from an export.

## License

MIT. See [LICENSE](./LICENSE). Platform names are trademarks of their owners; this project is not affiliated with or endorsed by them.
