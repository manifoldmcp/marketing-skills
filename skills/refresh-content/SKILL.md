---
name: refresh-content
description: When the user wants to find and refresh old pages that lost Google traffic or sit on page two. Finds decayed pages from the user's Google Search Console clicks year over year (or DataForSEO ranking gaps), reads what outranks each one and the subtopics it misses, and hands back a refresh list that says what to add to each page, or whether to merge or retire it. Also use when the user mentions which old posts should we update, content decay, refresh our blog, pages that used to rank, articles stuck on page two, prune old content, or which old guides to rewrite and which to merge. Title and meta fixes on pages ranking 4 to 10 go to find-seo-quick-wins, one URL in depth to optimize-page, a whole new page to write-seo-brief, a fall across the whole site to diagnose-traffic-drop, two pages on one keyword to fix-keyword-cannibalization.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Content refresh

Pages the site already has are cheaper to lift than new ones: they are indexed, they have links, and Google already ties them to a topic. This skill finds the pages that decayed or sit on page two, sees what outranks each one, and ends in a refresh list that says what to add to each page, or whether to merge or retire it.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_ranked_keywords` and `seo_get_serp` (hosts often add a prefix, for example `mcp__manifold__seo_get_ranked_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Then follow the tool checks in the [Google search notes](../create-seo-plan/references/platforms/google.md#tools): the `seo_*` tools are required, the `console_*` tools optional.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the Search Console property, the market, what the product does, so pages with no fit can be retired) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Site**: the user's domain, or the section to refresh (the blog, the guides).
- **Search Console**: whether the `console_*` tools are there. This job is much better with them, because only clicks over time show decay; the estimates show where pages rank now. The [Google search notes](../create-seo-plan/references/platforms/google.md#search-console) say how to check.
- **Capacity**: pages the team can refresh a month. Default: four, so the list stops at the ten best.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: about 30 + 10 x (1 + 10) = 140 credits for ten pages without Search Console, about 110 with it, plus 12 for each page you plan to merge or retire; `seo_get_page` is free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the candidates.**
   - With Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["page"]` and `limit: 1000` for the last three months and for the same three months a year earlier (two calls, free). Keep pages that lost 30 percent or more of their clicks from a base of at least about 100 clicks. Add pages whose average `position` is 11 to 30 with high impressions: page two and three with demand. Positions 4 to 10 that need only a title or a link are [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md).
   - Without it: `seo_get_ranked_keywords` on the domain with `limit: 500` (30 credits), filtered to the section's URLs. Per `url`, add up the `volume` of keywords ranking 11 to 30 and the estimated `traffic` it earns. Pages with the biggest gap between the two are under-performing on topics Google already gives them. Ask the user for pages their analytics shows falling; the estimates cannot see decay over time.
2. **Pick each page's main keyword.** The keyword with the most volume that the page's topic answers. For the ten best candidates, keep the page's other ranking keywords as well: a refresh must not lose them.
3. **See what outranks it.** `seo_get_serp` for each main keyword (1 credit each). Note the top three: their format, their titles (a year in a title signals freshness wins here), and `features`. If the winners are a different format from the page, the refresh is a rewrite into that format.
4. **Compare the pages.** `seo_get_page` on the user's page and the top three results (free, rate limited). Headings in `h2` that two or more winners share and the page lacks are the sections to add. Compare `word_count` with the winners' median, `schema_types` (FAQPage, HowTo, Article, Product), and whether the `title` still matches the query.
5. **Find the missing subtopics.** `seo_get_ranked_keywords` on the top result's URL (10 credits each, the URL as `target`). Keywords it ranks for that the user's page does not are the questions and subtopics Google rewards for this topic.
6. **Decide per page.** Refresh when the page matches the intent and lacks sections. Merge when two pages of the site split one keyword: keep the stronger URL, fold the other in and 301 it ([fix-keyword-cannibalization](../fix-keyword-cannibalization/SKILL.md) decides which). Retire when a page has no rankings, no links and no fit with the product. Before retiring or merging, check links with `seo_get_backlinks` on the URL (12 credits): a page with links is redirected, never deleted.
7. **Deliver** a table: URL, main keyword, volume, position now, clicks lost or traffic gap (say which source), the top result and its format, sections to add (from the headings and keywords), other fixes (title, schema, a stale year), action (refresh, merge into which URL, retire), priority.

## Judgment

- Follow the [Google search notes](../create-seo-plan/references/platforms/google.md): [Search Console](../create-seo-plan/references/platforms/google.md#search-console) against estimates, the [keyword floors](../create-seo-plan/references/platforms/google.md#keyword-floors), the [credits](../create-seo-plan/references/platforms/google.md#credits) and the [handoff](../create-seo-plan/references/platforms/google.md#handoff).
- A refresh keeps the URL. Changing it throws away the page's history and links; if it must change, it is a 301 and a line in the table.
- Update the visible date only when the content changed. Google and readers both notice a new date on an old page.
- Word count is a symptom. Add the sections the winners cover; do not pad to their length.
- If demand fell (`trend[12]` from `seo_get_keyword_metrics`, 10 credits for up to 100 keywords), the page did not decay, the topic did. Refreshing it will not bring the traffic back.
- The tools cannot read the body text or the publish date. If the host can open the page, it can find stale facts, dead screenshots and old prices; otherwise the user checks them.
- Refresh the pages closest to page one first. A page at 12 moves with a new section; a page at 45 usually needs a rewrite, which is a [write-seo-brief](../write-seo-brief/SKILL.md) for its keyword.

## Related skills

- Pages at 4 to 10 that need only a title or a link: [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md). One URL in depth: [optimize-page](../optimize-page/SKILL.md).
- Two pages splitting one keyword: [fix-keyword-cannibalization](../fix-keyword-cannibalization/SKILL.md). A page that needs a full rewrite: [write-seo-brief](../write-seo-brief/SKILL.md).
- A fall across the whole site, or one with a date: [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md).
- Where refreshes sit in a quarter's SEO work: [create-seo-plan](../create-seo-plan/SKILL.md).
