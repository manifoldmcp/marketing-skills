---
name: optimize-page
description: When the user wants one existing page to rank higher on Google for its keyword. Confirms the page's target keyword from the user's Google Search Console or DataForSEO, checks nothing technical holds it back, compares its title, meta description, headings, length, schema and links with the top results, and hands back a change list for that one page. Also use when the user mentions optimize this page, on-page SEO for this URL, an on-page review, why does this page rank 12th, how do we get this page higher, or improve our pricing page for X. A new page goes to write-seo-brief, many pages at once to find-seo-quick-wins or refresh-content, two of the site's pages competing for the keyword to fix-keyword-cannibalization, a sitewide crawl to audit-technical-seo.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Optimize a page

One existing URL, measured against the keyword it should rank for and the pages that beat it. This skill confirms which keyword the page targets, checks that nothing technical holds it back, compares it with the top results element by element, and ends in a change list for that one page. For a new page, use [write-seo-brief](../write-seo-brief/SKILL.md); for many pages at once, [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md) or [refresh-content](../refresh-content/SKILL.md).

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_page` and `seo_get_serp` (hosts often add a prefix, for example `mcp__manifold__seo_get_page`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Then follow the tool checks in the [Google search notes](../create-seo-plan/references/platforms/google.md#tools): the `seo_*` tools are required, the `console_*` tools optional.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the Search Console property, the market, the keywords that matter, customer language for the title) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **URL**: the page. As a `target`, the full URL means that page only and the bare domain means the whole site; each step says which it passes.
- **Keyword**: the query the page should win. Default: chosen in step 1 from what the page already ranks for.
- **Search Console**: whether the `console_*` tools are there. This job is better with them: the page's real queries, impressions and CTR. The [Google search notes](../create-seo-plan/references/platforms/google.md#search-console) say how to check.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: about 10 + 6 + 2 + 10 = 28 credits without Search Console, about 18 with it, plus 20 for the link comparison in step 5 when on-page is already level; `seo_get_page` is free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **What the page ranks for.**
   - With Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["query"]` and a `filters` entry of dimension page equal to the URL, for the last 90 days (free): clicks, impressions, `ctr` and `position` per query.
   - Without it: `seo_get_ranked_keywords` with the URL as `target` (10 credits): keyword, `rank`, `volume` and estimated `traffic`.

   Pick the target keyword: the one with the most volume whose intent the page serves. Then `seo_get_position` with that keyword and the domain as `target` (6 credits). If its `url` is another page of the site, the two pages compete: settle which one owns the keyword with [fix-keyword-cannibalization](../fix-keyword-cannibalization/SKILL.md) before changing either.
2. **Read page one.** `seo_get_serp` for the keyword with `ai_overview: true` (2 credits): the format and titles of the top three, `features`, and whether the `ai_overview` cites anyone.
3. **Check the page against the winners.** `seo_get_page` on the URL and on the top three results (free). Go element by element:
   - Indexable: `status` 200, `canonical` pointing at the page itself, no noindex in `robots_meta`. Any failure here comes first.
   - `title`: the keyword or a close variant near the start, under about 60 characters, a reason to click the others lack.
   - `meta_description`: present, under about 155 characters, answers the query.
   - `h1`: one, matching the query. `h2`: the sections the winners share that the page lacks.
   - `word_count` against the winners' median, `schema_types` against theirs, `images_without_alt`, and `links_internal`.
4. **Find the missing subtopics.** `seo_get_ranked_keywords` on the top result's URL (10 credits): the keywords it ranks for that this page does not. Each is a question or section to add.
5. **Compare links, when on-page is level.** If the page already matches the winners on intent, format and sections, the gap is usually links. `seo_get_backlink_summary` on the URL and on the top result (10 credits each): compare `referring_domains`. A large gap goes to [find-backlink-targets](../find-backlink-targets/SKILL.md), with this URL as the page that wants links.
6. **Deliver** a table: element, now, recommended, evidence (which winners do it, or which keyword asks for it), priority. Above it: the keyword, its volume, the position now and its source, so the next check has a baseline.

## Judgment

- Follow the [Google search notes](../create-seo-plan/references/platforms/google.md): [Search Console](../create-seo-plan/references/platforms/google.md#search-console) against estimates, the [keyword floors](../create-seo-plan/references/platforms/google.md#keyword-floors), the [credits](../create-seo-plan/references/platforms/google.md#credits) and the [handoff](../create-seo-plan/references/platforms/google.md#handoff).
- Keep the URL. A new URL restarts the page's history; if it must change, it gets a 301.
- Match the intent before anything else. A product page will not rank for a query whose top ten are guides, however good its title.
- A low CTR at a good position is a title and snippet problem; a low position with a good CTR is a content or links problem. Search Console shows which; without it, compare the `title` with the winners'.
- Change the title and the content in one pass, then wait. Changing again every week makes it impossible to tell what worked. Re-check in two to four weeks with [track-rankings](../track-rankings/SKILL.md).
- The tools cannot read the page's body text. If the host can open the page, it can judge the copy; otherwise the headings and word count are the evidence.

## Related skills

- A new page for a keyword: [write-seo-brief](../write-seo-brief/SKILL.md). Many pages at once: [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md), [refresh-content](../refresh-content/SKILL.md).
- Another page of the site holds the keyword: [fix-keyword-cannibalization](../fix-keyword-cannibalization/SKILL.md).
- The page needs links to beat the winners: [find-backlink-targets](../find-backlink-targets/SKILL.md).
- Whether the page is cited in Google's AI overview for its keyword: [check-ai-overviews](../check-ai-overviews/SKILL.md).
- Checking the position after the changes ship: [track-rankings](../track-rankings/SKILL.md).
