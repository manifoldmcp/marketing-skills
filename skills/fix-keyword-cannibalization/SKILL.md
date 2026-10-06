---
name: fix-keyword-cannibalization
description: When the user wants to find pages of their own site that compete for the same Google keyword and decide which one keeps it. Finds the pairs from the user's Google Search Console query and page data (or DataForSEO ranked keywords), keeps the pairs that cost traffic, confirms the page Google shows today, reads both pages, and hands back a merge, retarget or leave decision per pair. Also use when the user mentions keyword cannibalization, two of our pages compete for the same keyword, Google keeps switching which page ranks, should we merge these two posts, or two guides that rank for the same searches. One page's on-page fixes go to optimize-page, refreshing decayed pages to refresh-content, a sitewide crawl for duplicate titles to audit-technical-seo, a page that fell behind a competitor to diagnose-traffic-drop.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Keyword cannibalization

Two pages of the same site aimed at one keyword split its clicks and links, and Google keeps swapping which one it shows. Neither page ranks as well as one page would. This skill finds the keywords where two of the site's URLs compete, keeps the pairs that cost traffic, and decides for each pair which page owns the keyword: merge, retarget or leave.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_ranked_keywords` and `seo_get_position` (hosts often add a prefix, for example `mcp__manifold__seo_get_ranked_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Then follow the tool checks in the [Google search notes](../create-seo-plan/references/platforms/google.md#tools): the `seo_*` tools are required, the `console_*` tools optional.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the Search Console property, the market, the keywords that matter) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Site**: the user's domain, or one section of it.
- **Search Console**: whether the `console_*` tools are there. This job is much better with them: real clicks per query and page show the split, and the estimates may keep only the best-ranking URL per keyword. The [Google search notes](../create-seo-plan/references/platforms/google.md#search-console) say how to check.
- **Suspect pages**: pages the user already thinks overlap (two posts on one topic, a blog post and a product page for the same query). Optional.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: about 55 + 10 x 6 + 5 x 12 = 175 credits without Search Console, about 120 with it, plus 10 for each suspect page; `seo_get_page` is free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the pairs.**
   - With Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["query", "page"]` for the last 90 days and `limit: 1000`, following `cursor` (free). Keep the queries where two or more pages each have at least about 10 percent of the query's impressions (a rule of thumb). To catch Google swapping the page, run the same call for the 90 days before: a query whose top page changed between the windows is a pair even if one page now holds it.
   - Without it: `seo_get_ranked_keywords` on the domain with `limit: 1000` (55 credits). Rows with the same `keyword` and a different `url` are pairs. The index may keep only the best-ranking URL per keyword, so for each suspect page also run `seo_get_ranked_keywords` with the page URL as `target` (10 credits each) and compare: keywords both pages rank for are the overlap.
2. **Keep the pairs that cost traffic.** Apply the [keyword floors](../create-seo-plan/references/platforms/google.md#keyword-floors). Drop pairs where one page is in the top 3 and the other also sits on page one: that is two listings, not a split. Drop overlap on long-tail variants when each page ranks best for its own main keyword. What is left are the main keywords where both pages sit below the top 3, or the page shown is the wrong one.
3. **Confirm the live page.** For the ten most valuable keywords left, `seo_get_position` with the domain as `target` (6 credits each): `url` is the page Google shows today, and `above[]` shows who sits ahead. A `url` that differs from the page with the most clicks in step 1 is a swap.
4. **Read both pages.** `seo_get_page` on each page of each pair (free): `title`, `h1`, `h2`, `word_count`, `canonical` and `status`. Same intent and the same headings means one page is redundant; different intents (a guide and a product page) mean the titles and headings blur them.
5. **Decide per pair.**
   - **Merge** when both pages serve the same intent. Keep the URL with more clicks (or estimated `traffic`) and links, fold the other's unique sections into it, and 301 the old URL. Before merging, `seo_get_backlinks` on the URL that goes (12 credits) lists the links the redirect must carry.
   - **Retarget** when the intents differ. Give each page its own main keyword in `title` and `h1`, cut the overlapping section from the weaker one, and point internal links with the keyword as anchor text at the page that owns it.
   - **Leave** when both pages hold page one, or the overlap is only long-tail.
6. **Deliver** a table: keyword, volume, page A and page B (URL, clicks or estimated traffic, position; say which source), the page Google shows now, intent of each, decision (merge into which URL, retarget to which keyword, leave), links on the page that goes, and the internal links to change.

## Judgment

- Follow the [Google search notes](../create-seo-plan/references/platforms/google.md): [Search Console](../create-seo-plan/references/platforms/google.md#search-console) against estimates, the [keyword floors](../create-seo-plan/references/platforms/google.md#keyword-floors), the [credits](../create-seo-plan/references/platforms/google.md#credits) and the [handoff](../create-seo-plan/references/platforms/google.md#handoff).
- Two of the site's URLs on page one for one keyword is a win, not a problem. Act only where the split keeps both pages below the top 3, or Google swaps them week to week.
- Do not fix a split with `noindex` or a cross-canonical between pages whose content differs. Google treats a canonical as a hint and ignores it when the pages are not duplicates; merge or retarget instead.
- Internal links decide ties. If the blog links to the guide with the keyword as anchor text and the nav links to the product page, Google gets two answers; make every internal link for that keyword point at the owner.
- After merging or retargeting, re-check after two to four weeks with [track-rankings](../track-rankings/SKILL.md). Pages that were split often jump once the signals point one way.

## Related skills

- The page that keeps the keyword, tuned against the winners: [optimize-page](../optimize-page/SKILL.md).
- Decayed pages to refresh, merge or retire: [refresh-content](../refresh-content/SKILL.md).
- Duplicate titles and canonicals across the whole site: [audit-technical-seo](../audit-technical-seo/SKILL.md).
- Checking the positions after the change: [track-rankings](../track-rankings/SKILL.md).
