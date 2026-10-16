# A technical audit

One question: can Google crawl, render, index and understand the pages that earn or should earn traffic? An audit is a crawl weighed by traffic, ending in a fix list ordered by impact, not by the crawler's counts. The fixes go to the user's developer; never edit the site. Checked October 2026.

## Order of work

1. **Weigh first.** Which pages earn traffic: `console_get_search_analytics` with `dimensions: ["page"]` over 90 days (free, measured), or `top_pages` from `seo_get_domain_overview` (5 credits, estimated). An issue matters by the traffic on the pages it touches, not by how many pages it touches: a `noindex` on the pricing page beats a thousand missing alt texts.
2. **Test rendering.** `seo_get_page` (free) on the homepage and one key template. A `word_count` under about 100 or an empty `h1` on a page that shows full content in a browser means the content needs JavaScript: crawl with `render: true` and treat it as a finding, since Google renders with a delay and other engines mostly do not ([indexing](indexing.md#rendering-javascript)).
3. **Crawl.** `seo_run_technical_crawl` sized to the site, then `get_task` after `poll_after_s` (a few minutes for a few hundred pages).
4. **Confirm on pages that matter.** For each sample URL that is also a top page, `seo_get_page`; with Search Console, `console_inspect_url` on up to ten key URLs and `console_get_sitemaps`; with Bing, `console_get_crawl_issues`.
5. **Rank and deliver** (below).

## Reading the crawl

- **Size.** `max_pages` up to 1,000 per crawl on the current plan. It charges on pages requested, not crawled: 3 credits per 100, 30 per 100 with `render: true`. If `pages_crawled` is far below the site's size, the crawler was blocked or internal links do not reach the pages; if it equals `max_pages`, the result is a sample, so say so. A page the user expects that the crawl never found is a finding: nothing links to it.
- **Where it starts.** It follows links from the homepage of the `target` host and stays on that host unless `include_subdomains` is true, when subdomains share `max_pages`. Crawl a large blog or docs subdomain as its own `target` ([field notes](field-notes.md)).
- **Time.** A few hundred pages took 10 to 45 minutes in our runs. Start the crawl first and do the free checks while it runs.
- **What comes back.** `status_codes` counts crawled pages per HTTP status: the reliable count. `broken_links` counts links, not broken pages, so one broken link in a template reads as one per page. `non_indexable` counts URLs blocked from indexing by robots.txt, headers or meta tags, including linked URLs robots.txt kept the crawler from, so it can exceed `pages_crawled`. Quote neither without its sample URLs. `duplicate_titles` and `duplicate_descriptions` are counts of pages. `issues[]` lists each failed `check` with `pages` and up to ten `sample_urls`. Sort them:

| Bucket | Checks | Priority |
|---|---|---|
| Indexing | `is_5xx_code`, `is_4xx_code`, `is_broken`, `is_redirect`, `canonical_chain`, `recursive_canonical`, the `non_indexable` count | First, on pages that earn traffic |
| Duplication | `duplicate_title`, `duplicate_description`, `duplicate_content` | Second; usually one template fix |
| Structure | `is_orphan_page`, `is_link_relation_conflict`, `is_http` | Third |
| On-page | `no_title`, `no_description`, `no_h1_tag`, `title_too_long`, `title_too_short`, `low_content_rate` | Fourth; handed to the writer |
| Speed and hygiene | `high_loading_time`, `large_page_size`, `has_render_blocking_resources`, `no_image_alt`, `no_favicon`, `has_misspelling` | Last, one line, not one row each |

- **One template fix clears hundreds of pages.** When sample URLs share a path pattern, say so and count it once.
- **Cloudflare's own paths** (`/cdn-cgi/...`, such as its email-obfuscation link) show as 4xx or broken. They are artefacts, not lost pages.
- **`onpage_score` is DataForSEO's score, not Google's.** A site at 98 can have its money page set to `noindex`. Never chase 100.

## What the crawl does not measure

- **Resources are not loaded.** Images, scripts and stylesheets are not fetched, so `broken_resources` reads 0 whatever the truth: a 0 there is not a pass.
- **`has_render_blocking_resources` fires on nearly every page** of a normal site (102 of 103 in our test) and says nothing about real speed. Leave it out of the fix list unless field data shows a slow LCP.
- **`low_content_rate` is a text-to-code ratio**, not thin content. It flagged a 1,700-word homepage in our test. Judge thin content by `word_count` and by what ranks.
- **No Core Web Vitals and no Lighthouse.** `high_loading_time` is the crawler's own fetch time from its own servers. For real speed, see [Core Web Vitals](core-web-vitals.md).
- **No Google view.** The crawler is not Googlebot: it can be blocked or challenged where Googlebot is not, and the reverse. Index status and the canonical Google chose come only from Search Console.
- **The 2MB cutoff.** `large_page_size` flags heavy pages, but whether Googlebot truncated one is not reported; check that the canonical and main content sit early in any HTML over about 1MB ([indexing](indexing.md#crawling-and-crawl-budget)).
- **AI crawler access** (robots.txt rules for answer engines, CDN blocks, llms.txt) is a separate free check, `aeo_get_site_readiness`, read in [ai-search](../../ai-search/SKILL.md).
- Results expire after 30 days. Crawl again after fixes ship and compare counts.

## Rank and deliver

Order: what blocks indexing on pages that earn traffic (5xx, `noindex`, wrong canonicals, soft 404s), then broken internal links and redirect chains, then duplication, then on-page gaps, hygiene last. A table: issue, check name, pages affected, sample URLs, why it matters, fix, owner (developer or writer), priority. Above it, the crawl size, whether it was rendered, the data sources and the date. Say what the crawl did not cover.

Budget: a 1,000-page audit is about 35 credits (5 for the overview, 30 for the crawl), or about 305 rendered. `seo_get_page`, `get_task` and the console tools are free.

## Without the connector

Search Console's Page indexing report and URL Inspection (with its live test) give Google's own view for free. A desktop crawler with a free tier (Screaming Frog's limit is 500 URLs) gives the crawl, and PageSpeed Insights gives field Core Web Vitals per URL and origin. Weigh issues by the Performance report's clicks per page. A `site:` search is not a count of indexed pages; never report it as one.
