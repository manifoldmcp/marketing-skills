# Crawling, rendering, canonicals and indexing

Whether Google can fetch a page, render it, choose it as the canonical and keep it in the index, and what Search Console's statuses mean. Google rewrote most of this documentation between November 2025 and July 2026 and moved the crawling pages to a separate crawling site; a model's memory of the old pages is out of date in places. Checked October 2026.

## Page indexing statuses

Google says plainly not to expect every URL to be indexed. The question is whether the pages that should earn traffic are. The totals in the Page indexing report are complete; the example list per status stops at 1,000 URLs.

| Status | What it means | What to do |
|---|---|---|
| Crawled, currently not indexed | Fetched, judged, left out. "It may or may not be indexed in the future." | On a page that matters: make it clearly different from and better than its near-duplicates, link to it from pages that rank, or merge it into one that does. Resubmitting does not help. On thin tag, filter or archive pages: expected, leave it. |
| Discovered, currently not indexed | Known, not yet fetched; Google cites site load. | On a large site, crawl budget (below). On a small site load is rarely the reason: the URLs look low-value or are poorly linked. Link them from crawled pages, or stop generating them. |
| Duplicate, Google chose different canonical than user | Google indexed another URL from the cluster. | Find out why (below). Often harmless if the chosen URL is the user's own equivalent. |
| Alternate page with proper canonical tag | Working as intended. | Nothing. |
| Soft 404 | A "not found" page answering 200, or a redirect to an irrelevant page such as the homepage. | Return 404 or 410, or redirect to a real equivalent. |
| Page with redirect | A redirecting URL is not indexed; its target may be. | Nothing, unless the target is wrong. |
| Blocked by robots.txt | Not fetched. It can still be indexed from links, with no snippet ("Indexed, though blocked by robots.txt"). | To keep a page out, allow crawling and use `noindex`; robots.txt cannot remove a page. |
| Excluded by 'noindex' tag | The page asked to be left out. | Check it was meant to: a leftover staging `noindex` is the classic migration fault. |

## Crawling and crawl budget

- **Most sites have no crawl budget problem.** Google's guide is for sites of about 1 million pages that change weekly, 10,000 or more that change daily, or with many URLs stuck in "Discovered, currently not indexed". Below that, slow indexing is about demand (perceived value, links), not capacity.
- **What wastes it:** duplicate and parameter URLs (sorts, filters, session IDs), soft 404s, long redirect chains, slow responses, and `noindex` pages, which Google still fetches before dropping.
- **What does not save it:** blocking URLs in robots.txt does not hand their budget to other pages unless Google is already at the site's capacity limit. 404 and 410 are strong signals to stop crawling a removed URL; Google never forgets a known URL entirely.
- **Server signals:** 429 and 5xx responses lower Google's crawl rate for the whole site, shared across all Google crawlers. A CDN or firewall that answers bots with 429 or 5xx under load slows crawling site-wide.
- **Mobile first:** most crawl requests come from Googlebot Smartphone. robots.txt cannot address the mobile and desktop crawlers separately. A mobile template missing content or links the desktop has loses them in Google.
- **The 2MB limit (documented February 2026).** Googlebot fetches the first 2MB of each HTML or other supported file (uncompressed, headers included), and indexes and renders what it got as if it were the whole file; PDFs get 64MB. The long-quoted 15MB is now the default for Google's other crawlers. Each script and stylesheet has its own 2MB. Pages bloated by inline base64 images, inline JSON state or huge menus can lose their canonical, structured data or body text past the cutoff: keep the head and the main content early in the HTML.

## Rendering JavaScript

- **Google renders, with a queue.** Every 200 page goes to rendering. In Vercel and MERJ's study of over 100,000 Googlebot fetches (mostly nextjs.org, April 2024; vendor study) all HTML pages were fully rendered; the delay from crawl to render was a median 10 seconds, about 3 hours at the 90th percentile and 18 at the 99th. JavaScript complexity did not change it.
- **Pages that are not 200 may not be rendered** (December 2025). A client-rendered app that serves its error page or its content with a non-200 status hides its content from Google.
- **`noindex` in the raw HTML stops rendering.** Google may skip rendering on seeing it, so JavaScript that removes it does nothing; Google calls the behaviour "not well defined". Never ship `noindex` in the initial HTML of a page that should be indexed.
- **Canonicals before and after rendering.** Google reads the canonical in both. Put it in the raw HTML; if JavaScript sets it, set the same URL. In the HTTP Archive's 2025 crawl, about 3% of pages changed their canonical during rendering and 0.7% had conflicting canonicals.
- **Links need `<a href>`.** Googlebot does not click and does not follow fragment routes (`#/page`). Each view needs its own URL through the History API.
- **Soft 404 in single-page apps:** redirect missing routes to a URL that returns 404, or add `noindex` to the error view.
- **Other engines mostly do not render.** AI answer crawlers read the raw HTML; a client-rendered site is empty to them. That check lives in [ai-search](../../ai-search/SKILL.md).

## Canonicals

- **Signals, strongest first:** a redirect, then `rel="canonical"`, then sitemap inclusion (weak). All are signals; Google picks the canonical from content similarity and can overrule them. Name the same URL in every method.
- **Do not canonicalise with** robots.txt, `noindex` or the URL removal tool.
- **When Google picks another URL,** the usual causes are a CMS or plugin emitting the wrong canonical or redirect, regional duplicates without `hreflang`, syndicated copies (block indexing on the partner's copy rather than rely on a cross-domain canonical), scrapers, or hacked redirects. Google says re-evaluation after a fix can take up to two weeks (July 2026).
- **Protocol and host variants.** Every variant (http, https, www, apex) must redirect in one hop to the canonical host. Search Console can keep listing and even showing an old variant for weeks after the redirect is in place ([field notes](field-notes.md)).

## Reading Google's view with Manifold

- `console_inspect_url` reports Google's last indexed crawl, not the live page: `coverage_state`, `google_canonical` against `user_canonical`, `last_crawl_at`, `crawled_as`, `referring_urls[]`. Check `last_crawl_at` before reading a fix as failed. The API has no live test (the Search Console UI does). 2,000 inspections per property per day, shared by everyone on it. "URL is unknown to Google" can come back for a URL that still has impressions in Performance; trust Performance for whether a URL was shown.
- `console_get_sitemaps`: `last_read_at`, `urls_submitted`, `errors`. Its `urls_indexed` is always 0 today; do not report it.
- `seo_get_page` (free, cached an hour) is the live page as a bot without JavaScript sees it: `status`, `final_url`, `canonical`, `robots_meta`, `word_count`. A 403 titled "Just a moment..." with `noindex,nofollow` is a CDN challenge page served to Manifold's fetcher, not the site; it says nothing about what Googlebot gets.
- `console_get_crawl_issues` lists URLs Bing failed to crawl, with inbound link counts: free ground truth for one engine, and each failing URL is worth checking on Google with `console_inspect_url`.
