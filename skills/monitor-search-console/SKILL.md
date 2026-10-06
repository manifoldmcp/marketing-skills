---
name: monitor-search-console
description: When the user wants to be told when their own Google traffic drops. Reads the site's connected Search Console (and Bing Webmaster) every day or week on the host's schedule, compares the last 7 days of clicks and impressions with the weeks before and the same weeks a year earlier, finds the pages and queries behind a drop and whether rank, demand or an AI Overview caused it, and checks indexing and sitemaps. Costs no credits. Also use when the user mentions alert me if our Google traffic drops, watch our Search Console, daily clicks and impressions check, tell me when a page loses traffic, or traffic drop alerts. Diagnosing a drop already found goes to diagnose-traffic-drop, keyword positions to track-rankings, one weekly digest of everything to write-weekly-report.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Search Console watch

The user's own Google clicks and impressions, checked every day or week against the 28 days before, with the pages and queries behind any drop. It reads the user's connected Search Console (and Bing Webmaster, if connected), so it costs no credits. The one-off diagnosis of a drop is [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md); this skill catches the drop the week it starts and hands it there.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `console_list_properties` and `console_get_search_analytics` (hosts often add a prefix, for example `mcp__manifold__console_get_search_analytics`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `console_*` tools are not, the tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: this skill has no estimate to fall back on.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the Search Console property, the money pages and the brand queries) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Property**: `console_list_properties` (free) lists what the connected accounts can read; prefer an `sc-domain:` property, which covers every host and protocol. On `NotConnected`, give the user its `connect_url` and stop: this skill has no estimate to fall back on. How to read Search Console is in the [Google search notes](../create-seo-plan/references/platforms/google.md#search-console).
- **Watched pages and queries**: up to 20 money pages and the brand queries, reported on their own lines. Default: the 10 pages and 10 queries with the most clicks in the baseline.
- **Cadence**: daily or weekly. Google's data lags about two days, so each run reads up to the last complete day (`end_date` defaults to 3 days ago). Daily suits a site where a lost day costs money; weekly suits the rest.
- **Budget**: 0 credits. Console tools are limited to 60 calls a minute per workspace; a run makes 2 to 6 calls, plus one per page inspected in step 4. `seo_get_serp` in step 3 is the one paid call (2 credits a query); pass `max_credits` on it.

## Steps

1. **Set up (first run only).** Settle the inputs, store them with the host (see [Schedule and state](#schedule-and-state)), and set the schedule. The baseline: `console_get_search_analytics` with `dimensions: ["date"]` and the default window (28 days), and the same 28 days a year earlier (Google keeps 16 months), so a seasonal dip is not read as a loss. Store each day's `clicks` and `impressions`, and the baseline's top pages and queries with `dimensions: ["page"]` and `dimensions: ["query"]` (`limit: 1000`).
2. **Run the site check.** `console_get_search_analytics` with `dimensions: ["date"]` and the default window. Compare the last 7 days with the weekly average of the 21 days before, and each day with the same weekday a week earlier (B2B sites dip every weekend). Flag only a drop that clears the threshold in Judgment. A quiet run is one line.
3. **Find where it moved.** Only when step 2 flags: `console_get_search_analytics` with `dimensions: ["page"]`, then `dimensions: ["query"]`, for the last 7 days and for the 7 days before (four calls, `limit: 1000`). Join the two periods and rank by clicks lost. Read each loser by what moved with the clicks:
   - `position` fell: a ranking loss. Hand the pages to [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md).
   - `impressions` fell with `position` steady: demand fell (season, news, a trend ending). Check the year-earlier baseline before calling it a loss.
   - `impressions` and `position` steady, `ctr` fell: the results page changed. `seo_get_serp` on the top query with `ai_overview: true` (2 credits) shows whether an AI Overview or another feature now sits above the user, and whether it cites them; that goes to [check-ai-overviews](../check-ai-overviews/SKILL.md).
4. **Check the pages that vanished.** For a watched page with clicks near zero this week, `console_inspect_url` (free, Google only): a `verdict` other than PASS, a changed `google_canonical`, or a `robots_txt_state` block is the cause, and a fix for the developer, not for content. Once a week, `console_get_sitemaps` (free): new `errors` or a `last_read_at` that stopped moving.
5. **Store and deliver.** The host appends the run's daily totals, the watched rows and the date. Deliver a table: metric (clicks, impressions, CTR, average position) for the last 7 days, the 21-day weekly average, the change, and the same week a year ago; then the losing pages and queries (page or query, clicks before and now, position before and now, the cause from step 3, the skill that acts on it); then the watched rows. A quiet run is one line.

## Judgment

- Flag clicks for the last 7 days 20 percent or more below the weekly average of the 21 days before (a rule of thumb). Lead with what changed; levels come second, as context.
- Search Console is the user's real data, so it outranks every estimate in the other skills. When it and `seo_get_domain_overview` disagree, report Search Console.
- Average position blends every query, location and device in the window. A move in it with flat clicks is usually a mix change (new long-tail queries ranking low), not a loss.
- Clicks falling while impressions and position hold is the 2026 pattern of AI Overviews and other features taking the click. Step 3's SERP check is what tells it apart from a ranking loss.
- Watch Bing only when it brings the user real clicks, with `engine: "bing"`; its query and page rows cover the last weeks with no date range, so compare runs, not windows.
- A Google core update in the window moves many sites at once. No tool reads Google's Search Status Dashboard: the user or the host's browser checks it, and the report names the update when the dates match.
- The baseline lives with the host. Search Console keeps 16 months, so a lost store can be rebuilt from the console itself.
- Changing a page is the user's call.

## Schedule and state

- The host runs the schedule. If it has a scheduler (a scheduled task, a routine, a cron job), set the skill up there with the cadence the user chose. If it has none, say so: the user asks again each day or week, and the host reruns the skill with the stored state.
- The first run is the baseline: nothing in it is a drop. Say so in the first report.
- The host stores the settings (property, watched pages and queries, engine) with the state from step 5 where the next run can read it: a file, a doc, a sheet, its memory. A watched row added later starts its own baseline.
- Console tools are free at any cadence, but a rerun before Google's data moves returns the same numbers and adds nothing new.
- The deliverable is the change table. The server never sends alerts; if the host has an email, chat or notification tool, offer to send the table there.

## Related skills

- The full diagnosis of a drop this watch found: [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md). Crawl and indexing problems: [audit-technical-seo](../audit-technical-seo/SKILL.md).
- Positions for a fixed keyword set, live: [track-rankings](../track-rankings/SKILL.md).
- AI Overviews taking the click: [check-ai-overviews](../check-ai-overviews/SKILL.md).
- Search Console, mentions, competitors, ranks and AI visibility in one weekly report: [write-weekly-report](../write-weekly-report/SKILL.md).
