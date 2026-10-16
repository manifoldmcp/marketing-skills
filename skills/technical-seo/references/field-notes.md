# Field notes

What Manifold has seen in its own data: Search Console for Manifold's own small sites, and crawls run through `seo_run_technical_crawl`. Small samples: read them as evidence of how the data behaves, not as benchmarks. Newest first.

## What the crawl's counts really count (15 October 2026)

`seo_run_technical_crawl` on dataforseo.com with `max_pages: 300` (9 credits). It took about 45 minutes; the 200-page crawl below took about 10. Tell the user a crawl of a few hundred pages can take most of an hour.

- `pages_crawled` 300, equal to `max_pages`, so the result is a sample of a larger site. `status_codes`: 249 at 200, 49 at 301, one 302, one 404.
- **`non_indexable` was 713, more than twice the pages crawled.** It counts URLs blocked from indexing by robots.txt, headers or meta tags, and a URL robots.txt blocks is counted without being crawled, so it is not a count of crawled pages. Never report it as "pages of the site that cannot be indexed"; use `status_codes` and the `issues[]` rows for that.
- **`broken_links` was 249 for one broken URL.** The only 404 was `/cdn-cgi/l/email-protection`, Cloudflare's email-obfuscation link, present in a template on every page. The count is links, not broken targets: one template link inflates it to the page count. Read the `is_broken` and `is_4xx_code` sample URLs before quoting the number, and recognise Cloudflare's own paths as artefacts, not lost pages.
- **No subdomain was crawled.** Every sample URL across 14 checks sat on dataforseo.com, though its docs and app subdomains are linked from the navigation. The crawl stays on the target host unless asked: the tool now has `include_subdomains` (false by default), and the subdomains then share `max_pages`. Crawl a large blog or docs subdomain as its own `target`.
- 50 `is_redirect` pages were internal links to URLs that 301 (mostly a trailing-slash mismatch). Real, low priority: point the links at the final URLs.
- `has_render_blocking_resources` fired on 103 of 300 pages here, against 102 of 103 below: it varies by template, but still says nothing about field speed. `no_image_alt` fired on every 200 page.

## What the crawl flags on a healthy site (15 October 2026)

`seo_run_technical_crawl` on site C with `max_pages: 200` (6 credits). The sitemap lists 94 URLs.

- `pages_crawled` 103: 102 pages answered 200 and one 301. Charged for 200 requested, as documented. `onpage_score` 97.81; `broken_links`, `non_indexable` and both duplicate counts 0.
- **`has_render_blocking_resources` on 102 of 103 pages.** The crawler loads no resources, so this comes from the HTML pattern alone, and it fires on a statically generated site. It is not evidence of slowness.
- **`low_content_rate` on 62 pages, the homepage among them** (1,693 words by `seo_get_page`). It measures text against code, not thin content.
- The crawl entered at `http://www.site-c.example/` and reported it as `is_http` and `is_redirect`: the host's http to https redirect, which is correct. One flagged URL, not a site-wide problem.

## Search Console disagrees with itself (15 October 2026)

site C, a domain property, 30 days of Performance by page against `console_inspect_url`, all free.

- **Old host variants still show.** Performance listed `http://` URLs and the apex `https://site-c.example/` with impressions, although every variant answers a single 301 to `https://www.` (checked live with curl).
- **Inspection is the stored record, not the live page.** `http://www.site-c.example/privacy` came back "Submitted and indexed" with itself as `google_canonical`, `sitemaps[]` empty and `last_crawl_at` three weeks old, while the live URL 301s to the https page, whose canonical is the https URL. We could not tell when that redirect went in; either way, read `last_crawl_at` before concluding a fix failed.
- **"URL is unknown to Google" for a URL with clicks.** An API endpoint URL on a subdomain had 4 impressions and 1 click in Performance, and inspection said unknown. Trust Performance for whether a URL was shown.
- `console_get_sitemaps` read the sitemap the day before, 94 URLs, `urls_indexed` 0, as the tool description warns.

## The num=100 break on one small site (15 October 2026)

site B, daily Search Console rows by device, 20 August to 10 September 2025 (22 days) against 13 September to 5 October 2025 (23 days). Free.

| Device | Impressions a day, before | After | Clicks a day, before | After | Average position, before | After |
|---|---|---|---|---|---|---|
| Desktop | 1,397 | 531 (−62%) | 3.4 | 3.6 | 34.6 | 20.4 |
| Mobile | 308 | 124 (−60%) | 1.6 | 1.5 | 39.4 | 24.3 |

- The step came on 12 September 2025 (desktop 1,162 on the 10th, 1,078 on the 11th, 664 on the 12th) and held.
- Clicks did not move; impressions fell by more than half and average position "improved" by 14 to 15 places. Read without the date, that is a crash plus a ranking gain, and neither is real.
- **Mobile fell as much as desktop here,** although published accounts describe a mostly desktop effect. One site of a few clicks a day; do not assume mobile impressions are safe across that date.
- Around the end of Google's impressions logging error (27 April 2026) the same property showed no clean step: daily impressions swung between 840 and 3,650 in the weeks either side. On a small site that fix is lost in the noise.

## Fetch quirks

- `seo_get_page` on a site behind a bot challenge returns its challenge page: on lovable.dev, `status` 403, title "Just a moment...", `robots_meta` "noindex,nofollow", `word_count` 0. That is what Manifold's fetcher got, not what Googlebot gets. Check with Search Console or in a browser before reporting a blocked or noindexed site.
