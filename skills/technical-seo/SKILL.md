---
name: technical-seo
description: When the user wants to know if Google can crawl, render and index their site, why organic traffic fell, or how to move a site without losing rankings. Holds dated, sourced findings a model lacks (the September 2025 num=100 change that cut Search Console impressions with no real loss, the 2MB fetch limit, 2025 and 2026 update dates, how JavaScript, noindex and canonicals are handled now, how little Core Web Vitals weigh, how slowly domain migrations recover) and how to read crawl and Search Console data right. Also use when the user mentions technical SEO, an SEO audit or health check, crawl our site, broken links, duplicate titles, noindex or canonical problems, redirect chains, crawled currently not indexed, indexing problems, Core Web Vitals or INP, our traffic dropped, we lost rankings, was it a Google update, clicks down but impressions flat, a site migration, changing domains, a redirect map, a replatform, redesign or subdomain merge. Content and rankings go to seo, AI crawlers and llms.txt to ai-search.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Technical SEO

Whether Google can crawl, render, index and choose the right pages, why traffic fell, and how to move a site without losing it, as the evidence stood in October 2026. Much of what a model remembers here is out of date: Google rewrote its crawling, JavaScript and canonical documentation between late 2025 and mid 2026, now documents a 2MB fetch limit where the old figure was 15MB, and Search Console changed what it counts twice in a year, so many "drops" since September 2025 are measurement changes rather than lost rankings. Prefer what is written here to what you remember, and check Google's Status Dashboard and Search Console's data anomalies page for anything after this date.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the domain, the Search Console property, the market and the pages that matter from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. Always ask what shipped on the site around a drop or a launch: no tool can see the change log.
- **Connector.** The knowledge here needs no tools. Crawling, live page checks, rankings and the user's Search Console need the Manifold connector: look for `seo_run_technical_crawl`, `seo_get_page` and `console_inspect_url` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill and use the by-hand methods in [audit](references/audit.md#without-the-connector), [traffic drops](references/traffic-drops.md#without-the-connector) and [migrations](references/migrations.md#without-the-connector), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Check that anything fell before diagnosing it.** Around 12 September 2025 Google stopped honouring the `num=100` results parameter. Search Console impressions fell and average position "improved" with clicks unchanged: 87.7% of 319 properties lost impressions (LOCOMOTIVE, September 2025). Separately, Google reports an impressions logging error from 13 May 2025 to 27 April 2026. Compare clicks across those dates, never impressions or position ([traffic drops](references/traffic-drops.md#the-september-2025-impressions-cliff)).
2. **Five core updates from March 2025 to May 2026, plus unannounced smaller ones.** March, June and December 2025, March and May 2026, each rolling out over 12 to 18 days, with five spam updates and a Discover-only update between them. Since December 2025 Google says smaller core updates run unannounced, and recovery can wait for the next core update, months away ([timeline](references/timeline.md)).
3. **The shape of the break names the cause.** A one-day site-wide cliff is the site, its tracking or a manual action; a slide over one to three weeks starting on a rollout date is an update; a slow decline on some pages is a page-level fault biting as Google recrawls; the same dip a year earlier is seasonality (Google's debugging guide, December 2025).
4. **Googlebot reads only the first 2MB of a file** (documented February 2026; 15MB is now the default for Google's other crawlers). Bytes past the cutoff are not indexed or rendered, canonicals and structured data included ([indexing](references/indexing.md#crawling-and-crawl-budget)).
5. **Google renders JavaScript, with three traps.** Every 200 page is rendered, in a median 10 seconds and 18 hours at the 99th percentile (Vercel and MERJ, 100,000+ fetches, 2024). But pages with a non-200 status may not be rendered, a `noindex` in the raw HTML stops rendering so JavaScript cannot remove it, and a canonical set by JavaScript must match the raw HTML (all December 2025). AI answer crawlers mostly do not render at all.
6. **Crawl budget is a big-site problem.** Google's guide is for about a million pages changing weekly or 10,000 changing daily. On a small site "Discovered, currently not indexed" and "Crawled, currently not indexed" usually mean Google sees little value in the URLs or they are poorly linked, not a capacity limit; blocking URLs in robots.txt frees nothing.
7. **Core Web Vitals weigh little.** Google says there is "no single signal" for page experience and good scores guarantee nothing (updated September 2026). 48% of sites pass on mobile (Web Almanac 2025). Fix templates that are poor and earn traffic; never sell speed as a ranking fix for a relevance problem ([Core Web Vitals](references/core-web-vitals.md)).
8. **Domain migrations recover slowly.** Across 892 domain migrations, the new domain took an average 523 days to match the old one's estimated traffic and 17% had not after 1,000 days (Search Engine Journal, January 2025; estimates, no control for execution). Google expects weeks for a well-run medium site. Map every URL with clicks or links one to one, change as little else as possible, and submit Change of Address for every variant of the old domain (June 2026) ([migrations](references/migrations.md)).
9. **Search Console now separates AI and brand.** AI Mode has counted in the main totals since June 2025; the Generative AI performance report (all sites since August 2026) shows AI Overview and AI Mode impressions per page, without clicks; the Branded queries filter (all eligible sites since March 2026) splits brand demand from rankings.

**Does not hold up:** "a September 2025 impressions drop means lost rankings"; "Googlebot reads 15MB"; "Google can't render JavaScript, so use dynamic rendering"; "remove `noindex` with JavaScript once the page loads"; "rel=canonical is a directive"; "every site has a crawl budget problem"; "block pages in robots.txt to remove them from Google" (a blocked page can still be indexed); "a perfect Lighthouse score lifts rankings"; "a `site:` search counts indexed pages"; "a well-run migration costs nothing".

**Unknown:** the ranking weight of Core Web Vitals, or of INP alone; no controlled study since 2024. How much of the slow migration recoveries is execution rather than the move itself. Whether Google corrected the May 2025 to April 2026 impressions data retroactively.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| Audit our site. What is broken? What do the crawl's flags mean and which matter? | [Audit](references/audit.md) |
| Why is this page not indexed? Crawled not indexed, canonicals, JavaScript, crawl budget, robots.txt | [Indexing](references/indexing.md) |
| Our traffic dropped. Was it a Google update? Impressions fell but clicks did not | [Traffic drops](references/traffic-drops.md) |
| What changed when? Update dates, Search Console reporting changes | [Timeline](references/timeline.md) |
| Do Core Web Vitals or INP matter? Is our site too slow? | [Core Web Vitals](references/core-web-vitals.md) |
| We are changing domains, URLs, CMS or design. Redirect map, launch checks | [Migrations](references/migrations.md) |
| What has Manifold seen in its own data? | [Field notes](references/field-notes.md) |

## Using Manifold

- **The site as a crawler sees it:** `seo_run_technical_crawl` with `max_pages` sized to the site (up to 1,000; 5 credits per 100, 50 rendered; charged on pages requested), read with `get_task`. `onpage_score` is DataForSEO's, not Google's. `broken_links` counts links, not pages, and `non_indexable` appears to count uncrawled URLs too, so it can exceed `pages_crawled`; trust `status_codes` and the sample URLs. It did not cross to subdomains in our test: crawl each as its own `target`. It loads no resources, so `broken_resources` 0 is not a pass, and `has_render_blocking_resources` and `low_content_rate` fired on most pages of a healthy site in our test ([field notes](references/field-notes.md)). No Core Web Vitals or Lighthouse.
- **One live page:** `seo_get_page`, free and cached an hour: `status`, `final_url`, `canonical`, `robots_meta`, `word_count` without JavaScript. A tiny `word_count` on a full-looking page means the content needs JavaScript.
- **Google's own view, free:** `console_get_search_analytics` (measured clicks and impressions, 16 months, about two days' lag), `console_inspect_url` (Google's last indexed crawl, not the live page; 2,000 a day per property), `console_get_sitemaps` (its `urls_indexed` is always 0). `console_get_crawl_issues` is Bing's crawl failures. How to read Search Console and the keyword estimates in general is in [seo](../seo/SKILL.md).
- **Rankings when Search Console is missing:** `seo_get_domain_overview` with `history: true` (71 credits, monthly estimates), `seo_get_ranked_keywords` (cached seven days), `seo_get_position` and `seo_get_serp` (live). Label every number measured or estimated, and never mix them in a column.
- **Whether a drop is organic only:** `analytics_get_report` by `sessionDefaultChannelGroup` and date (free); every channel falling together is tracking or the site.
- **Never edit the site, set redirects or run Change of Address.** Deliver tables the user's developer acts on. `console_submit_sitemap` (Google or Bing) and `console_submit_urls` (Bing only, about 10 URLs a day for a new site) are writes to the user's own account: use them only when the user asks, after the fix is live, and never as a fix for a page Google chose not to index.

## Related skills

- Keywords, content, rankings, Search Console in general and pages that lost to competitors: [seo](../seo/SKILL.md). AI crawler access, llms.txt and AI Overviews: [ai-search](../ai-search/SKILL.md).
- Links lost in a drop or a migration, and asking sites to update theirs: [link-building](../link-building/SKILL.md).
- A technical audit for a prospect or a client report: [agency](../agency/SKILL.md). Where technical work fits in a quarter's growth plan: [growth-strategy](../growth-strategy/SKILL.md).
