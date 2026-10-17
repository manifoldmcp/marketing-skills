---
name: market-research
description: When the user wants to size a market, map the players in a category, check whether there is demand for an idea, or decide whether to enter a new country, language or segment. Holds dated, sourced findings a model gets wrong (search volume is a bucketed 12-month mean, Trends disagrees with itself, AI search volume is a model, stated interest overstates paying, other countries use other search engines) and how to count accounts, demand and players with live data. Also use when the user mentions TAM, SAM, market size, how many companies fit our ICP, is the market big enough, a market map or landscape, who are all the players in X, segment the market, idea validation, is there demand for X, is anyone searching for X, is this market growing or dying, should we build X, a fake door or smoke test, expand into Germany, which country first, international expansion, localization, or moving into enterprise. Named rivals go to competitor-intel, pain points to voice-of-customer, a go-to-market plan to growth-strategy.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Market research

How big a market is, who serves it, whether people want what the user plans, and whether a new country or segment is worth entering, as the evidence stood in October 2026. A model is likely to treat search volume as a count, Google Trends as stable, "AI search volume" as a measurement of what people ask ChatGPT, a waitlist as proof of demand, and Google as the search engine everywhere. None of those hold. Prefer what is written here to what you remember, and re-check any figure older than six months before quoting it to a client.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the product, category, ICP, buyer titles, competitors, home market and price points from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Counting accounts, pricing keywords and reading answers live needs the Manifold connector: look for `seo_search_keywords`, `seo_get_keyword_metrics`, `leads_search_companies` and `aeo_search_prompts` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill and use the by-hand method at the end of each reference, and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Search volume is a bucket and a 12-month mean, not a count.** Keyword Planner reports about 60 to 80 fixed values, groups close variants under one figure (so summing variants double-counts) and shows low-spend accounts only ranges. Ahrefs found 54% of Keyword Planner figures more than 50% above Search Console impressions (72,635 keywords, 2021, vendor study). Manifold's default Labs volume is the same buckets, a month behind Keyword Planner ([demand signals](references/demand-signals.md)).
2. **Some head terms jumped in 2026, in Google's own data.** In our October 2026 pull, generic English software terms ("crm software", "email marketing software") rose 4x to 100x between March and May 2026 and again in September, in the US and in Germany, while long-tail and German terms did not: "email marketing software" shows 135,000 a month against a median month of about 38,000. Read `trend[12]` and size from the median month ([field notes](references/field-notes.md)).
3. **Google Trends is a sample, rescaled on every request.** The same query on another day or from another country returns different values, worst for low volumes and for windows of eight months or less (Behnen et al., 2020; Hölzl, Keusch and Collins, April 2025). The consistently scaled Trends API (July 2025) is still an alpha by application in October 2026.
4. **"AI search volume" is a model, not a measurement.** Only Microsoft shares assistant query data (Bing Webmaster Tools' AI Performance, February 2026, and only for queries citing the site). DataForSEO's figure, behind Manifold's `ai_volume`, comes from Google's People Also Ask; Ahrefs' (August 2026) multiplies Google volume by referral-traffic ratios; panels extrapolate. In our pull `ai_volume` ran from 0.07% to 77% of Google volume across twenty keywords. Rank prompts with it; never size with it.
5. **Google is still the bigger demand signal.** About 14 billion searches a day (Google, March 2025) against about 2.5 billion ChatGPT prompts (OpenAI, July 2025), a quarter of them seeking information (24% in July 2025; NBER, September 2025); AI platforms sent about 0.3% of web visits in 2026 (SE Ranking). Search undercounts categories buyers research in assistants, and nothing yet measures how much.
6. **People overstate what they would pay.** Hypothetical willingness to pay ran about 21% above real across 77 studies (Schmidt and Bijmolt, 2020), more for expensive and specialty products, and conjoint overstated more than direct questions. Only money (a preorder, a deposit, a paid pilot) settles demand ([validation](references/validation.md)).
7. **Fake-door pages run into ad policy.** Google Ads disapproves destinations that say they provide no service ("coming soon"); Meta requires the page to reflect what the ad offers. The test page must describe a real offer honestly. Separating a 2% from a 4% conversion rate takes about 1,100 visitors per variant.
8. **Bottom-up counts beat top-down shares, but check precision.** A filter for dental practices in Germany returned 2,277 companies, of which at most 3 in a sample of 25 were practices. The account index is thin on local and small businesses and outside English-speaking tech; there, public business statistics size the market ([sizing](references/sizing.md)).
9. **Google is not the search engine everywhere.** September 2026, StatCounter: Yandex 70% in Russia, Baidu 47% in China (Google 1.5%), Naver 43% in South Korea (63% for 2025 on the Korean InternetTrend panel), Seznam 15% in Czechia. Yahoo! Japan serves Google's results. OpenAI does not serve China, Hong Kong or Russia. Manifold's search data is Google's, so in Russia, China and Korea it covers half the searches or fewer ([market entry](references/market-entry.md)).
10. **Buyers abroad often search in English or loanwords.** In Germany "crm system" had 14,800 searches a month against 90 for "kundenverwaltung software". Consumers still buy in their own language: 76% prefer it and 40% never buy from sites in other languages (CSA Research, 8,709 consumers in 29 countries, 2020).

**Does not hold up:** "TAM is the analyst figure times 1%"; "add up the volumes of the variants"; "100 on Trends means high demand"; "AI search volume shows how many people ask ChatGPT"; "a long waitlist proves demand"; "no search volume means no market" (a new category has no agreed name yet: Keyword Planner had no data for "generative engine optimization", which Labs put at 4,400); "translate the keywords and you have the market".

**Unknown:** what caused the 2026 jumps on English software head terms; how any vendor's AI search volume compares with real prompt logs (no published validation); how much B2B buyers abroad prefer their own language (CSA's B2B figures could not be checked); when the Trends API opens.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| How big is the market? TAM, SAM, how many companies or buyers fit our ICP? | [Sizing](references/sizing.md) |
| Is there demand for X? Is it growing? Can we trust search volume, Trends or AI search volume? | [Demand signals](references/demand-signals.md) |
| How do we prove people will pay? A fake door, smoke test, waitlist or preorder | [Validation](references/validation.md) |
| Who are all the players? Which segments are crowded, which have gaps? | [Market map](references/market-map.md) |
| Should we enter Germany, Korea or enterprise? Which country first? Localise or not? | [Market entry](references/market-entry.md) |
| What has Manifold seen in its own pulls? | [Field notes](references/field-notes.md) |

