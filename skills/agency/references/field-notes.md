# Field notes

What Manifold has seen in its own data that bears on pitching and reporting. Small samples: read them as evidence of how the data behaves, not as benchmarks. Newest first.

## Estimated traffic against real clicks (15 October 2026)

Five small sites the account owns, United States, English. `seo_get_domain_overview` (default params) against the same site's Google Search Console for September 2026, filtered to the US. 20 credits (the overviews were already cached; `seo_get_ranked_keywords` cost 6 and 14).

| Site | Estimated US organic visits a month | Keywords in the top 10 (estimate) | Search Console US clicks, Sept 2026 | US impressions | Average position |
|---|---|---|---|---|---|
| site A | 1,168 | 0 | 0 | 2,883 | 64 |
| site B | 129 | 3 | 2 | 5,182 | 28 |
| site D | 8 | 1 | 1 | 318 | 40 |
| site C | 0.2 | 0 | 3 | 61 | 35 |
| site E | null | null | 0 | a handful | |

- **The big estimate was fiction.** site A had 0 US clicks in September, 0 in June (2,084 impressions) and 0 in March (8,709 impressions), against an estimate of 1,168 visits a month. Its 162 ranked keywords sum exactly to the estimate, and the largest share (466 visits) comes from three keywords of 74,000 monthly searches ranked 49, 54 and 104, each credited 155 visits; every one of the 162 ranks outside the top 10. The model gives deep rankings on huge keywords a click share they do not get. A pitch quoting that number to the site's owner would have lost the meeting.
- **Rule from it:** before using `organic_traffic` for a small site, read `positions`. When `top_10` is 0 or near it, show keywords and rank instead of traffic. Comparisons between similar large domains from the same source remain fair; absolute numbers for small sites do not.
- **Small estimates can be low too.** site C was estimated at 0.2 visits and had 3 US clicks. Small sites are noise in both directions.
- **`seo_get_ranked_keywords` rows were not sorted by traffic** in these calls: on site A the first ten rows carried 7 of the 1,168 estimated visits, because the vendor's default order is by rank. The tool now asks for traffic order, highest first. One row had `rank` 104: `rank` counts every element on the page, ads and features included, so an organic top-100 keyword can read past 100.
- **`meta.data_as_of` is null on `seo_get_domain_overview`** but set on `seo_get_ranked_keywords` (17 and 26 September 2026 in these calls, so the index was two to four weeks old). Date an overview by the call, and say the index may be weeks old.
- **Search Console totals depend on the grouping.** For site B in September, the US total by country showed 2 clicks; the same filter grouped by page showed 14 pages with 0 clicks each, and by query, 0 clicks on the top rows (Google withholds rare queries and small rows for privacy). Take period totals from a country or date grouping.
- **Impressions without clicks are common on small sites.** site B's guide page had 2,532 US impressions at an average position of 35 and no clicks; the estimate credited that page 113 visits a month.
