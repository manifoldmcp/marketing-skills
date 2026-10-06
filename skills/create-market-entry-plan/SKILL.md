---
name: create-market-entry-plan
description: When the user wants to know whether a new country or segment is worth entering, and how. Sets the target market beside the home market on search demand in the local language and in English, who wins Google there, what AI engines answer there, how many ICP companies there are, and who advertises there, ending in a scorecard with a go, wait or no-go call. Also use when the user mentions expand into Germany, is there demand for us in the UK, enter a new market, international expansion, localization, which country first, or move into the enterprise segment. A go-to-market plan for a new product goes to create-gtm-plan, a growth plan in the home market to create-growth-plan, a count of companies alone to size-market.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Market entry

Whether a new country or segment is worth entering, and how: the demand there in the local language, who already wins it, how many companies fit the ICP, and what AI engines answer there. The search and answer tools take `location` and `language`, which is what makes this work; the rest is setting the target beside the home market. It hands back a scorecard and a go, wait or no-go call.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `leads_search_companies` (hosts often add a prefix, for example `mcp__manifold__seo_search_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The steps read `seo_*`, `aeo_*`, `leads_*`, `ads_*` and `reddit_*`. If some of these tool groups are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, carry on, and mark those rows of the scorecard "not measured".

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the home market, the category keywords, the competitors and the ICP filters) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Home market**: where the user sells now. Default: United States, English.
- **Target**: a country with its language (for example `location: "Germany"` and `language: "de"`), or a segment (enterprise, a vertical, a company size).
- **Keywords**: the category in the target language. Translate, then have a native speaker check: buyers often use a different word from the literal translation, or the English one.
- **Competitors**: the user's home competitors, and any local ones the user knows.
- **ICP filters**: `industries` (LinkedIn's industry names, matched exactly) and `employee_ranges`, for the company counts.
- **Budget**: about 20 + 6 + 5 + 15 + 36 + 8 + 2 = 92 credits for one country. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Demand there.** `seo_search_keywords` with the translated seed and the target `location` and `language` (10 credits each; two seeds). Then `seo_get_keyword_metrics` with the home market's top 20 keywords, in English, at the target `location` (about 6 credits): in many B2B markets buyers search in English. Set each beside the home market's volume.
2. **Who wins there.** `seo_get_serp` for the five biggest target keywords with the target `location` and `language` (1 credit each). Mark each result: the user, a home competitor, a local competitor (a local domain, the local language), or a publisher. Then `seo_get_domain_overview` with the target `location` on the user's site and on the two strongest local competitors (5 credits each): the traffic the user already gets there, and what the local players get.
3. **What AI engines answer there.** `aeo_run_ai_answers` with two buyer questions in the target language, the target `location` and `language`, and `brands` set to the user and the competitors (18 credits a prompt), then `get_task`.
4. **Companies there (B2B).** `leads_search_companies` with the ICP filters and `locations` set to the target country, then again with the home country (4 credits each). Compare `meta.rows_available`: the ratio is the size of the new market against the one the user already sells into.
5. **Who pays to win customers there.** `ads_search_ads` with `platform: "facebook"`, the category in the target language and `country` set to the target's code (1 credit): who advertises there, since when, and in which language. An advertiser with ads still running 90 or more days after `first_shown` in that country has found paid acquisition there that pays: the strongest go signal the libraries give. `reddit_search_subreddits` with the category in the target language (1 credit): whether local communities discuss it.
6. **A segment instead of a country.** Keep the home `location` and swap the filters: `industries`, `employee_ranges` and `keywords` (the segment's own words, "law firm") on `leads_search_companies`, segment words on the search keywords ("for enterprise", "for law firms"), and the segment's own competitors. The steps are the same.
7. **Deliver** a scorecard: metric (category search demand in the local language, English search demand, ICP companies, AI mentions of the user, local competitors on page one, advertisers), home value, target value, ratio, and what it means. Add the local competitors (domain, estimated traffic there, the keywords where they rank, ads running) and a go, wait or no-go call with the reasons. For a go, name the skills to run with the target `location` and `language`, for example [create-seo-plan](../create-seo-plan/SKILL.md), [create-ai-search-plan](../create-ai-search-plan/SKILL.md) and, for B2B, [build-lead-list](../build-lead-list/SKILL.md).

## Judgment

- A smaller country has less of everything. Compare ratios (demand per ICP company, the share of page one held by local players), not raw volume.
- Local competitors on local domains holding page one is the strongest sign of a hard market: buyers there prefer local vendors, or the language is a moat. Home competitors ranking there with English pages is the opposite sign.
- Translation is not localization. If the local-language keywords show no volume and the English ones do, the market may buy in English; check that before paying for a translation.
- Company counts come from one provider, and its coverage differs by country. Compare the target with the home market from the same source and treat the ratio as rough.
- The tools cannot see what else entry needs: local payment methods, invoicing and tax, data residency, support hours, the law. List them as open questions for the user.
- One market at a time. Two countries at once halve the effort each one gets.
- Say the estimate before the first paid call; `dry_run: true` prices any call for free. The cost is per market: a second country runs the steps again.

## Related skills

- A go-to-market plan for a new product: [create-gtm-plan](../create-gtm-plan/SKILL.md).
- A growth plan in the home market: [create-growth-plan](../create-growth-plan/SKILL.md).
- The full count of companies that fit the ICP: [size-market](../size-market/SKILL.md).
- The local competitors in depth: [tear-down-competitor](../tear-down-competitor/SKILL.md).
- The channel work once the call is go: [create-seo-plan](../create-seo-plan/SKILL.md), [create-ai-search-plan](../create-ai-search-plan/SKILL.md), [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md), [build-lead-list](../build-lead-list/SKILL.md).
