# Field notes

What Manifold has measured on its own Search Console properties and keyword data. Two small sites (site C, a few dozen clicks a quarter; site B, a few hundred), named by letter because their Search Console data is private, so read these as evidence of how the data behaves, not as benchmarks. Newest first.

## Search Console, keyword volumes and estimates (15 October 2026)

30 credits in all; every `console_*` call was free.

**Most clicks sit in hidden queries.** 90 days (14 July to 11 October 2026), grouped by `device` with and without a query filter (`includingRegex` on `.+`), which drops anonymised queries:

| Property | Clicks, total | Clicks in visible queries | Hidden share of clicks | Hidden share of impressions |
|---|---|---|---|---|
| site C | 24 | 3 | 88% | 22% |
| site B | 287 | 133 | 54% | 5% |

Clicks are hidden far more than impressions: the queries people click are long and rare, the impressions pile up on head terms. Any analysis built from query rows (quick wins, cannibalisation, CTR by query) sees a minority of a small site's clicks.

**Many impressions are machines.** site C's top 40 query rows (12 September to 11 October): 216 of 288 impressions (75%) averaged a position beyond 50, a depth people rarely reach since Google stopped serving 100 results a page, and 15 of the 40 rows were quoted brand names or long `-site:` exclusion strings of the kind brand-monitoring tools send. site B's US rows held prompt-length questions and queries carrying a date ("... october 4 2026"), which read as AI agents searching Google. None of these rows had a click. Weight decisions on clicks; treat deep-position and operator-laden impressions as noise.

**Keyword volumes are Keyword Planner's, a month late.** 15 US keywords through `seo_get_keyword_metrics`, default Labs source (6 credits) against `source: "google_ads"` (24 credits):

- Where both had data, the monthly `trend[12]` values were identical, Labs ending August 2026 (`data_as_of` 17 September) and Planner ending September. Headline `volume` differed (210 against 170 for "ai landing page generator") only because the windows differ.
- Every value sat in Planner's buckets (10, 20, 30, 40, 50, 70, 90, 110, 140, 170, 210, 260, 320, 390, 480 ...).
- Labs returned null for two long-tail "alternatives" phrases, one of which Planner put at 10. Planner returned null for all five "<tool> mcp" product terms that Labs put at 90 to 480 ("google search console mcp" at 480, rising from 110 to 1,000 a month over the year). Neither source is a superset; try the other when a keyword that matters comes back null.
- "a single purpose landing page" averaged position 3.7 in Search Console with 22 US impressions over the month, against a Planner volume of 110 for September. One data point; impressions are not volume.

**The traffic estimate missed by 14 times.** `seo_get_domain_overview` on site B (served from cache, 0 credits) put US `organic_traffic` at 129 visits a month, 113 of them from one 2025 guide. Search Console showed 9 US clicks site-wide over the last 30 days, and that guide had under two US impressions: its impressions were in Sweden, Australia and elsewhere. `keyword_changes` showed 48 keywords lost at the index's last update. For small sites the estimate can be stale and far off; check it against Search Console before quoting it.

**Unexplained, verify before relying on it:** grouping site B by `page` with a country filter on the US returned about 80 impressions over 30 days, against 5,245 for the US when grouped by `country` or by `country` and `date`. Google aggregates by page differently, but not by that much. Until this is understood, read page-level numbers for one country with care and cross-check against a country total.
