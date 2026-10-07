---
name: diagnose-traffic-drop
description: When the user wants to know why organic traffic or Google rankings fell. Dates the drop and finds the pages and keywords that lost it, from the user's Google Search Console or DataForSEO estimates, then tests each cause in turn (less demand, competitors, AI overviews and SERP features, technical breakage, lost links, a Google update) and hands back a verdict with the evidence and the fix. Also use when the user mentions our traffic dropped, we lost rankings, was it a Google update, a core update, organic clicks are down, clicks down but impressions flat, or why did this page fall. A crawl with no drop named goes to audit-technical-seo, a drop right after a domain change or redesign to create-migration-plan, rankings watched every week to track-rankings, Search Console alerts to monitor-search-console, recovering lost backlinks to reclaim-lost-links.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Traffic drop

A drop has a date, a place and a cause. This skill finds when organic traffic fell and which pages and keywords lost it, then tests the causes one by one: less demand, competitors who took the positions, AI overviews and other features taking the clicks, technical breakage, and lost links. It ends in a verdict with the evidence and the fix for each cause found.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_domain_overview` and `seo_get_serp` (hosts often add a prefix, for example `mcp__manifold__seo_get_serp`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Then follow the tool checks in the [Google search notes](../create-seo-plan/references/platforms/google.md#tools): the `seo_*` tools are required, the `console_*` tools optional.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the brand name, the Search Console property, the market, the keywords that matter) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Site**: the user's domain, and the pages or section they think fell.
- **When**: the date the user noticed, and where they saw it (analytics, Search Console, sales).
- **Google Analytics**: whether the `analytics_*` tools are there. They show whether the drop is organic only or every channel, and whether conversions fell with it. The [Google search notes](../create-seo-plan/references/platforms/google.md#google-analytics) say how to use them.
- **Changes**: anything that shipped around then: a redesign, a new CMS or template, a domain or URL change, deleted or merged pages, robots.txt or noindex edits. Ask; it is the most common cause and no tool can see the change log.
- **Search Console**: whether the `console_*` tools are there. This job is much better with them: real daily clicks, 16 months of history and index status. The [Google search notes](../create-seo-plan/references/platforms/google.md#search-console) say how to check.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: without Search Console about 61 + 10 + 30 + 10 x 2 + 5 x 6 + 15 + 10 = 176 credits; with it about 20 + 30 + 15 + 10 = 75. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Confirm and date it.**
   - With Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["date"]` over the last 16 months and `limit: 500`, one row per day (free). Find the day the line breaks, and compare the same weeks a year earlier to rule out seasonality. Read clicks and impressions together: impressions down means rankings or indexing; impressions flat with clicks down means the page one changed (steps 4 and 5) or the titles did.
   - With Google Analytics as well: `analytics_get_report` with `metrics: ["sessions", "keyEvents"]` and `dimensions: ["date", "sessionDefaultChannelGroup"]` over the eight weeks around the break and `limit: 1000` (free). If every channel fell on the same day, the cause is the site or its tracking (a broken tag, a consent banner), not Google: check that first. If only `Organic Search` fell, carry on.
   - Without it: `seo_get_domain_overview` with `history: true` (61 credits): 12 months of estimated `organic_traffic` and `organic_keywords`. The data is monthly and modelled, so it dates a drop to the month at best, and a fall in `organic_keywords` alongside traffic means rankings were lost, not just clicks.
2. **Find where.**
   - With Search Console: `console_get_search_analytics` with `dimensions: ["page"]` for four weeks before and four weeks after the break (two calls, free). Rank pages by clicks lost. For the top five pages, call it again with `dimensions: ["query"]` and a `filters` entry on that page, before and after, to name the keywords. Then the same two windows with `dimensions: ["device"]` and with `["country"]`: a fall on one device or in one country narrows the cause at once. Split branded from non-branded queries with one `filters` entry on query, `operator: "excludingRegex"` and the brand name: a fall only in non-branded queries is rankings, a fall in branded queries is demand. For a publisher, also run `search_type: "discover"` and `"news"`; a Discover fall does not show in `web`.
   - With Google Analytics: `analytics_get_report` with `dimensions: ["landingPage"]`, a `filters` entry on `sessionDefaultChannelGroup` equal to `Organic Search`, and the four weeks after the break against the four before as the comparison range (free). It names the landing pages that lost organic visits and, with `keyEvents`, the ones whose loss costs conversions; fix those first.
   - Without it: the estimates show where the site ranks now, not where it ranked before. Ask the user for the landing pages that fell in their analytics, or read them from Google Analytics as above. Call `seo_get_ranked_keywords` on the domain with `limit: 500` (30 credits) for the current ranks, and compare with the pages and keywords the user names. A page the user says used to bring traffic that now has few rows here, or rows at 11 and below, is where the loss sits.
