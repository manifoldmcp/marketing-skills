# Comparison pages

People who search "<competitor> alternatives" or "<competitor> vs <other>" are close to buying and already know the category. This playbook finds which of those searches have volume, which comparison pages the user already has and where they rank, and what competitors publish for the same searches, including pages about the user. It ends in a list of the user's own "X vs Y" and "X alternatives" pages to create or improve.

## Inputs to settle first

- **Site and product**: the user's domain and the product's name as people search it.
- **Competitors**: the products buyers compare the user with. Ask; `seo_get_serp_competitors` returns sites that share search results, which are often publishers rather than rivals. The `competitors` group's [find competitors](../../competitors/references/find-competitors.md) playbook answers it if the user does not know.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run for three competitors costs about 3 x 2 x 10 + 10 + 30 + 10 = 110 credits; `seo_get_page` is free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the searches.** For each competitor, `seo_search_keywords` twice (10 credits each): `seed: "<competitor> alternative"` and `seed: "<competitor> vs"`. The default mode keeps phrases that contain the seed, which catches "alternatives", "free alternative", "alternative for <use case>" and every "vs" pair people search. Then `seo_get_keyword_metrics` (10 credits for up to 100 phrases) on the phrases with the user's own name: "<product> vs <competitor>", "<product> alternatives", "<product> reviews". Small brands often get null or tiny volume there; that is normal.
2. **See what the site has.** `seo_get_ranked_keywords` on the site with `limit: 500` (30 credits). Keep the rows whose keyword holds a competitor's name, "vs", "alternative" or "compare". They show which comparison pages exist and rank, at which `rank`, and which `url`. Ask the user for comparison pages that exist but rank for nothing, and read them with `seo_get_page` (free).
3. **See who ranks.** `seo_get_serp` for the ten phrases with the most volume (1 credit each). Sort each page one into: vendors' own comparison pages, third-party lists and review sites (G2, Capterra, blogs), forum threads (`type: "discussions_and_forums_element"`), and videos. A vendor page in the top five proves a vendor page can rank for that phrase. Page ones held only by lists and review sites are harder for the user's own page, and those lists are worth getting into instead: that is the `link-building` group's [best-of lists](../../link-building/references/best-of-lists.md).
4. **Read what competitors publish.** `seo_get_page` on each vendor comparison page in the results (free, rate limited): `title`, `h1`, `h2` (the criteria they compare on: price, features, migration, support, who it is for), `word_count` and `schema_types`. Look hardest at the SERPs for the user's own name: a competitor's "<product> alternatives" page ranking there is taking the user's buyers and needs an answer first.
5. **Get the facts right.** A comparison page stands on current prices, plans and features for each competitor. No tool here reads a page's body text; take those facts from the `competitors` group's [messaging and pricing](../../competitors/references/messaging-pricing.md) playbook, or from the user. Never guess a competitor's price.
6. **Deliver** a table: phrase, volume, KD, page type (vs page, alternatives page, or a defence page for the user's own name), the user's URL and rank now (or none), who holds the top three (vendor, list, review site, forum), the competitor pages to match (URL and the criteria in their headings), action (create, improve, leave), priority.

## Judgment

- Start with the phrases where a vendor page already ranks and the user has no page. They are proven winnable.
- "<competitor> alternatives" pages rank best as honest lists that include other tools, not a single pitch. A "vs" page can be a straight two-way comparison.
- Be fair and dated. A wrong claim about a rival's price is the first thing a buyer checks, and comparative claims carry legal risk in some markets. Put "as of <month>" on prices and a source for each claim.
- One page per competitor pair or per "alternatives" phrase. Do not build separate pages for "alternative" and "alternatives".
- A competitor's comparison page cited in AI answers is the `ai-search` group's concern; this playbook is about Google's page one.
