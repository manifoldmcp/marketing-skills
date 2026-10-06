---
name: find-competitors
description: When the user wants to know who their real competitors are. Finds the direct, indirect and search-only competitors from five sources (Google results, AI answers, comparison searches, the Meta, LinkedIn and TikTok ad libraries, and Reddit) and returns one classified table with the source of every name. Also use when the user mentions who are our real competitors, who else sells what we sell, competitors we have never heard of, who buyers compare us with, alternatives to a product, the competitive landscape, or which of our search rivals are actual competitors. One competitor in depth goes to tear-down-competitor, a recurring watch to monitor-competitors, and the whole market mapped by segment to map-market.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find competitors

Who the user really competes with: the direct competitors buyers compare them with, the indirect ones that do the same job another way, and the search-only sites that take their traffic without selling to their buyers. Five sources vote; a company named by several of them is a real competitor. It ends in a classified table, with the source of every number.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_serp_competitors` and `aeo_run_ai_answers` (hosts often add a prefix, for example `mcp__manifold__seo_get_serp_competitors`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- This skill reads several tool groups: `seo_*`, `aeo_*`, `ads_*`, `reddit_*` and `leads_*`. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run without them, and mark their source column "not checked" rather than leaving it empty, so nobody reads a gap as a zero.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product and its domain, the category words, the competitors the user already knows and the market) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Product**: the user's domain, and one line on what it does and for whom.
- **Category words**: two or three phrases a buyer uses ("scheduling software for clinics", "patient booking app"). Default: from the homepage title and `h1` in step 1.
- **Known competitors**: the ones the user already names, so the table shows who is new.
- **Market**: `location` and `language` if not the United States and English; `country` for the ad libraries.
- **Budget**: a default run costs about 10 + 2 + 54 + 10 + 3 + 6 + 60 = 145 credits, plus 1 for each company record fetched for an unclear candidate. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Describe the product.** `seo_get_page` on the user's homepage (free): `title`, `meta_description` and `h1[]` give the category words when the user has none.
2. **Search.** `seo_get_serp_competitors` on the user's domain (10 credits for 100 rows). Apply the [real competitors](#real-competitors) rule: mark publishers, marketplaces, review sites, directories, forums and video sites search-only (keep the five with the most `shared_keywords`, drop the rest), then call `seo_get_page` (free) on each remaining homepage and read `title` and `meta_description`. The same job for the same buyer makes a direct candidate; anything else is search-only. Then `seo_get_serp` for "<category> alternatives" and "best <category>" (1 credit each): the vendors on page one are candidates, and a list article's `h2[]` (`seo_get_page`, free) names the products it ranks.
3. **AI answers.** `aeo_run_ai_answers` with three prompts ("best <category>", "best <category> for <the buyer>", "alternatives to <best-known competitor>") and `brands` holding the user and the known competitors (ten at most), on the default engines (18 credits per prompt, 54). Call `get_task` (free) after `poll_after_s`. `mentions[]` only detects the brands passed in, so read each `answer` for the other names too, and count in how many of the 15 cells each brand appears. These are the brands buyers hear about when they ask.
4. **Comparison searches.** `seo_search_keywords` with "<best-known competitor> vs" as `seed` (10 credits for 100 rows). Every "X vs Y" and "X alternative" keyword names a product buyers weigh against it, and its `volume` says how many do. Keep the brands that sell the same job; check an unknown one's homepage title (`seo_get_page`, free).
5. **Ads.** `ads_search_ads` with the category words as `query` and `active_only: true` on `facebook`, `linkedin` and `tiktok` (1 credit a page each). Only Facebook applies `active_only`; LinkedIn and TikTok rows carry `active: null`, so keep those whose `last_shown` falls in the last 30 days. An advertiser paying for those words now is competing for the same buyer now.
6. **Reddit.** `reddit_search_posts` for "alternative to <best-known competitor>" and "<category> recommendation" (1 credit each), and `reddit_search_comments` for "switched from <best-known competitor>" (1 credit), all with the default relevance sort and no time limit, per the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching) (the sort and `time_range` to pass, the on-topic check, and what to do when a search fails); keep the rows whose `created_at` falls in the past two years. Then `reddit_get_comments` on the three busiest threads (1 credit each). The names in the replies are what buyers actually compare; the replies also carry the indirect competitors ("we just use a spreadsheet", "our agency does it").
7. **Classify and size.** Merge into one row per company and classify it direct, indirect or search-only per the [real competitors](#real-competitors) rule. `seo_get_traffic_estimates` on the kept domains in one call (50 plus 50 per 100 domains: about 60 for 20) for `organic_traffic` as a size signal. Fetch `leads_get_company` (1 credit) for a candidate whose homepage leaves the class unclear.
8. **Deliver** a table: company, domain, class (direct, indirect, search-only), what they sell (from the title or description), the sources that named them (search, AI answers with the cell count, comparison keywords, ads, Reddit), `organic_traffic`, known to the user (yes or no), and one evidence link. Direct competitors first, sorted by the number of sources; indirect next; search-only last. Name the two or three to tear down next with [tear-down-competitor](../tear-down-competitor/SKILL.md).

## Judgment

- The number of independent sources is the best signal. A brand in AI answers, Reddit threads and active ads is a real competitor whatever its traffic; a domain found only by `seo_get_serp_competitors` is search-only until something else names it.
- The top SERP competitors are usually G2, Capterra, Forbes and the like. They sell attention, not the product: search-only, never direct.
- A competitor the user has never heard of, named across several engines, is the finding of the run. Say it first.
- For an early product the biggest competitor is often indirect: the spreadsheet, the agency, doing nothing. Reddit shows it; keep it in the table even with no domain.
- A "vs" keyword with volume is buyers comparing on Google. A brand in several of them is in the buyer's set whatever its own traffic.
- A Reddit search is ranked and never complete. Count the names across threads; do not treat one thread as the market.
- Traffic sizes SEO-led competitors well and sales-led ones badly: an enterprise vendor can have little traffic and a large sales team. `organic_traffic` is an estimate modelled from rankings: good for comparing sites with each other, not a visit count.
- Every claim about a competitor carries its source: the tool and field, or the URL. Mark an inference as an inference.
- Credits: say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. `aeo_run_ai_answers` is never cached, so put every known competitor in `brands[]` (up to 10) on the one run. Other results this account already paid for are free while cached: 7 days for most SEO data (24 hours for `seo_get_serp`), 24 hours for ad lists, 30 days for company records.
- Never contact anyone. The deliverable is the table. After delivering, offer to write the classes, domains and handles into `.agents/product-marketing.md` with [create-product-context](../create-product-context/SKILL.md).

## Real competitors

- A business competitor sells to the same buyer for the same job. A SERP competitor only shares keywords. `seo_get_serp_competitors` returns SERP competitors, and many of its top rows are publishers, marketplaces, review sites and directories. Never report a domain from it as a business competitor until its homepage title or its company description (`seo_get_page`, `leads_get_company`) shows it sells the same thing.
- Three classes. **Direct**: the same job for the same buyer. **Indirect**: the same job done another way: a spreadsheet, an agency, a feature inside a bigger platform, hiring someone, doing nothing. **Search-only**: competes for the user's keywords but not for their buyers: publishers and vendors in adjacent categories. Search-only matters for SEO and never for sales.
- Carry two or three competitors into the next step, not ten. A teardown of every name on a list is expensive and nobody reads it.

## Related skills

- One competitor across every channel: [tear-down-competitor](../tear-down-competitor/SKILL.md). How each one pitches and prices: [compare-messaging](../compare-messaging/SKILL.md).
- The same competitors watched every week: [monitor-competitors](../monitor-competitors/SKILL.md).
- The whole market mapped by segment: [map-market](../map-market/SKILL.md). Search rivals as keyword gaps and "X vs Y" pages: [plan-comparison-pages](../plan-comparison-pages/SKILL.md).
- Whether AI engines recommend a competitor over the user, in depth: [check-ai-visibility](../check-ai-visibility/SKILL.md).
- A plan to beat the ones found here: [create-competitor-plan](../create-competitor-plan/SKILL.md).
