---
name: create-migration-plan
description: When the user wants to keep Google rankings through a site migration. Before launch, inventories the URLs that rank, earn clicks or have backlinks and builds a one-to-one redirect map; after launch, crawls the new site, tests every redirect and checks that positions and indexing held, with DataForSEO and the user's Google Search Console when connected. Covers a domain change, a new URL structure, a replatform, a redesign or merging a subdomain into the main site. Also use when the user mentions a site migration, we're changing domains, a redirect map, moving to Webflow or another CMS, a replatform, check the redirects after launch, or merging blog.example.com into the main site. A crawl with no move planned goes to audit-technical-seo, a drop with no migration behind it to diagnose-traffic-drop. Setting the redirects is out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Site migration

A migration (a new domain, a new URL structure, a new CMS, a redesign, or merging a subdomain into the main site) keeps its rankings when every URL that earns traffic or links has a one-to-one redirect to its new home, and the new pages carry what the old ones did. This skill has two halves: before launch, an inventory of the URLs that matter and a redirect map; after launch, a check that the redirects work, the new site is healthy and the positions held.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_run_technical_crawl` and `seo_get_ranked_keywords` (hosts often add a prefix, for example `mcp__manifold__seo_get_ranked_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Then follow the tool checks in the [Google search notes](../create-seo-plan/references/platforms/google.md#tools): the `seo_*` tools are required, the `console_*` tools optional.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the Search Console property, the market, the keywords that matter) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **What changes**: the domain, the URL paths, the platform, the templates, or several at once. Ask for the old and new domains and a sample of old and new URLs.
- **Launch date**, and whether the user is before or after it. After launch, skip to step 5.
- **Search Console**: whether the `console_*` tools are there. The after-check is much better with them: real clicks per page, and Google's own view of the redirects and the new sitemap. The [Google search notes](../create-seo-plan/references/platforms/google.md#search-console) say how to check. The inventory runs in full without it.
- **URL list**: a sitemap or CMS export of every old URL, if the user has one. The tools find the URLs that rank and the URLs with links; they do not list every URL.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: before launch about 5 + 55 + 25 + 30 = 115 credits; after launch about 30 + 10 x 6 + 55 = 145. `seo_get_page` and `get_task` are free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

Before launch:

1. **Baseline the old site.** `seo_get_domain_overview` on the old domain (5 credits): estimated `organic_traffic`, `organic_keywords` and `top_pages`. Then `seo_run_technical_crawl` with `max_pages` sized to the site (3 credits per 100 pages, 30 for 1,000) and `get_task` (free) after `poll_after_s`. The crawl returns counts and up to 10 sample URLs per issue, not every URL: keep `status_codes`, `non_indexable` and `issues[]` as the health baseline, and note the issues not to carry over.
2. **List the URLs that matter.**
   - Ranking URLs: `seo_get_ranked_keywords` on the old domain with `limit: 1000` (55 credits). Group by `url`, add up `traffic`, and keep each URL's keywords and `rank`. This is also the baseline of positions for step 7.
   - Linked URLs: `seo_get_backlinks` on the old domain with `limit: 1000` (25 credits). Group the live rows by `url_to` and count the linking domains per URL.
   - With Search Console: `console_get_search_analytics` with `dimensions: ["page"]` for the last 12 months and `limit: 1000`, following `cursor` (free). This is the fullest list: every page that earned a click.
   - Add the user's sitemap or export for everything else.
3. **Build the redirect map.** One row per old URL that ranks, earns clicks or has links: old URL, new URL, 301. Map each to its closest equivalent: the same content first, then the closest topic, then the parent category. Never send them all to the homepage; Google treats that as a soft 404 and the page's rankings are lost. Old URLs with no traffic, no links and no equivalent can return 404 or 410.
4. **Record what the pages carry.** `seo_get_page` on the top 50 old URLs by traffic and links (free, rate limited): `title`, `meta_description`, `h1`, `canonical` and `schema_types`. The new pages should keep them unless there is a reason to change; a migration that also rewrites every title cannot tell what lost the rankings.

After launch:

5. **Crawl the new site.** `seo_run_technical_crawl` on the new domain (30 credits for 1,000 pages) and `get_task`. Compare with the baseline: new 4xx or 5xx pages, `non_indexable` pages (a noindex left over from staging is the classic mistake), `duplicate_titles`, and `is_redirect` or `canonical_chain` issues from internal links that still point at old URLs.
6. **Test every redirect.** `seo_get_page` on each old URL in the map (free, rate limited). It follows redirects: `final_url` must equal the mapped new URL and `status` must be 200. A `final_url` on the homepage, a 404, or a URL that did not move is a broken row. The tool shows where a redirect ends, not whether it was a 301 or a 302 or how many hops it took. The host checks that if it has a fetch tool, or the user with a redirect checker; the crawl's `status_codes` counts 301 against 302 only for old URLs still linked inside the site.
7. **Check positions and indexing.** A week after launch, `seo_get_position` on the ten keywords that brought the most traffic, with the new domain as `target` (6 credits each): the `url` should be the new URL at a similar `rank`. After two to four weeks, `seo_get_ranked_keywords` on the new domain with `limit: 1000` (55 credits) and compare with the step 2 baseline, URL by URL. With Search Console: `console_get_sitemaps` on the new property for the new sitemap's read date and errors, `console_inspect_url` on the top ten new URLs for `coverage_state` and `google_canonical`, and daily clicks with `dimensions: ["date"]` on the old and new properties (all free).
8. **Deliver** before launch the redirect map: old URL, clicks or estimated traffic (say which), linking domains, top keyword and rank, new URL, status code to return, notes. After launch the check: old URL, `final_url`, status, matches the map (yes or no), rank before, rank now, issue, fix.

## Judgment

- Follow the [Google search notes](../create-seo-plan/references/platforms/google.md): [Search Console](../create-seo-plan/references/platforms/google.md#search-console) against estimates, the [credits](../create-seo-plan/references/platforms/google.md#credits) and the [handoff](../create-seo-plan/references/platforms/google.md#handoff). The redirect map is for the user's developer; never set redirects.
- A dip of a few weeks is normal, and a domain change takes longer than a path change. A page that has not recovered in six to eight weeks is a problem to investigate, usually a redirect, a canonical or a lost section of content.
- Keep the redirects for at least a year, and update internal links to the new URLs rather than leaning on redirects.
- Change as little at once as possible. A new domain, new URLs and new content together make any loss impossible to pin down.
- On a domain change, the user also runs Search Console's change of address tool and submits the new sitemap. No tool here can; it is a step for the user's checklist.
- Links pointing at old URLs pass through the redirects. On a domain change, ask the sites behind the strongest links from step 2 (by `dr_from`) to update them to the new URLs, through the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps): a direct link does not depend on a redirect being kept. If the map missed URLs, [reclaim-lost-links](../reclaim-lost-links/SKILL.md) finds the broken pages that still have links.
- The estimates lag: `seo_get_ranked_keywords` results are cached for 7 days and the index refreshes on its own schedule. For the first weeks, `seo_get_position` (live) and Search Console are the numbers to trust.

## Related skills

- A health check with no move planned: [audit-technical-seo](../audit-technical-seo/SKILL.md). A fall that the migration does not explain: [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md).
- Broken pages that still have links, and asking sites to update theirs: [reclaim-lost-links](../reclaim-lost-links/SKILL.md), [create-link-building-plan](../create-link-building-plan/SKILL.md).
- Watching positions and clicks in the weeks after launch: [track-rankings](../track-rankings/SKILL.md), [monitor-search-console](../monitor-search-console/SKILL.md).
