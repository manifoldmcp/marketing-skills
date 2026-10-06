---
name: track-rankings
description: When the user wants to know where a site ranks on Google, now or every week. Checks positions for a set of keywords with the cheapest method (a live position check, a results page for several sites at once, the ranked-keywords index, or the user's own Search Console), reads which page ranks and who sits above it, and can run the same set on the host's schedule with stored history and a change report. Also use when the user mentions where do we rank for X, are we on page one, check our Google position, who ranks above us, track our rankings, rank tracker, position tracker, weekly keyword positions, or alert me if we drop off page one. A fall in traffic goes to diagnose-traffic-drop, keyword research to create-seo-plan, AI answers to check-ai-visibility, one weekly digest of everything to write-weekly-report.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Rank tracking

Where a site ranks on Google for a set of keywords. The one-off check picks the cheapest method that answers the question, reads the result right (which page ranks, who sits above it), and hands back a dated table. Tracked on the host's schedule, the same check is stored and compared with the runs before, and only the moves are reported.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_position` and `seo_get_serp` (hosts often add a prefix, for example `mcp__manifold__seo_get_position`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Then follow the tool checks in the [Google search notes](../create-seo-plan/references/platforms/google.md#tools): the `seo_*` tools are required, the `console_*` tools optional.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the competitors, the market, the keywords in the tracking set, the Search Console property) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Target**: a domain (`example.com` covers every subdomain), one host (`www.example.com`), or one page (a full URL).
- **Keywords**: the list to check. If the user has none, take the top keywords from `seo_get_ranked_keywords` on the site (10 credits) and confirm them.
- **Competitors**: other sites to compare on the same keywords. Optional.
- **Market**: `location`, `language` and `device` (desktop or mobile). Rankings differ across all three; default United States, English, desktop.
- **Search Console**: for the user's own site, whether the `console_*` tools are there. They give the average position the site actually had, free. The [Google search notes](../create-seo-plan/references/platforms/google.md#search-console) says how to check.
- **Once or tracked**: a check now, or the same set on a schedule (step 5).
- **Budget**: once, 6 credits per keyword and target with `seo_get_position`: 10 keywords for one site is 60 credits. With competitors, `seo_get_serp` is cheaper: 10 keywords at `depth: 30` is 30 credits for every site at once. Tracked, the cost per run and per month is in [tracking](references/tracking.md). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the method.**
   - One target, up to about 15 keywords: `seo_get_position` per keyword (6 credits each). It scans the top 100 and returns the rank, the ranking URL and who is above.
   - Several targets on the same keywords: `seo_get_serp` per keyword with `depth` deep enough to reach them, 20 or 30 (2 or 3 credits). Every site's position is on one page. Use `seo_get_position` only for a target that is not within the depth.
   - Every keyword a site ranks for, not a chosen list: `seo_get_ranked_keywords` (10 credits per 100 rows). It is an index snapshot, refreshed on the provider's schedule and cached for 7 days, not a live check.
   - The user's own site with Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["query"]` and `limit: 1000`, then pick the keywords from the rows; add `device` to `dimensions` to split mobile and desktop (free). To read only the chosen keywords, pass one `filters` entry on query with `operator: "includingRegex"` and `expression: "^(keyword one|keyword two)$"`; every entry in `filters` must match, so one entry per keyword returns nothing. This is an average over the window, not a snapshot; report it next to the live check, not instead of it.
2. **Run it** with the market from the inputs on every call, so all rows are comparable.
3. **Read the result.** `rank` counts every item on the page, including features; `organic_rank` counts organic results only. Report both where they differ, since a rank of 7 can be the third organic result under an AI overview and a video pack. `rank` null means not in the top 100. Check `url`: the wrong page ranking (a blog post for a product query, an old page after a migration) is a finding in itself. From `above[]`, or the SERP rows, name who sits directly ahead. From `seo_get_serp`, note `features`; add `ai_overview: true` (+1 credit) when the user asks whether an AI overview shows.
4. **Deliver** a table: keyword, target, rank, organic rank, ranking URL, the result directly above, SERP features, and the date, location and device checked. Save nothing on the server: if the user wants to compare next month, the host keeps the table.
5. **Track it on a schedule.** When the user wants the same keywords checked every week, open [tracking](references/tracking.md): it fixes the set and the method, stores each run with the host, and reports the change.

## Judgment

- A rank is a snapshot from one location on one device. The user's own search is personalised and will differ; that is not an error in the check.
- `seo_get_position` and `seo_get_serp` results are cached for 24 hours. Checking again the same day returns the same answer, free for this account.
- Search Console's average position blends every query variant, location and device over the window. A 9.6 there and a 6 in a live check can both be right.
- For more than about 20 keywords on one site, `seo_get_ranked_keywords` answers most of them for a fraction of the cost; run `seo_get_position` only on the ones that matter most.
- A domain target counts any subdomain. If the user asks about their blog on `blog.example.com` alone, pass that host.
- `dry_run: true` prices any call for free.

## Related skills

- Traffic or rankings that fell, and why: [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md). Crawl and indexing problems: [audit-technical-seo](../audit-technical-seo/SKILL.md).
- Pages ranking 4 to 20 that a page-level fix can move: [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md). One page slipping over several runs: [refresh-content](../refresh-content/SKILL.md).
- Two of the user's pages swapping on one keyword: [fix-keyword-cannibalization](../fix-keyword-cannibalization/SKILL.md).
- The site's own Search Console clicks watched for drops: [monitor-search-console](../monitor-search-console/SKILL.md).
- Ranks, mentions, competitors and AI visibility in one weekly report: [write-weekly-report](../write-weekly-report/SKILL.md).
