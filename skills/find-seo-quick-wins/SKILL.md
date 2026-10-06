---
name: find-seo-quick-wins
description: When the user wants fast SEO wins from keywords the site already ranks 4 to 20 for on Google. Finds striking-distance keywords from the user's Google Search Console or DataForSEO estimates, keeps the ones with volume and intent that matter, compares each page with what ranks above it, and hands back a per-page list of title, meta description, section and internal link fixes. Also use when the user mentions SEO quick wins, low-hanging fruit, striking distance keywords, almost on page one, keywords ranking 5 to 15 that could reach the top 3, which title tags and meta descriptions to fix, or what can we fix this week. Pages that decayed or need a rewrite go to refresh-content, one URL in depth to optimize-page, two of the site's pages on one keyword to fix-keyword-cannibalization, the full SEO plan to create-seo-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# SEO quick wins

The cheapest organic traffic is on keywords the site already ranks 4 to 20 for: Google already thinks the page is relevant, and a better title, a missing section or an internal link can move it up. This skill finds those keywords, keeps the ones with volume and intent that matter, and ends in a fix list per page.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_ranked_keywords` and `seo_get_serp` (hosts often add a prefix, for example `mcp__manifold__seo_get_ranked_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Then follow the tool checks in the [Google search notes](../create-seo-plan/references/platforms/google.md#tools): the `seo_*` tools are required, the `console_*` tools optional.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the Search Console property, the market, the money pages, customer language for titles) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Site**: the user's domain, or one section of it.
- **Search Console**: whether the `console_*` tools are there. This job is much better with them, because impressions show demand that the estimates miss. The [Google search notes](../create-seo-plan/references/platforms/google.md#search-console) say how to check.
- **Focus**: money pages only, the blog only, or everything. Default: everything, money pages first.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: about 30 + 10 + 5 = 45 credits without Search Console, about 15 with it; `seo_get_page` is free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the keywords.**
   - With Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["query", "page"]`, the last 90 days as `start_date` and `end_date`, and `limit: 1000` (free). Keep rows with `position` from 4 to 20 and at least about 100 impressions in the window. Also keep rows in positions 1 to 3 whose `ctr` is under about half of what the others in that position earn on the site: that is a title problem, not a rank problem.
   - Without it: `seo_get_ranked_keywords` on the domain with `limit: 500` (30 credits). Keep rows with `rank` from 4 to 20.
2. **Filter.** Apply the [keyword floors](../create-seo-plan/references/platforms/google.md#keyword-floors): volume, intent and the site's reach. Drop other brands' navigational keywords and keywords where the ranking `url` cannot serve the intent (a blog post ranking for a "pricing" query needs a different page, not a fix). Rank what is left: positions 4 to 10 first (moving from 8 to 4 roughly doubles the clicks), then 11 to 20, then by volume and intent.
3. **Group by page.** One page usually carries several of these keywords. Fix per page, and read every keyword the page ranks for before changing its title, so a rewrite for one keyword does not cost another that already brings clicks. Where two URLs of the site rank for the same keyword, flag it and leave it out: the fix is to pick one page, not to tune both, and that is [fix-keyword-cannibalization](../fix-keyword-cannibalization/SKILL.md).
4. **See what beats it.** `seo_get_serp` for the main keyword of the top 10 pages (1 credit each). Note the format of the top three, the titles that win, and `features` (`featured_snippet`, `people_also_ask`, `video`, `ai_overview`). If the top three are a different format from the user's page, a title fix will not do: move the page to the [refresh-content](../refresh-content/SKILL.md) list.
5. **Read the pages.** `seo_get_page` on each page (free, rate limited). Check that `status` is 200, `canonical` points at the page itself and `robots_meta` does not say noindex. Then the fixes: the keyword or a close variant near the start of `title`, a title under about 60 characters, a `meta_description` that answers the query in under about 155 characters, one `h1`, and `h2` headings for the subtopics the winners cover.
6. **Add internal links.** `seo_get_domain_overview` on the site (5 credits): its `top_pages` are the strongest pages. Suggest one or two links from a related top page to each fixed page, with the keyword as anchor text. The tools cannot see which pages already link to it; the user checks before adding.
7. **Deliver** a table, one row per page: URL, main keyword, other keywords in range, volume, current position (say whether Search Console or estimate), intent, what ranks above (format), the fix (new title, new meta description, a missing section, an internal link from which page), and effort (minutes or a rewrite).

## Judgment

- Follow the [Google search notes](../create-seo-plan/references/platforms/google.md): [Search Console](../create-seo-plan/references/platforms/google.md#search-console) against estimates, the [keyword floors](../create-seo-plan/references/platforms/google.md#keyword-floors), the [credits](../create-seo-plan/references/platforms/google.md#credits) and the [handoff](../create-seo-plan/references/platforms/google.md#handoff).
- Search Console's `position` is an average over every impression, location and device. A 9.6 can be 4 on desktop and 15 on mobile; check the keywords that matter with `seo_get_position` if the fix depends on it.
- Few impressions make noise. A keyword with 30 impressions in 90 days at position 6 is not a win yet.
- Google rewrites titles it finds unhelpful. A title that matches the page's `h1` and the query is rewritten less often.
- Title and meta fixes move clicks in days to weeks; section additions and links take weeks to months. Re-check after two to four weeks with [track-rankings](../track-rankings/SKILL.md), once or on a schedule.
- A keyword at 11 to 20 that needs a whole new section is a refresh, not a quick win. Keep this list to changes of an hour or less per page.
- If every candidate sits on one page type that has a technical problem (noindex, a wrong canonical, a slow template), the win is the [audit-technical-seo](../audit-technical-seo/SKILL.md), not the titles.

## Related skills

- Pages that decayed or need new sections: [refresh-content](../refresh-content/SKILL.md). One URL in depth: [optimize-page](../optimize-page/SKILL.md).
- Two of the site's pages on one keyword: [fix-keyword-cannibalization](../fix-keyword-cannibalization/SKILL.md). A technical problem under the candidates: [audit-technical-seo](../audit-technical-seo/SKILL.md).
- Where quick wins sit in a quarter's SEO work: [create-seo-plan](../create-seo-plan/SKILL.md).
- Checking the positions after the fixes ship: [track-rankings](../track-rankings/SKILL.md).
