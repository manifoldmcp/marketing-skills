# Reading Search Console and Analytics

Search Console is the only measured record of how a site does in Google before the click; Analytics is what the visitor did after it. Both are free in Manifold. Both are also easy to misread, and the numbers changed meaning in September 2025. Checked October 2026.

## Connecting

- The `console_*` tools (Search Console and Bing Webmaster) and `analytics_*` tools (GA4) are optional and ride the user's own Google connection. If they are absent, use the estimates and do not ask the user to connect anything unless the job is about their own clicks, conversions or revenue. If the `seo_*` tools are missing while other Manifold tools are present, the SEO tool group is switched off on the app's Tools page; say so.
- Call `console_list_properties` first (free). Prefer an `sc-domain:` property, which covers every host and protocol; a URL property covers only that prefix. On `NotConnected`, give the user its `connect_url` once and carry on with estimates.
- Free, limited to 60 calls a minute per workspace, cached 6 hours. Google's data lags about two days (the tool's `end_date` defaults to three days ago) and goes back 16 months, so a lost baseline can be rebuilt from the console.

## What the numbers are

From Google's own Performance report docs (checked October 2026):

- **An impression** is the site appearing in results the user could see. Items in a carousel count only when scrolled into view; People Also Ask answers only when expanded. An AI Overview takes one position and every link in it shares that position. In AI Mode a follow-up question counts as a new query. AI Overview and AI Mode impressions and clicks stay inside the `web` totals.
- **Position** is the topmost result of the site per query, averaged. It is recorded only with an impression, so a page that no one scrolls to has no position at all, not a bad one. A 9.6 average can be 4 on desktop and 15 on mobile: add `device` to `dimensions` before acting on it.
- **Totals differ by grouping.** By property (query, country, device, date) two results of the site on one query are one impression; by page each page counts. Never add page rows and compare them with query rows.

## What is hidden

- **Anonymised queries.** Google withholds rare queries for privacy. They stay in totals grouped by date, country or device, but vanish from any query table and from any call that filters on query. For small sites this is most of the data: in our own properties 54% and 88% of clicks over 90 days sat in hidden queries, against 5% and 22% of impressions ([field notes](field-notes.md)). Long, specific, high-intent queries are the ones hidden, so a quick-win list built from query rows misses the long tail by construction.
- **To measure the hidden share:** group by `device` twice, once plain and once with a `filters` entry on query with `operator: "includingRegex"` and `expression: ".+"`. The difference is the anonymised traffic.
- **Row caps.** Google's API returns top rows, not all rows, and caps each request at 25,000; the tool returns up to 1,000 a call (`limit`, then `cursor`) and takes up to three `dimensions`. Query and page together is the costliest grouping and the first to be truncated on a big site; ask for one section with a page filter instead.
- **Filters combine with AND.** One `filters` entry per keyword returns nothing. To read a list of keywords, use one entry with `operator: "includingRegex"` and `expression: "^(keyword one|keyword two)$"`.

## The September 2025 break

Around 12 September 2025 Google stopped serving 100 results per request (the `num=100` parameter). Rank trackers and scrapers had been loading deep result pages, and each load counted as an impression for every site on it.

- 87.7% of 319 properties lost impressions and 77.6% lost unique ranking queries (Tyler Gargula of LOCOMOTIVE, reported by Search Engine Land, autumn 2025). An agency's 100 Australian campaigns averaged a 49.6% impression drop with average position improving 1.1 places (Edge, August to October 2025). The change was to what gets counted; nothing about it removed people clicking.
- **So never compare impressions, query counts or average position across September 2025.** A year-over-year impressions fall that starts there is a reporting change, not lost visibility. Compare clicks, or start the baseline after October 2025.
- Machine impressions did not go away. In our own data three-quarters of a small site's impressions in the top query rows sat at an average position beyond 50, a depth people rarely reach, and many queries were quoted brand names, long `-site:` exclusion strings or prompt-length questions with a date in them: monitoring tools and AI agents searching Google ([field notes](field-notes.md)). Treat impressions beyond position 20 and operator-laden queries as machines, and weight decisions on clicks.

## Reports the tools do not read

Google added these to the Search Console interface; none is exposed through the tools today, so ask the user to read them in the UI when they matter.

- **Branded queries filter** (November 2025, all eligible sites by March 2026): Google's AI classifier splits branded from non-branded, including misspellings and product names. Domain-level properties with enough volume only; Google says it misclassifies some queries. Without it, approximate with an `includingRegex` or `excludingRegex` on the brand's spellings. Always report brand and non-brand apart: brand clicks follow awareness, not SEO.
- **Generative AI performance report** (June 2026, all sites from 31 August 2026): impressions only in AI Overviews, AI Mode and Discover's AI features, by page, country and device. No clicks, no queries. It shows which pages Google's AI answers use; it cannot give AI click-through. Those impressions are also still inside the main totals.
- **Web multimodal** search type (September 2026): Lens, Circle to Search and image uploads.
- Weekly and monthly views (December 2025), custom annotations (November 2025), Query groups in Insights (October 2025). Annotations are the place for the user to mark when fixes shipped.

The tool's `search_type` reads `web` (default), `image`, `video`, `news`, `discover` and `googleNews`.

## Bing

`engine: "bing"` reads Bing Webmaster. Its query and page rows cover the last weeks with no date range, and it takes one dimension per call, so compare runs, not windows. Watch Bing only when it brings real clicks. `console_get_crawl_issues` is Bing-only.

## Google Analytics

- `analytics_list_properties`, then `analytics_get_report`. Organic alone is a `filters` entry on `sessionDefaultChannelGroup` equal to `Organic Search`; landing pages are `landingPage`. `compare_start_date` and `compare_end_date` give a period against the one before. Data settles within about a day.
- **Sessions are not clicks.** Consent banners, ad blockers and redirects drop sessions; one click can make none or several. Compare the shape of the lines (when each broke, which pages lead), never the totals.
- `subject_to_thresholding` means Google withheld small rows; low-traffic landing pages may be missing.

## Label every number

Search Console is measured. `traffic` and `organic_traffic` from the `seo_*` tools are models built from rank, volume and a click curve, and can miss a small site by an order of magnitude ([keyword data](keyword-data.md#estimated-traffic)). Never put the two in one column; say which each table uses. When they disagree on the user's own site, Search Console wins.
