# Market map

The companies that serve a market, grouped by segment: who serves which buyer, how crowded each segment is, who advertises there, which brands AI engines name for it, and where a segment has few players. It maps the vendors, not the buyers. It ends in a map table and a player table.

## Inputs to settle first

- **Market**: the category and its edges: what is in and what is out ("construction management software", not general project management).
- **Segments**: the cut that matters to the user: buyer size (small business, mid-market, enterprise), vertical (clinics, agencies, schools), or use case. Ask which; default to three or four segments by buyer, since that is how buyers search ("<category> for <buyer>").
- **Known leaders**: one or two companies the user knows are big in the market, as seeds for step 3.
- **Market location**: `location` and `language` if not the United States and English; `locations` for the company search.
- **Budget**: a default run for four segments costs about 16 + 20 + 8 + 72 + 75 = 191 credits, plus 1 for each company record fetched in step 6. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Define the segments.** Write each segment as two or three keyword phrases a vendor in it would use about itself ("crm for real estate agents", "real estate crm"). These phrases drive every step below.
2. **Count the players.** `leads_search_companies` with each segment's phrases as `keywords` (4 credits per page of 100, 16 for four segments). `rows_available` per segment shows how crowded it is. The rows give names, domains, and `industry`, `employees` and `location` when the provider holds them; keep them as candidates.
3. **Search players.** `seo_get_serp_competitors` on the one or two known leaders (10 credits each). Keep the vendors and drop publishers, marketplaces, review sites and directories: these rows are SERP competitors, not business competitors. `organic_traffic` on each row is a first size signal.
4. **Paid players.** `ads_search_ads` with each segment's main phrase as `query` and `active_only: true` on `facebook` and `linkedin` (1 credit a page each, 8 for four segments). Only Facebook applies `active_only`; on the other libraries keep the rows whose `active` is true or whose `last_shown` is recent. A company advertising into a segment now is investing in it now.
5. **The engines' picks.** `aeo_run_ai_answers` with one prompt per segment ("best <category> for <segment>"; up to 10 prompts) and `brands` holding the ten most-named candidates so far, on the default engines (18 credits per prompt, 72 for four). Call `get_task` (free) after `poll_after_s`. Count the cells that mention each brand per segment, and read each `answer` for brands not in the list. These are the names buyers hear when they ask.
6. **Place and size.** Merge into one row per company. `seo_get_traffic_estimates` on all the domains in one call (50 plus 50 per 100 domains: about 75 for 50) for `organic_traffic`. Place each company in its segments by what its homepage says: `seo_get_page` (free) for `title`, `h1[]` and `meta_description`; a company can sit in two. Fetch `leads_get_company` (1 credit) for the five to ten that matter most when the row lacks `employees`, or for its `description`. It holds no funding or revenue.
7. **Deliver** two tables. The map, one row per segment: the leaders (by AI mentions, then traffic), the count of companies (`rows_available`), the active advertisers, the approach most players take, and the gap (a segment with demand signals and few or weak players). The players, one row per company: company, domain, segments, `organic_traffic`, AI mentions (cells out of those run), advertising now (yes or no), employees where known.

## Judgment

- Segment by the buyer, not by the vendors' feature lists. Buyers search and ask engines by who they are; that is where a segment is won.
- `rows_available` compares segments; it is not a census ([router](../SKILL.md#signals-and-their-limits)). Its loose keyword match also counts agencies and consultants that only mention the words, so a service-heavy segment looks more crowded than it is.
- Traffic sizes SEO-led vendors well and sales-led ones badly: an enterprise vendor can have modest traffic and most of the revenue. Where it matters, compare headcount (`employees`) instead.
- Many companies with no clear AI pick means a fragmented segment, which is an opening. Few companies with the same names in every engine and every ad library means a concentrated one.
- An empty cell on the map is a hypothesis. Check that buyers want it with [demand check](demand-check.md) before calling it an opportunity.
- The number of companies that could buy (not sell) in a segment is [leads market size](../../leads/references/market-size.md). The user's own direct competitors, classified, are [competitors find competitors](../../competitors/references/find-competitors.md).
