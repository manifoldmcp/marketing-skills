---
name: write-weekly-report
description: When the user wants one recurring report on their own marketing. Runs every week on the host's schedule and puts the monitoring jobs side by side, the site's own Search Console clicks, new brand mentions, competitor changes, Google rank moves and AI visibility in ChatGPT, Gemini, Claude, Perplexity and AI Overviews, each with the change from last week, and leads with at most three things worth acting on. Also use when the user mentions a weekly marketing report, a Monday digest, what changed this week, one weekly summary of mentions, ranks and competitors, keep an eye on everything, or ongoing monitoring with no single subject. One subject watched alone goes to monitor-brand-mentions, monitor-competitors, track-rankings, monitor-search-console or check-ai-visibility, a client's monthly report to write-client-report.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Weekly marketing report

One report a week that puts the monitoring jobs side by side: the site's own search clicks, new mentions, competitor changes, rank moves and AI visibility, each with the change from last week. It runs the other skills rather than repeating them, and it leads with the few things worth acting on. It ends in one document the host keeps and, if it has the tools, delivers.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_get_new_posts` and `seo_get_position` (hosts often add a prefix, for example `mcp__manifold__seo_get_position`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The report crosses tool groups: `reddit_*` and the platform groups for mentions and posts, `ads_*` for ads, `seo_*` for ranks and search footprint, `aeo_*` for AI answers, `console_*` for the user's own Search Console data. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run what the rest allow, and mark those sections "not watched" in every report rather than reporting zero.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand, the competitors, the keywords and AI prompts in the tracking set, the accounts to watch, the Search Console property) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Sections**: which of the five the report carries: [monitor-search-console](../monitor-search-console/SKILL.md), [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md), [monitor-competitors](../monitor-competitors/SKILL.md), [track-rankings](../track-rankings/SKILL.md) and [check-ai-visibility](../check-ai-visibility/SKILL.md). Default: all five, Search Console only when it is connected. Each section's own inputs are settled in its skill; a section never run before starts with its baseline this week.
- **Day and reader**: default Monday morning, for the user. The host schedules it and, if it has an email or chat tool, delivers it.
- **Which sections run weekly**: default mentions, competitors and ranks every week, AI visibility every other week (the report shows the last measured date in the off weeks). Mentions may run daily on their own schedule; the report then reads the week's stored mentions instead of searching again.
- **Budget**: at each skill's defaults, a run costs about 0 (Search Console) + 26 (mentions, weekly) + 41 (competitors) + 120 (ranks) = 187 credits, or 367 in the weeks AI visibility runs (+180): about 1,200 credits a month (about $12). Daily mentions instead of weekly raise it to about 1,570. Say both numbers before setting up the schedule; pass `max_credits` on every call so no week overspends.

## Steps

1. **Set up (first run only).** Settle each section's inputs from its skill, store them with the host (see [Schedule and state](#schedule-and-state)), and set one schedule for the report day. The first report is the baseline for every section: it says so and lists the starting numbers.
2. **Run the sections.** Follow each section's skill from its run step, cheapest first: Search Console watch, brand mentions, competitor watch, rank tracking, then AI visibility in the weeks it is due. Add `aeo_get_site_readiness` on the user's site (free): a check with `tier: "blocks_citations"` that newly shows `ok: false` (a CDN or firewall rule now turning away AI crawlers) goes to the top of the report. If a tool group is switched off or a call fails, that section says "not measured this week", never zero.
3. **Compute the week-over-week change.** From each section's stored state:
   - Search Console: clicks and impressions against the 21 days before, and the pages that lost the most.
   - Mentions: new mentions by platform and by kind, and the three with the most reach.
   - Competitors: new and stopped ads, new posts worth noting, traffic moves and new top pages.
   - Ranks: keywords up, down and unchanged, the count in the top 3 and top 10, the biggest moves.
   - AI visibility: mention and citation share per brand, against last run and the three-run average.
4. **Pick what to act on.** At most three items, each tied to a row and to the skill that acts on it: a thread worth an answer ([find-reddit-threads](../find-reddit-threads/SKILL.md)), a competitor push ([research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md) or [create-competitor-plan](../create-competitor-plan/SKILL.md)), a page off page one ([refresh-content](../refresh-content/SKILL.md) or [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md)), a prompt where a competitor took the user's place ([build-ai-citations](../build-ai-citations/SKILL.md)). Only changes above the thresholds in Judgment qualify.
5. **Deliver** one document: a headline table (metric, this week, last week, change, baseline), the three actions, each section's change table from its skill (a quiet section is one line), and the week's credits by section. The host stores this week's numbers as next week's comparison. The server sends nothing; if the host has an email or chat tool, offer to send the document there.

## Judgment

- Changes first. A report that restates levels every week stops being read by the third one.
- The headline table carries the few numbers that answer "better or worse than last week": Search Console clicks, new mentions, rank count in the top 10, AI mention share, and the competitor moves count. Everything else sits in the sections.
- Each section keeps its own noise rules; a report that flags everything flags nothing. Flag a change only above the noise: a rank move of 3 or more positions, or one that crosses into or out of the top 3 or the top 10; an AI mention share that moves 10 or more points, or smaller moves that hold for three runs; Search Console clicks for the last 7 days 20 percent or more below the weekly average of the 21 days before (a rule of thumb); any new ad id; a competitor post at twice that account's median engagement. Mentions alert at once only on a complaint or a question with reach, or a mention by a journalist or a creator the user named; the rest go in the digest.
- Rank tracking and AI visibility are the largest lines in the cost. Run AI visibility every other week (its week-to-week moves are mostly noise anyway), and trim the rank set to the keywords that matter before cutting a section.
- SEO traffic estimates move slowly; report them month over month inside competitor watch, not as a weekly headline.
- Acting on a row (replying, answering a competitor, changing a page) is the user's call.

## Schedule and state

- The host runs the schedule. If it has a scheduler (a scheduled task, a routine, a cron job), set the report up there for the report day. If it has none, say so: the user asks again each week, and the host reruns the report with the stored state.
- Each section names the state it needs, and the host stores it where the next run can read it (a file, a doc, a sheet, its memory), with the settings: the terms, communities, keywords, prompts, engines, competitors, `location`, `language` and `device`. Every run uses the same ones. A changed setting starts a new baseline for what it changed; report those rows apart until they have history. Without the stored state every run is a first run.
- Store each run's date and the credits it cost (`meta.credits_charged` on every response), so the monthly cost is measured, not guessed. A month is about 4.3 weekly runs.
- `reddit_get_new_posts` and `aeo_run_ai_answers` are never cached and are charged in full on every run. The rest are free when this account already paid for the same result inside the cache window (1 hour for Reddit searches and threads, 6 hours for other platform searches and listings, 24 hours for profiles, positions and ad lists, 7 days for single ads and domain overviews). Console tools are free at any cadence. `dry_run: true` prices any call for free.

## Related skills

- Each section on its own schedule: [monitor-search-console](../monitor-search-console/SKILL.md), [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md), [monitor-competitors](../monitor-competitors/SKILL.md), [track-rankings](../track-rankings/SKILL.md), [check-ai-visibility](../check-ai-visibility/SKILL.md).
- A report for an agency's client, monthly and branded for them: [write-client-report](../write-client-report/SKILL.md), which can reuse these sections.
- The reaction to a launch, measured in the weeks after it: [create-launch-plan](../create-launch-plan/SKILL.md).
- A plan whose KPIs this report re-measures: [create-growth-plan](../create-growth-plan/SKILL.md).
