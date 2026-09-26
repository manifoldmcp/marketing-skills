# Technical audit

A technical audit answers one question: can Google crawl, index and understand the pages that matter? This playbook crawls the site, weighs every issue by how many pages it hits and whether those pages earn traffic, and ends in a fix list ordered by impact, not by the crawler's count.

## Inputs to settle first

- **Site**: the domain. If a subdomain matters (a blog or docs host), check afterwards that the crawl reached it in `sample_urls`; if it did not, crawl that host as its own `target`.
- **Size**: roughly how many pages the site has. `max_pages` should cover it, up to 1,000 per crawl today. Default: 500 for a small business site, 1,000 otherwise.
- **JavaScript**: whether the pages build their content in the browser (many React and Vue single-page apps do). Step 1 tests it.
- **Search Console**: optional. With the `console_*` tools, index status comes from Google itself; the [router](../SKILL.md#search-console) says how to check. The audit runs in full without it.
- **Budget**: a default run costs about 5 + 30 = 35 credits for 1,000 pages, or 5 + 300 = 305 rendered; `seo_get_page` and `get_task` are free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Size it and test rendering.** `seo_get_domain_overview` on the site (5 credits): `organic_keywords` and `top_pages` say which pages earn traffic, so the audit weighs issues by them. Then `seo_get_page` on the homepage and one key page (free). If `word_count` is tiny (under about 100) or `h1` is empty on a page that shows full content in a browser, the content needs JavaScript: crawl with `render: true`, and flag that Google must render it too.
2. **Crawl.** `seo_run_technical_crawl` with the domain as `target` and `max_pages` from the inputs (3 credits per 100 pages, 30 per 100 with `render: true`; charged on `max_pages` requested, not crawled). It returns a `task_id`. Call `get_task` (free) after `poll_after_s`; while it answers `TaskPending`, wait that long again. If `pages_crawled` is far below the site's size, the crawler was blocked or the internal links do not reach the pages; if it equals `max_pages`, the audit is a sample.
3. **Read the result.** `status_codes` counts the crawled pages per HTTP status; `broken_links`, `broken_resources`, `non_indexable`, `duplicate_titles` and `duplicate_descriptions` are counts. `issues[]` lists each failed `check` with `pages` and up to 10 `sample_urls`. Sort the checks into five buckets:
   - Indexing: `is_5xx_code`, `is_4xx_code`, `is_broken`, `is_redirect`, `canonical_chain`, `recursive_canonical`, and the `non_indexable` count.
   - Duplication: `duplicate_title`, `duplicate_description`, `duplicate_content`.
   - On-page: `no_title`, `no_description`, `no_h1_tag`, `title_too_long`, `low_content_rate`.
   - Site structure: `is_orphan_page`, `is_link_relation_conflict`, `is_http`.
   - Speed and hygiene: `high_loading_time`, `large_page_size`, `has_render_blocking_resources`, `no_image_alt`, `no_favicon`.
4. **Confirm on the pages that matter.** For every sample URL that is also a top page, `seo_get_page` (free): `status`, `canonical`, `robots_meta` and `final_url` confirm the issue on the live page. With Search Console: `console_inspect_url` on up to 10 key URLs for `coverage_state`, `google_canonical` against `user_canonical` and `last_crawl_at`, and `console_get_sitemaps` for sitemap errors (free). With Bing connected, `console_get_crawl_issues` lists the URLs Bing failed to crawl with their inbound links (free).
5. **Rank the fixes.** Order by what blocks indexing, then by pages affected, then by the traffic of those pages: 5xx and noindex or wrong canonicals on pages that earn traffic first; then broken internal links and redirect chains; then duplicates; then on-page gaps; hygiene last. One template fix often clears hundreds of pages: say so when the sample URLs share a pattern.
6. **Deliver** a table: issue, check name, pages affected, sample URLs, why it matters, the fix, owner (developer or writer), priority. Put the `onpage_score` and the crawl size above it, and say what the crawl did not cover.

## Judgment

- `onpage_score` is DataForSEO's score, not Google's. Do not chase 100; chase the issues on pages that earn traffic.
- Many flags are cosmetic. `title_too_long` on a blog post, `no_image_alt` on decorative images and `has_misspelling` rarely move rankings. Mention them in one line, not one row each.
- The crawler follows links from the homepage, so a page nothing links to may never be crawled. A page the user expects that is missing from the crawl is itself a finding.
- There is no Lighthouse or field data in this crawl. `high_loading_time` is the crawler's own timing; for Core Web Vitals the user checks PageSpeed Insights or Search Console's report.
- Results expire after 30 days. Crawl again after the fixes ship and compare the counts.
- Whether AI crawlers (GPTBot, PerplexityBot, ClaudeBot) can read the site is a different check with a different tool: the `ai-search` group's [site readiness](../../ai-search/references/site-readiness.md) playbook. Point to it; do not fold it into this audit.