3. **Rule out demand.** Take the 20 keywords that brought the most traffic (from step 2) and call `seo_get_keyword_metrics` (10 credits). If `trend[12]` falls in the same months, searchers left, not Google. Skip this with Search Console, where the year-over-year comparison in step 1 already answers it.
4. **Who took the positions.** For the 10 keywords that lost the most, `seo_get_serp` with `ai_overview: true` (2 credits each). Read what now ranks: a competitor that took several of them, a different format (lists replacing product pages, forum rows with `type: "discussions_and_forums_element"`, videos), or another URL of the user's own site (two pages competing, or a page that moved). If the site is not on the page, `seo_get_position` on the five most valuable keywords (6 credits each) finds its rank now, and `above[]` names who is ahead.
5. **AI overviews and features.** In the same SERPs, a non-null `ai_overview` on a query where impressions held but clicks fell means the answer now sits on Google's page. Check whether the site is among `ai_overview.references[]`. A new `featured_snippet` held by a rival does the same. The fix is on the page (a direct answer early, the format that wins the snippet); which keywords carry an overview and whether the site is cited in it is [check-ai-overviews](../check-ai-overviews/SKILL.md).
6. **Technical breakage.** `seo_get_page` on each page that lost (free): `status`, `final_url` (a redirect the user did not intend), `robots_meta` (noindex), `canonical` (pointing elsewhere), and a `title` or `h1` that no longer matches the keywords it lost. If several sections fell at once, `seo_run_technical_crawl` with `max_pages: 500` (15 credits), then `get_task` (free) after `poll_after_s`: `status_codes`, `non_indexable`, `broken_links` and `issues[]`. With Search Console, `console_inspect_url` on the five biggest losers gives `coverage_state`, `google_canonical` against `user_canonical` and `last_crawl_at`, and `console_get_sitemaps` shows sitemap errors (both free).
7. **Lost links.** `seo_get_backlink_summary` on the site (10 credits): `referring_domains` and `broken_backlinks` now. The summary has no history: `broken_backlinks` above zero means links point at pages that now fail, and links that sites removed show only in the `lost` flag of each link. When links may be the cause, [reclaim-lost-links](../reclaim-lost-links/SKILL.md) finds and recovers them.
8. **Deliver** a verdict in two lines, then a table: cause, evidence (the numbers and their source), pages affected, keywords affected, clicks or estimated traffic lost, the fix, owner. List the causes you tested and ruled out, with the evidence, so the user does not chase them again.

## Judgment

- Follow the [Google search notes](../create-seo-plan/references/platforms/google.md): [Search Console](../create-seo-plan/references/platforms/google.md#search-console) against estimates, the [credits](../create-seo-plan/references/platforms/google.md#credits) and the [handoff](../create-seo-plan/references/platforms/google.md#handoff).
- **One cause at a time.** A drop usually has one main cause (a lost page, a Google update, lost links, a new competitor, a change on the site). Name the cause the dates support, and say what would confirm it.
- Check the site's own changes first. A redesign, a template change or a robots edit near the break date explains most sudden drops, and it is the one cause the tools cannot see without the user.
- A drop across the whole site within days, with no change on the site and no single competitor gaining, points to a Google update. No tool here lists update dates; the host can check Google's Search Status Dashboard for the date, if it can browse.
- A drop on one page with one new competitor above it is a content or intent problem, not a penalty. The fix is [refresh-content](../refresh-content/SKILL.md) or [optimize-page](../optimize-page/SKILL.md).
- Clicks lost to an AI overview do not come back by ranking higher. Say so plainly, and measure impressions and position as well as clicks from then on.
- Estimates are monthly and modelled. A 15 percent move in `organic_traffic` from one month to the next is within their noise; treat it as a drop only when the user's analytics or the positions agree.
- One cause rarely explains everything. Rank the causes by traffic lost and fix the largest first.

## Related skills

- A crawl of the whole site with no drop named: [audit-technical-seo](../audit-technical-seo/SKILL.md). A drop after a domain change, redesign or replatform: [create-migration-plan](../create-migration-plan/SKILL.md) and its after-launch check.
- One page that fell behind a competitor: [refresh-content](../refresh-content/SKILL.md) or [optimize-page](../optimize-page/SKILL.md). Two of the site's pages swapping on one keyword: [fix-keyword-cannibalization](../fix-keyword-cannibalization/SKILL.md).
- A drop traced to lost links: [reclaim-lost-links](../reclaim-lost-links/SKILL.md). Clicks lost to AI overviews: [check-ai-overviews](../check-ai-overviews/SKILL.md).
- Catching the next drop early: [track-rankings](../track-rankings/SKILL.md) and [monitor-search-console](../monitor-search-console/SKILL.md).
