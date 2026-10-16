# Keyword research and demand

Which searches a listing should be built around, how many people make them, and how far to trust the number. Checked October 2026.

## Where Amazon keyword numbers come from

- **Amazon's own data, brand owners only.** Brand Analytics needs Brand Registry. Search Query Performance gives, per search term, the search query volume (how often it was typed in the period) and the funnel for the brand's ASINs against the market: impressions, clicks, cart adds and purchases, with the brand's share of each. Top Search Terms gives a search frequency rank and the top clicked ASINs with click and conversion share, never a volume. Product Opportunity Explorer gives niche-level searches over 90 and 360 days. These are the only first-party counts. Ask the user for an export when they have it; nothing in Manifold reads them.
- **Everything else is a model.** Helium 10, Jungle Scout, DataForSEO (behind Manifold) and the rest estimate monthly Amazon searches by undisclosed methods. DataForSEO's docs call its figures "approximate" and say nothing about inputs or refresh.
- **No independent accuracy test exists.** The most-cited comparison is Helium 10's own (August 2024): it scored itself 93.5% against Brand Analytics and Jungle Scout 41.9% against Search Query Performance, two different yardsticks. Brand Analytics deduplicates repeat searches by one shopper and Search Query Performance does not, so a tool can look accurate or wrong depending on which it is checked against. Treat a published accuracy figure for Amazon volume as marketing.
- **Use them to rank, not to count.** A modelled volume is good for "which of these 40 terms matter most" and for trends inside one tool. It is not good for "how many units will we sell". Never mix volumes from two tools in one table, and never compare an Amazon volume with a Google volume as if the units matched, even from one vendor: DataForSEO's Amazon figures ran at a median 0.43 of its Google figures on 15 product terms, from 0.04 to 1.19 ([field notes](field-notes.md)). Low Amazon volumes come in steps of about 37, so 37 against 74 is no real difference. Google keyword data and its pitfalls are in [seo](../../seo/SKILL.md).

## Building the list

1. **Seeds from the product, not the brand:** the product type, the use case, the audience, the problem ("standing desk", "desk for small spaces", "desk converter"). `amazon_search_keywords` (5 plus 5 per 100 rows returned) expands each seed from Amazon's related-searches box, two steps out, with volume. Expect a short list (24 rows for "standing desk") that drifts into neighbouring categories (desk, chair, treadmill); drop what is not the product. `related_searches[]` on `amazon_search_products` gives the same box free with the page.
2. **What already sells:** pick the two or three organic leaders for the main term from `amazon_search_products` and run `amazon_get_keywords` on each. Its rows come biggest search first, so the default 100 hold a leader's head terms; ask for 300 rows (20 credits) to reach the mid-tail, and drop zeros, other languages and other retailers' names. Their ranked terms are proven demand; their sponsored terms show what they pay for. Terms two leaders share are the core.
3. **Volume on the merged list:** `amazon_get_keyword_metrics` (5 + 5 per 100 keywords) in one call. A null is no data, not zero; long-tail and new phrasing often come back null and can still convert well.
4. **Check intent on the page:** for each core term, `amazon_search_products` (2 credits) shows whether the page is the user's kind of product at the user's price. A term whose page is all a different product type or a much cheaper tier is the wrong term however big its volume.
5. **Sort into three groups:** title and Item Highlights (the two or three terms that define the product), bullets and attributes (use cases, audiences, features, compatibility), backend search terms (synonyms, spellings and other languages that would read badly on the page). The rules for each field are in [listing audit](listing-audit.md).

Report terms with volume, who ranks, the price band on the page, and whether the user ranks now (from the results page, not the index's `rank`). Typical cost: about 95 credits for two seeds, three leaders at 300 rows each, a 200-term metrics call and a few results pages; more with 1,000-row pulls.

## Sizing demand

- **Amazon volume is one input.** Sum the volumes of the core terms (not every related term, which double-counts the same shoppers) for a monthly search figure, and say it is modelled.
- **From searches to units needs assumptions the user must own.** Searches times the share of clicks a listing at that rank gets times conversion. Amazon publishes neither curve. With Brand Analytics, use the user's own click and purchase shares on the term; without it, show a low and high case and label them as assumptions.
- **The results page gives a floor on what sells.** `bought_past_month` on page-one rows is Amazon's "N+ bought in past month" badge: a bucketed lower bound (50+, 100+, 1K+ and so on), counted in customers rather than units, and shown only above a sales threshold. Summing the floors across page one gives "at least this many buyers a month bought these products", which is often more persuasive than a volume and honest about being a floor.
- **Seasonality:** a monthly volume averages the year, and Manifold's Amazon volume has no monthly trend; the Google trend from [seo](../../seo/SKILL.md) is the nearest proxy for the shape. Amazon search spikes around Prime Day (23 to 26 June in 2026), the October deals event and Q4. Ask when the product sells.
- Market sizing beyond Amazon, Google Trends and demand validation belong to [market-research](../../market-research/SKILL.md).

## Without the connector

Type the seed into Amazon's search bar on the right marketplace in a private window and record the autocomplete suggestions; open the top organic listings and note their titles and badges; read related searches at the bottom of the page. That gives terms and a rough order, no volumes. If the user has Brand Analytics, its Search Query Performance export is better data than any tool.
