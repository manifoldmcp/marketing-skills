---
name: check-ai-overviews
description: When the user wants to know which of the keywords their site already ranks for show a Google AI overview, and whether it cites them. Takes the site's top non-branded keywords from Search Console or ranking estimates, checks each results page for an AI overview and its cited sources, sizes the clicks at risk, and compares the user's page with the pages cited instead. Also use when the user mentions which of our keywords have an AI overview, are we cited in Google's AI overviews for our keywords, how much of our traffic is exposed to AI Overviews, or an AI overview audit for our rankings. Buyer prompts across ChatGPT, Perplexity and other engines go to check-ai-visibility, a sudden fall in clicks to diagnose-traffic-drop, and weekly positions to track-rankings.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# AI overview exposure

Which of the keywords the site already ranks for show a Google AI overview, whether the site is among its cited sources, and how many clicks sit under an answer the site is not part of. It starts from the site's own Google keywords, not from buyer prompts: [check-ai-visibility](../check-ai-visibility/SKILL.md) covers prompts across every engine. It hands back a keyword table with the action for each, and the clicks at risk.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_serp` and `seo_get_ranked_keywords` (hosts often add a prefix, for example `mcp__manifold__seo_get_serp`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The `seo_*` tools are required and the `console_*` tools optional, as the [Google search notes](../create-seo-plan/references/platforms/google.md#tools) say.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the brand name, the Search Console property, the keywords in the tracking set, the markets) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Site**: the user's domain, and the brand name to leave branded queries out.
- **Search Console**: whether the `console_*` tools are there. This job is much better with them: real clicks and CTR show what an overview costs; the estimates only show the traffic exposed. The [Google search notes](../create-seo-plan/references/platforms/google.md#search-console) say how to check.
- **Keywords**: default the site's top 50 non-branded keywords that rank in the top 20. The user can give a list instead.
- **Market**: `location`, `language` and `device`. Overviews differ by device; pass `device: "mobile"` when most of the site's clicks are mobile (Search Console with `dimensions: ["device"]` says).
- **Budget**: about 50 x 2 + 14 = 114 credits with Search Console, about 124 without it. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the keywords.**
   - With Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["query"]` for the last 90 days, `limit: 1000` and one `filters` entry on query with `operator: "excludingRegex"` and the brand name (free). Keep the 50 queries with the most clicks whose average `position` is 20 or better.
   - Without it: `seo_get_ranked_keywords` on the domain (10 credits). Drop branded keywords and keep the 50 with the most `traffic` whose `rank` is 20 or better.
2. **Read each page one.** `seo_get_serp` with `ai_overview: true` for each keyword (2 credits each). Per keyword: whether `ai_overview` is there, whether the site's domain is among `ai_overview.references[]`, the site's own `rank`, and the domains cited instead.
3. **Size it.** `seo_get_keyword_metrics` on the 50 keywords with `ai_volume: true` (14 credits): `volume` and `ai_volume`, how often AI engines get the same question. With Search Console, compare the site's `ctr` on keywords with an overview against keywords without one in the same position band (1 to 3, 4 to 10): the difference times the impressions is the clicks the overview takes. Without it, add up the estimated `traffic` on keywords with an overview the site is not cited in: that is the traffic exposed, not a measured loss.
4. **Look at the gaps that can close.** For keywords where an overview shows, the site ranks in the top 10 and is not cited: `seo_get_page` on the user's ranking page and on the first three cited references (free). Compare `h2` and `schema_types`, and check the `ai_overview` text against the page: an overview quotes a short, direct answer, and a page whose answer sits under a vague heading deep in the page is passed over.
5. **Deliver** a table: keyword, clicks and impressions or estimated traffic (say which), `volume`, `ai_volume`, the site's rank, overview (yes, no), site cited (yes, no), the domains cited, CTR against its band, and the action: held (cited), answer on the page (top 10, not cited: [optimize-page](../optimize-page/SKILL.md)), rank first (outside the top 10: [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md)), or none (no overview). Above it: the share of the keywords with an overview, the share where the site is cited, and the clicks at risk.

## Judgment

- Search Console counts AI overview and AI Mode clicks and impressions inside `web` search, with no filter to separate them. The CTR comparison in step 3 is the measurement there is; say it is an inference.
- Overview citations follow Google's rankings closely, so a page outside the top 20 is rarely cited. Rank first, then shape the answer.
- An overview shows for one query on one day and not the next. One SERP is a sample: re-check the keywords that matter before telling the user they lost the overview.
- Clicks taken by an overview do not come back by ranking higher. Where the site is cited, measure impressions and position as well as clicks; where it is not, the citation is the prize.
- The server keeps no history. Put the date, market and device on the table so a later run can compare; weekly positions belong to [track-rankings](../track-rankings/SKILL.md).

## Related skills

- Buyer prompts on every AI engine: [check-ai-visibility](../check-ai-visibility/SKILL.md). The plan for AI search: [create-ai-search-plan](../create-ai-search-plan/SKILL.md).
- The page work behind each action: [optimize-page](../optimize-page/SKILL.md), [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md).
- A fall in clicks to find the cause of: [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md). Clicks watched over time: [monitor-search-console](../monitor-search-console/SKILL.md).
