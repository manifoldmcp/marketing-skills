---
name: map-market
description: When the user wants the players in a market mapped by segment. Finds the vendors in each segment from Google results, the leaders' search rivals, the Facebook and LinkedIn ad libraries and the brands AI engines name, sizes them by traffic, and shows which segments are crowded and which have gaps. Also use when the user mentions a market map, market landscape, who are all the players in X, segment the X market, a category map with the leaders in each niche, or a landscape of the whole category. The user's own direct competitors go to find-competitors, how many companies could buy to size-market, whether demand exists to check-demand, a deep look at one rival to tear-down-competitor.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Market map

The companies that serve a market, grouped by segment: who serves which buyer, how crowded each segment is, who advertises there, which brands AI engines name for it, and where a segment has few players. It maps the vendors, not the buyers. It ends in a map table and a player table.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_serp` and `aeo_run_ai_answers` (hosts often add a prefix, for example `mcp__manifold__seo_get_serp`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- This skill reads several tool groups: `seo_*`, `ads_*`, `aeo_*` and `leads_*`. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run on the sources that remain, and name the missing sources in the deliverable, since a signal that was not checked is not a signal that is absent.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the category, the competitors, the markets and languages) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first. After delivering, offer to write the competitive landscape into `.agents/product-marketing.md` with [create-product-context](../create-product-context/SKILL.md).

## Inputs to settle first

- **Market**: the category and its edges: what is in and what is out ("construction management software", not general project management).
- **Segments**: the cut that matters to the user: buyer size (small business, mid-market, enterprise), vertical (clinics, agencies, schools), or use case. Ask which; default to three or four segments by buyer, since that is how buyers search ("<category> for <buyer>").
- **Known leaders**: one or two companies the user knows are big in the market, as seeds for step 3.
- **Market location**: `location` and `language` if not the United States and English; `country` for the ad libraries.
- **Budget**: a default run for four segments costs about 8 + 20 + 8 + 72 + 75 + 10 = 193 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Define the segments.** Write each segment as two or three keyword phrases a vendor in it would use about itself ("crm for real estate agents", "real estate crm"). These phrases drive every step below.
2. **Find the players per segment.** `seo_get_serp` for each segment's main phrase with `depth: 20` (2 credits each, 8 for four). Keep the vendors' domains; a list article in the results names more products in its `h2[]` (`seo_get_page`, free). Keep every vendor as a candidate for its segment.
3. **The leaders' search rivals.** `seo_get_serp_competitors` on the one or two known leaders (10 credits each). Keep the vendors and drop publishers, marketplaces, review sites and directories: these rows are SERP competitors, not business competitors. `organic_traffic` on each row is a first size signal.
4. **Paid players.** `ads_search_ads` with each segment's main phrase as `query` and `active_only: true` on `facebook` and `linkedin` (1 credit a page each, 8 for four segments). Only Facebook applies `active_only`; LinkedIn rows carry `active: null`, so keep those whose `last_shown` falls in the last 30 days. A company advertising into a segment now is investing in it now.
5. **The engines' picks.** `aeo_run_ai_answers` with one prompt per segment ("best <category> for <segment>"; up to 10 prompts) and `brands` holding the ten most-named candidates so far, on the default engines (18 credits per prompt, 72 for four). Call `get_task` (free) after `poll_after_s`. Count the cells that mention each brand per segment, and read each `answer` for brands not in the list. These are the names buyers hear when they ask.
6. **Place and size.** Merge into one row per company. `seo_get_traffic_estimates` on all the domains in one call (50 plus 50 per 100 domains: about 75 for 50) for `organic_traffic`. Place each company in its segments by what its homepage says: `seo_get_page` (free) for `title`, `h1[]` and `meta_description`; a company can sit in two. `leads_get_company` (1 credit each) on the ten that matter most, for `employees` and `founded_year`.
7. **Deliver** two tables. The map, one row per segment: the leaders (by AI mentions, then traffic), the number of vendors found across steps 2 to 5, the active advertisers, the approach most players take, and the gap (a segment with demand signals and few or weak players). The players, one row per company: company, domain, segments, `organic_traffic`, AI mentions (cells out of those run), advertising now (yes or no), employees and founded year where fetched.

## Judgment

- Segment by the buyer, not by the vendors' feature lists. Buyers search and ask engines by who they are; that is where a segment is won.
- The vendor count compares segments; it is not a census. It holds only vendors that rank, advertise or get named by an engine, so a long tail of small vendors is missing from every segment.
- Traffic sizes SEO-led vendors well and sales-led ones badly: an enterprise vendor can have modest traffic and most of the revenue. Where it matters, fetch the company record.
- Many companies with no clear AI pick means a fragmented segment, which is an opening. Few companies with the same names in every engine and every ad library means a concentrated one.
- An empty cell on the map is a hypothesis. Check that buyers want it with [check-demand](../check-demand/SKILL.md) before calling it an opportunity.
- Every number carries the tool that returned it. Credits: `aeo_run_ai_answers` and `seo_get_traffic_estimates` are the expensive calls, and `aeo_run_ai_answers` is never cached, so a rerun pays for it again. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back; `dry_run: true` prices any call for free.

## Related skills

- The number of companies that could buy (not sell) in a segment: [size-market](../size-market/SKILL.md).
- The user's own direct competitors, classified: [find-competitors](../find-competitors/SKILL.md). One rival in depth: [tear-down-competitor](../tear-down-competitor/SKILL.md).
- Whether buyers want what a gap offers: [check-demand](../check-demand/SKILL.md). Where to place the product on the map: [find-positioning](../find-positioning/SKILL.md).
- Choosing the segment to win first: [create-gtm-plan](../create-gtm-plan/SKILL.md).
