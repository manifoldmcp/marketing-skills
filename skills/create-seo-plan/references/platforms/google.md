# Google search notes

What every skill that reads Google organic search shares: which tools it needs, Search Console against estimates, the keyword floors, the credits and the handoff. Every skill that calls a `seo_*` or `console_*` tool follows these notes.

## Tools

- If other manifold tools are there but the `seo_*` tools are not, the SEO tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: every Google search skill needs it.
- The `console_*` tools (the user's own Google Search Console and Bing Webmaster data) are optional, and most accounts do not have them yet. Their absence never blocks a skill; see [Search Console](#search-console).

## Search Console

Search Console holds the user's real clicks, impressions, CTR and average position, plus index status and sitemaps. The skills that are much better with it ([quick wins](../../../find-seo-quick-wins/SKILL.md), [traffic drop](../../../diagnose-traffic-drop/SKILL.md), [content refresh](../../../refresh-content/SKILL.md), [optimize a page](../../../optimize-page/SKILL.md), a [rank check](../../../track-rankings/SKILL.md) on the user's own site, and the [site migration](../../../create-migration-plan/SKILL.md) after-check) use it when it is there and run in full without it.

- **With it.** If the `console_*` tools are present, call `console_list_properties` first (free). Pick the property that covers the site: `sc-domain:example.com` covers every host and protocol, a URL property only that prefix. If it returns `NotConnected`, give the user its `connect_url` once, then carry on with the estimates. The console tools are free, limited to 60 calls a minute per workspace; Google's data lags about two days and goes back 16 months.
- **Without it.** If the tools are absent, go straight to the estimates and do not ask the user to connect anything. The estimate path runs on DataForSEO: `seo_get_ranked_keywords` on the user's domain or one URL (rank, volume and estimated `traffic` per keyword), `seo_get_domain_overview` with `history: true` (+56 credits, 12 months of estimated traffic and keyword counts), `seo_get_position` (one live rank, 6 credits) and `seo_get_serp` (the live page one, 1 credit per 10 results).
- **Label every number.** Search Console is measured; `traffic` and `organic_traffic` are models built from rank, volume and a click curve. Estimates are good for direction and for comparing sites, and can be far off for one site, most of all on long-tail and brand queries. Never mix the two in one column, and say which one each table uses.

## Keyword floors

- **Striking distance** is rank 4 to 20. At 1 to 3 little is left to gain; beyond 20 a page needs new content, not a fix.
- **Volume.** Skip keywords under about 50 monthly searches, except high-intent ones (a competitor's name with alternative, vs or pricing; "X software for Y"), where 20 buyers beat 2,000 browsers. A `volume` of null means no data, not zero: judge those by intent.
- **Difficulty.** Measure the site's reach from what it already wins: the median `kd` of the keywords where it ranks in the top 10 in `seo_get_ranked_keywords`. Aim at or below that, and up to about 10 above it for pages that will get links. A site with no top-10 rankings starts under KD 20.
- **Intent.** When the goal is signups or sales, `intent` commercial and transactional come first; informational keywords earn reach and support the money pages. Navigational keywords for another brand are off limits except on comparison pages.
- **Format.** The format of Google's top three (a list, a guide, a tool, a product page, a forum thread) is the format to match. A page in the wrong format does not rank by adding words.

## Credits

- Say the estimate before the first paid call; each skill gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Free first: `seo_get_page`, `get_task` and every `console_*` tool cost nothing (they are rate limited, so many URLs take time rather than credits). `seo_get_serp` is 1 credit per 10 results.
- Ranks in bulk, not one by one: `seo_get_position` is 6 credits per keyword and target, while `seo_get_ranked_keywords` is 10 credits per 100 rows. For two or more targets on one keyword, `seo_get_serp` with a larger `depth` is cheaper than a position per target: even `depth: 100` is 10 credits.
- `history: true` on `seo_get_domain_overview` adds 56 credits: use it only when the question is about change over time.
- `seo_run_technical_crawl` charges on `max_pages` requested, not pages crawled, so size it to the site. `render: true` costs 10 times as much: use it only when the content needs JavaScript.
- A result this account already paid for is free while it is cached: 24 hours for SERPs and positions, 7 days for keyword, domain and backlink data.

## Handoff

- The server only reads. Never edit the site, publish, submit sitemaps, request indexing or set redirects. The deliverable is a table or a brief for the user, their writer or their developer. If the host has a CMS or document tool, offer to pass the result to it; do not publish.
- Titles, meta descriptions and outlines in a deliverable are drafts the host writes from the evidence in the table. Write a full article only when the user asks.
- The server keeps no history. Put the date, the market and the source (Search Console or estimate) on every deliverable so a later run can compare. Checks on a schedule belong to [track-rankings](../../../track-rankings/SKILL.md) and [monitor-search-console](../../../monitor-search-console/SKILL.md).
