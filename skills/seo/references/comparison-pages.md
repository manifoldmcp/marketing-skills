# Comparison and alternatives pages

The user's own "X vs Y", "X alternatives" and defence pages (answers to competitors' pages about the user). People searching these are close to buying and already know the category, so small volumes are worth a page. Checked October 2026.

## What the evidence says

- **Vendor pages can rank, mostly in B2B software.** Vendor-written lists sat in the top 10 on 46% of B2B software results pages, against 3% in food and 6% in travel; publishers beat vendors in 54% of head-to-heads (Kevin Indig, Growth Memo, August 2026, 60,000 queries). 62% of vendor lists gained estimated traffic from January to August 2026.
- **Self-ranking lists carry the risk.** The SaaS sites that lost 29% to 49% of visibility in January 2026 held many "best X" lists ranking themselves first ([updates](google-updates.md#self-ranking-listicles-contested)), and Google's AI Overviews, when they cite such a list, leave out the brand it ranks first 83% of the time (Lily Ray, September 2026). A self-ranking list tends to send AI answers to the competitors it names.
- **These SERPs are crowded.** Reddit or YouTube appeared on 92% of results pages that held a top-10 list, and AI Overviews on 93% of B2B list-related results (Indig, August 2026). Expect fewer clicks than the volume suggests, and check whether a third-party list or thread is the better target ([link-building](../../link-building/SKILL.md) and [ai-search](../../ai-search/SKILL.md) for getting onto them).

So: a straight two-way "X vs Y" page and an honest "X alternatives" list that includes other tools and says where each wins are both fine. A list that ranks the author first with no stated criteria or testing is not worth making.

## Finding the searches

- Per competitor, `seo_search_keywords` with `seed` set to "<competitor> alternative" and again to "<competitor> vs" (10 credits each). The default mode keeps phrases containing the seed, which catches the plural, "free", "for <use case>" and every vs pair people search.
- `seo_get_keyword_metrics` on the phrases with the user's own name ("<product> vs <competitor>", "<product> alternatives", "<product> reviews"). Small brands often get null or tiny volume; that is normal, and null is not zero ([keyword data](keyword-data.md#volume)).
- The competitors are the products buyers compare the user with. Ask, or take them from [competitor-intel](../../competitor-intel/SKILL.md); `seo_get_serp_competitors` returns sites that share results, mostly publishers.

## What exists and who ranks

- `seo_get_ranked_keywords` on the site, `limit: 500` (30 credits): rows whose keyword holds a competitor's name, "vs", "alternative" or "compare" show which comparison pages exist, their `rank` and `url`. Ask for comparison pages that rank for nothing and read them with `seo_get_page`.
- `seo_get_serp` on the ten biggest phrases (1 credit each). Sort each page one into vendors' own pages, third-party lists and review sites, forum threads, videos. A vendor page in the top five proves a vendor page can rank there. Page ones held only by lists and review sites are better approached by getting onto those lists.
- `seo_get_page` on each vendor comparison page: the criteria in their `h2` headings (price, features, migration, support, who it is for) are what buyers expect compared.
- **Look hardest at the user's own name.** A competitor's "<product> alternatives" page ranking there is taking the user's buyers; a defence page (an honest "<product> vs <competitor>" or "is <product> right for you") comes first.

## Rules

- **Facts dated and sourced.** Current prices, plans and features for every competitor, "as of <month>", with a source for each claim. No tool reads a page's body text; take facts from the user or [competitor-intel](../../competitor-intel/SKILL.md). Never guess a price. Comparative claims carry legal risk in some markets.
- **One page per pair or phrase.** Not separate pages for "alternative" and "alternatives", and not one templated page per competitor with the names swapped ([scaled content](google-updates.md#policies-that-decide-what-not-to-build)).
- **Keep rows the user loses.** A comparison where the author wins every row reads as an ad to buyers and to AI answers.
- Start with phrases where a vendor page already ranks and the user has none: proven winnable.

## Deliverable and cost

About 110 credits for three competitors. The table: phrase, volume, KD, page type (vs, alternatives, defence), the user's URL and rank now, who holds the top three, the competitor pages to match and their criteria, action (create, improve, leave), priority. Each page then gets a [brief](briefs.md).