## Using Manifold

- **Search demand:** `seo_search_keywords` (10 credits for 100 rows) finds keywords around a seed; its default mode keeps only keywords containing the seed, so use `mode: "related"` for a problem phrase. `seo_get_keyword_metrics` (10 credits per 100 keywords) prices keywords the user has; never both on the same keywords. `source: "google_ads"` (24 flat) is Keyword Planner's own figure, a month fresher than Labs and with no `kd` or `intent`. `ai_volume: true` adds 4 plus 4 per 100 keywords.
- **AI prompts:** `aeo_search_prompts` (47 credits for 50 rows) shows recommendation prompts and their `cited_domains[]`; `ai_search_volume` is the same People Also Ask proxy. `aeo_run_ai_answers` (27 credits a prompt on five engines) shows who the engines name.
- **Accounts and buyers:** `leads_search_companies` and `leads_search_people` cost 12 credits per 100 rows asked for (`limit`), rounded up, so `limit: 1` is 1; read `rows_available` to count instead of fetching, and measure precision on the rows. `leads_get_company` (1 credit) shows the exact `industry` string to filter on.
- **Players and advertisers:** `seo_get_serp`, `seo_get_serp_competitors`, `seo_get_traffic_estimates` (about 75 credits for 50 domains) and `ads_search_ads` (1 credit a page, by `country`).
- **Read the data right:** `null` is no data, never zero; volume under about 50 is noise; the `seo_*` tools read Google only, wherever `location` points. English keywords abroad take the country's own `language` code. Typical runs: a demand check about 75 credits, six sizing segments about 146, a four-segment map about 225, one country's entry scorecard about 90. Pass `max_credits` when the user names a budget; `dry_run: true` prices any call free.
- **Never run tests, launch ads or contact anyone on the user's behalf.** Design the test; the user runs it.

## Related skills

- Named competitors, teardowns and battlecards: [competitor-intel](../competitor-intel/SKILL.md). Pain points, objections, personas and positioning in buyers' words: [voice-of-customer](../voice-of-customer/SKILL.md).
- A go-to-market, launch or growth plan once the market is chosen: [growth-strategy](../growth-strategy/SKILL.md).
- The accounts and buyers themselves, with emails: [outbound](../outbound/SKILL.md). Keyword research for content, and Search Console: [seo](../seo/SKILL.md).
- Whether AI engines name the brand: [ai-search](../ai-search/SKILL.md). Reading ad libraries: [paid-ads](../paid-ads/SKILL.md). Reddit communities: [reddit](../reddit/SKILL.md).
- Demand on Amazon rather than Google: [amazon](../amazon/SKILL.md). Counting a category of local businesses in a town: [local-seo](../local-seo/SKILL.md). Sizing an app category from the stores: [app-store](../app-store/SKILL.md).
