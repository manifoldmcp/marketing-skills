# Find competitors

Who the user really competes with: the direct competitors buyers compare them with, the indirect ones that do the same job another way, and the search-only sites that take their traffic without selling to their buyers. Five sources vote; a company named by several of them is a real competitor. It ends in a classified table.

## Inputs to settle first

- **Product**: the user's domain, and one line on what it does and for whom.
- **Category words**: two or three phrases a buyer uses ("scheduling software for clinics", "patient booking app"). Default: from the homepage title and `h1` in step 1.
- **Known competitors**: the ones the user already names, so the table shows who is new.
- **Market**: `location` and `language` if not the United States and English; `country` for the ad libraries.
- **Budget**: a default run costs about 10 + 2 + 54 + 10 + 3 + 6 + 60 = 145 credits, plus 10 for each company record fetched for an unclear candidate. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Describe the product.** `seo_get_page` on the user's homepage (free): `title`, `meta_description` and `h1[]` give the category words when the user has none.
2. **Search.** `seo_get_serp_competitors` on the user's domain (10 credits for 100 rows). Apply the [real competitors](../SKILL.md#real-competitors) rule: mark publishers, marketplaces, review sites, directories, forums and video sites search-only (keep the five with the most `shared_keywords`, drop the rest), then call `seo_get_page` (free) on each remaining homepage and read `title` and `meta_description`. The same job for the same buyer makes a direct candidate; anything else is search-only. Then `seo_get_serp` for "<category> alternatives" and "best <category>" (1 credit each): the vendors on page one are candidates, and a list article's `h2[]` (`seo_get_page`, free) names the products it ranks.
3. **AI answers.** `aeo_run_ai_answers` with three prompts ("best <category>", "best <category> for <the buyer>", "alternatives to <best-known competitor>") and `brands` holding the user and the known competitors (ten at most), on the default engines (18 credits per prompt, 54). Call `get_task` (free) after `poll_after_s`. `mentions[]` only detects the brands passed in, so read each `answer` for the other names too, and count in how many of the 15 cells each brand appears. These are the brands buyers hear about when they ask.
4. **Company data.** `leads_search_companies` with the category words as `keywords` (10 credits per page of 100). `rows_available` says how crowded the category is. Rows are stubs (name, domain, `founded_year`); keep the domains another source also named, or whose homepage title (`seo_get_page`, free) names the same job.
5. **Ads.** `ads_search_ads` with the category words as `query` and `active_only: true` on `facebook`, `linkedin` and `tiktok` (1 credit a page each). Only Facebook applies `active_only`; on the other libraries keep the rows whose `active` is true or whose `last_shown` is recent. An advertiser paying for those words now is competing for the same buyer now.
6. **Reddit.** `reddit_search_posts` for "alternative to <best-known competitor>" and "<category> recommendation" (1 credit each), and `reddit_search_comments` for "switched from <best-known competitor>" (1 credit), all with the default relevance sort and no time limit, per the [router](../SKILL.md#evidence); keep the rows whose `created_at` falls in the past two years. Then `reddit_get_comments` on the three busiest threads (1 credit each). The names in the replies are what buyers actually compare; the replies also carry the indirect competitors ("we just use a spreadsheet", "our agency does it").
7. **Classify and size.** Merge into one row per company and classify it direct, indirect or search-only per the [router](../SKILL.md#real-competitors). `seo_get_traffic_estimates` on the kept domains in one call (50 plus 50 per 100 domains: about 60 for 20) for `organic_traffic` as a size signal. Fetch `leads_get_company` (10 credits) only for a candidate whose homepage leaves the class unclear.
8. **Deliver** a table: company, domain, class (direct, indirect, search-only), what they sell (from the title or description), the sources that named them (search, AI answers with the cell count, company data, ads, Reddit), `organic_traffic`, known to the user (yes or no), and one evidence link. Direct competitors first, sorted by the number of sources; indirect next; search-only last. Name the two or three to tear down next with [teardown](teardown.md).

## Judgment

- The number of independent sources is the best signal. A brand in AI answers, Reddit threads and active ads is a real competitor whatever its traffic; a domain found only by `seo_get_serp_competitors` is search-only until something else names it.
- The top SERP competitors are usually G2, Capterra, Forbes and the like. They sell attention, not the product: search-only, never direct.
- A competitor the user has never heard of, named across several engines, is the finding of the run. Say it first.
- For an early product the biggest competitor is often indirect: the spreadsheet, the agency, doing nothing. Reddit shows it; keep it in the table even with no domain.
- `leads_search_companies` matches keywords loosely, and its first page is a sample, not the leaders. Use it to add names, never to rank them.
- A Reddit search is ranked and never complete. Count the names across threads; do not treat one thread as the market.
- Traffic sizes SEO-led competitors well and sales-led ones badly: an enterprise vendor can have little traffic and a large sales team.
