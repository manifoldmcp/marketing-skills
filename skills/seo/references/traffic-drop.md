# Traffic drop

A drop has a date, a place and a cause. This playbook finds when organic traffic fell and which pages and keywords lost it, then tests the causes one by one: less demand, competitors who took the positions, AI overviews and other features taking the clicks, technical breakage, and lost links. It ends in a verdict with the evidence and the fix for each cause found.

## Inputs to settle first

- **Site**: the user's domain, and the pages or section they think fell.
- **When**: the date the user noticed, and where they saw it (analytics, Search Console, sales).
- **Changes**: anything that shipped around then: a redesign, a new CMS or template, a domain or URL change, deleted or merged pages, robots.txt or noindex edits. Ask; it is the most common cause and no tool can see the change log.
- **Search Console**: whether the `console_*` tools are there. This job is much better with them: real daily clicks, 16 months of history and index status. The [router](../SKILL.md#search-console) says how to check.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: without Search Console about 61 + 10 + 30 + 10 x 2 + 5 x 6 + 15 + 10 = 176 credits; with it about 20 + 30 + 15 + 10 = 75. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Confirm and date it.**
   - With Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["date"]` over the last 16 months and `limit: 500`, one row per day (free). Find the day the line breaks, and compare the same weeks a year earlier to rule out seasonality. Read clicks and impressions together: impressions down means rankings or indexing; impressions flat with clicks down means the page one changed (steps 4 and 5) or the titles did.
   - Without it: `seo_get_domain_overview` with `history: true` (61 credits): 12 months of estimated `organic_traffic` and `organic_keywords`. The data is monthly and modelled, so it dates a drop to the month at best, and a fall in `organic_keywords` alongside traffic means rankings were lost, not just clicks.
2. **Find where.**
   - With Search Console: `console_get_search_analytics` with `dimensions: ["page"]` for four weeks before and four weeks after the break (two calls, free). Rank pages by clicks lost. For the top five pages, call it again with `dimensions: ["query"]` and a `filters` entry on that page, before and after, to name the keywords.
   - Without it: the estimates show where the site ranks now, not where it ranked before. Ask the user for the landing pages that fell in their analytics. Call `seo_get_ranked_keywords` on the domain with `limit: 500` (30 credits) for the current ranks, and compare with the pages and keywords the user names. A page the user says used to bring traffic that now has few rows here, or rows at 11 and below, is where the loss sits.
3. **Rule out demand.** Take the 20 keywords that brought the most traffic (from step 2) and call `seo_get_keyword_metrics` (10 credits). If `trend[12]` falls in the same months, searchers left, not Google. Skip this with Search Console, where the year-over-year comparison in step 1 already answers it.
4. **Who took the positions.** For the 10 keywords that lost the most, `seo_get_serp` with `ai_overview: true` (2 credits each). Read what now ranks: a competitor that took several of them, a different format (lists replacing product pages, forum rows with `type: "discussions_and_forums_element"`, videos), or another URL of the user's own site (two pages competing, or a page that moved). If the site is not on the page, `seo_get_position` on the five most valuable keywords (6 credits each) finds its rank now, and `above[]` names who is ahead.
5. **AI overviews and features.** In the same SERPs, a non-null `ai_overview` on a query where impressions held but clicks fell means the answer now sits on Google's page. Check whether the site is among `ai_overview.references[]`. A new `featured_snippet` held by a rival does the same. The fix is on the page (a direct answer early, the format that wins the snippet); being cited in AI answers is the `ai-search` group's job.
6. **Technical breakage.** `seo_get_page` on each page that lost (free): `status`, `final_url` (a redirect the user did not intend), `robots_meta` (noindex), `canonical` (pointing elsewhere), and a `title` or `h1` that no longer matches the keywords it lost. If several sections fell at once, `seo_run_technical_crawl` with `max_pages: 500` (15 credits), then `get_task` (free) after `poll_after_s`: `status_codes`, `non_indexable`, `broken_links` and `issues[]`. With Search Console, `console_inspect_url` on the five biggest losers gives `coverage_state`, `google_canonical` against `user_canonical` and `last_crawl_at`, and `console_get_sitemaps` shows sitemap errors (both free).
7. **Lost links.** `seo_get_backlink_summary` on the site (10 credits): `referring_domains` and `broken_backlinks` now. The summary has no history: `broken_backlinks` above zero means links point at pages that now fail, and links that sites removed show only in the `lost` flag of each link. When links may be the cause, the `link-building` group's [lost links](../../link-building/references/lost-links.md) playbook finds and recovers them.
8. **Deliver** a verdict in two lines, then a table: cause, evidence (the numbers and their source), pages affected, keywords affected, clicks or estimated traffic lost, the fix, owner. List the causes you tested and ruled out, with the evidence, so the user does not chase them again.

## Judgment

- Check the site's own changes first. A redesign, a template change or a robots edit near the break date explains most sudden drops, and it is the one cause the tools cannot see without the user.
- A drop across the whole site within days, with no change on the site and no single competitor gaining, points to a Google update. No tool here lists update dates; the host can check Google's Search Status Dashboard for the date, if it can browse.
- A drop on one page with one new competitor above it is a content or intent problem, not a penalty. The fix is the [content refresh](content-refresh.md) or [optimize a page](optimize-page.md) playbook.
- Clicks lost to an AI overview do not come back by ranking higher. Say so plainly, and measure impressions and position as well as clicks from then on.
- Estimates are monthly and modelled. A 15 percent move in `organic_traffic` from one month to the next is within their noise; treat it as a drop only when the user's analytics or the positions agree.
- One cause rarely explains everything. Rank the causes by traffic lost and fix the largest first.
