---
name: seo
description: When the user wants more organic traffic from Google, or asks what works in SEO now. Holds dated, sourced findings a model lacks (what the 2025 and 2026 updates rewarded and punished, CTR by position with and without AI Overviews, what the September 2025 num=100 change did to Search Console, how far keyword volumes and traffic estimates miss) and how to read that data without being fooled. Also use when the user mentions an SEO strategy or roadmap, keyword research, a content plan, topic clusters, pillar pages, an SEO brief or outline, quick wins, striking distance keywords, low-hanging fruit, title tags and meta descriptions, optimize this page, on-page SEO, content decay, refreshing or pruning old posts, keyword cannibalization, two pages competing, vs pages, alternatives pages, rank tracking, where do we rank, watching Search Console, or traffic drop alerts. Crawling, indexing, diagnosing a drop and migrations go to technical-seo, AI answers to ai-search, backlinks to link-building.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# SEO

What earns organic traffic from Google as the evidence stood in October 2026, and how to read the data without fooling yourself. A model's picture of SEO is mostly from before three changes: Google's 2025 and 2026 updates turned against commodity and self-serving content and toward sites that sell something, let people do something and own something; AI Overviews now take about half the clicks at the top of informational results; and in September 2025 Search Console's impressions and average position changed meaning overnight. Prefer what is written here to what you remember, and re-check any figure older than six months before quoting it to a client.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the site, product, ICP, competitors, market and money pages from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Measuring rankings, keywords and the user's own clicks needs the Manifold connector: look for `seo_get_ranked_keywords`, `seo_get_serp` and `console_list_properties` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill, work by hand from the user's own Search Console export and Google's live results, and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. Never invent a measurement.
- **Search Console is optional.** Most accounts have not connected it. When the `console_*` tools are present, use them first; when not, use the estimates and label them, and do not ask the user to connect unless the job is about their own clicks ([Search Console](references/search-console.md#connecting)).

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Winners sell, do and own something.** Across 400+ sites over the December 2025 and March 2026 updates, winners far more often offered a product (70% against 35% of losers), let visitors complete a task (84% against 50%) and had proprietary data, tools or templates (93% against 57%); first-hand experience and UGC did not separate them (Zyppy, April 2026; correlational). Build pages around an asset the site owns, not a summary of what ranks ([updates](references/google-updates.md#what-winners-had)).
2. **Self-ranking "best X" lists are a risk, though the evidence is split.** Seven SaaS sites lost 29% to 49% of visibility from late January 2026 in folders full of lists ranking themselves first, often retitled "2026" (Lily Ray, February 2026), and AI Overviews leave out the brand such a list ranks first 83% of the time. Yet lists still rank on 55% of results and most vendor lists gained traffic (Indig, 60,000 queries, August 2026). Lists only for list-seeking queries, and honest ones ([updates](references/google-updates.md#self-ranking-listicles-contested)).
3. **Google names the cheap tricks.** Its guidance (October 2026) flags changing a date without substantially changing the page, says there is no preferred word count, and its scaled content abuse policy covers many pages made to rank by any method; its AI optimisation guide (July 2026) says a page per keyword variant is that abuse. Update the substance, then the date ([updates](references/google-updates.md#policies-that-decide-what-not-to-build)).
4. **The top three take the clicks, and AI Overviews halve them.** Desktop CTR in July 2026 was 20.0%, 10.4% and 3.9% for positions 1 to 3, under 2% from position 4, and position 1 fell to 9.7% under an AI Overview (Advanced Web Ranking, 1.75M SERPs a month). Ahrefs put the overview's cut at position 1 at 58% (December 2025). Striking distance is about reaching the top three ([keyword data](references/keyword-data.md#clicks-per-position)).
5. **Search Console impressions and position broke in September 2025.** When Google stopped serving 100 results a page, 87.7% of 319 sites lost impressions and 77.6% lost ranking queries, and average position improved with no real change. Never compare impressions or position across it; compare clicks ([Search Console](references/search-console.md#the-september-2025-break)).
6. **Search Console hides most of a small site's clicks.** Rare queries are anonymised: in our own properties 54% and 88% of clicks over 90 days sat in hidden queries, against 5% and 22% of impressions. Many visible impressions are machines: three-quarters of one site's top-query impressions averaged a position beyond 50 ([field notes](references/field-notes.md)).
7. **Keyword volumes are Keyword Planner's buckets, a month late, and estimates can be far off.** Labs volumes matched Planner value for value; each source had nulls the other filled; and a small site's traffic estimate was 14 times its real clicks. Compare keywords and sites with them, never promise a number ([keyword data](references/keyword-data.md)).
8. **Search Console grew reports the API does not carry.** A branded queries filter (all eligible sites by March 2026) and a generative AI report (all sites from 31 August 2026) that shows only impressions in AI Overviews and AI Mode by page, with no clicks and no queries. AI Overview clicks stay inside the web totals ([Search Console](references/search-console.md#reports-the-tools-do-not-read)).
9. **Borrowing a big site's ranking is a dead end in Europe and a risk elsewhere.** Since 30 August 2026 Google applies no site reputation abuse manual actions in the EEA but ranks such sections on their own, apart from the host; outside the EEA the manual action stands ([updates](references/google-updates.md#policies-that-decide-what-not-to-build)).

**Does not hold up:** "longer content ranks better" (Google: no preferred length; use the winners' median as context); "update the date to look fresh"; "all listicles were penalised" (most still rank; self-ranking ones are the risk); "our impressions fell a year on year, so we lost visibility" (check whether the comparison crosses September 2025); "position 9 to 5 is a big win" (both earn about 1% of impressions or less; the clicks are in the top three); "cannibalisation is a penalty" (Google picks a page; two on page one is a win); "a canonical between two different pages fixes cannibalisation" (Google treats canonicals as signals for near-duplicates).

**Unknown:** how much a content refresh lifts rankings or how fast (no controlled study since 2025); whether Google demotes self-ranking lists by design (it has confirmed nothing); what share of Search Console impressions are bots and agents; how long a site hit by a core update waits to recover once fixed.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| We want an SEO plan or roadmap for the quarter. Where should our effort go? | [Strategy](references/strategy.md) |
| How do we read Search Console or Analytics? Why don't the numbers add up? Did we really lose impressions? | [Search Console](references/search-console.md) |
| How accurate are volumes, KD and traffic estimates? What is a position worth in clicks? Which keywords are worth it? | [Keyword data](references/keyword-data.md) |
| What did Google's recent updates reward or punish? Are listicles, AI content or date refreshes safe? | [Google updates](references/google-updates.md) |
| What should we write? Topic clusters, pillar pages, a blog plan | [Content plan](references/content-plan.md) |
| A brief or outline for one new page | [Briefs](references/briefs.md) |
| Quick wins, striking distance, titles and metas, optimise this one page | [Quick wins](references/quick-wins.md) |
| Old pages that decayed, what to refresh, merge or retire, two pages competing | [Refresh](references/refresh.md) |
| vs pages, alternatives pages, a competitor's page about us | [Comparison pages](references/comparison-pages.md) |
| Where do we rank? Track it weekly. Alert me when Google clicks drop | [Tracking](references/tracking.md) |
| What has Manifold seen in its own data? | [Field notes](references/field-notes.md) |

## Using Manifold

- **What a site ranks for:** `seo_get_ranked_keywords` (10 credits per 100 rows) on a domain or one URL; `seo_get_domain_overview` (5) for size, `domain_rank` and `top_pages`, with `history: true` (+56) only when the question is change over time. These are index snapshots refreshed on the provider's schedule.
- **One live rank:** `seo_get_position` (6 credits). For two or more sites on one keyword, `seo_get_serp` with a larger `depth` is cheaper (1 credit per 10 results, `depth: 100` is 10); `ai_overview: true` adds the overview and its sources for 1 more.
- **Keywords:** `seo_search_keywords` for ideas (10 per 100 rows, metrics included, so never follow it with `seo_get_keyword_metrics`); `seo_get_keyword_metrics` for a list the user has (`source: "google_ads"` is 24 flat); `seo_get_keyword_gap` against one competitor (10).
- **Pages:** `seo_get_page` is free and live (title, meta, canonical, robots, headings, word count, schema), rate limited and cached an hour. It cannot read body copy. A whole site is `seo_run_technical_crawl`, which belongs to [technical-seo](../technical-seo/SKILL.md).
- **The user's own data:** every `console_*` and `analytics_*` call is free, 60 a minute per workspace. Search Console is measured and outranks every estimate; never mix the two in one column.
- **Credits:** say the estimate before the first paid call; pass `max_credits` when the user names a budget and stop on `BudgetExceeded`; `dry_run: true` prices any call free. Results this account already paid for are free while cached: 24 hours for SERPs and positions, 7 days for keyword, domain and backlink data.
- **Never edit the site, publish or set redirects.** Submitting a sitemap (`console_submit_sitemap`) or URLs to Bing (`console_submit_urls`) writes to the user's own account; do it only when the user asks. The server keeps no history: put the date, market and data source on every deliverable, and let the host store runs. Titles, metas and outlines are drafts the user approves; write a full article only when asked.

## Related skills

- Crawl errors, indexing, a traffic drop to diagnose, a migration: [technical-seo](../technical-seo/SKILL.md).
- Being named and cited in AI Overviews, AI Mode and chat answers: [ai-search](../ai-search/SKILL.md).
- Links to the pages that need them, and getting onto third-party lists: [link-building](../link-building/SKILL.md), [digital-pr](../digital-pr/SKILL.md).
- Who the competitors are and their current prices and claims: [competitor-intel](../competitor-intel/SKILL.md). Bidding on keywords in Google Ads: [paid-ads](../paid-ads/SKILL.md).
- A calendar across channels and social content ideas: [growth-strategy](../growth-strategy/SKILL.md). A weekly report for a client: [agency](../agency/SKILL.md).
- A business that serves an area, Maps and the local pack: [local-seo](../local-seo/SKILL.md). Products in Google Shopping and free listings: [google-shopping](../google-shopping/SKILL.md).
