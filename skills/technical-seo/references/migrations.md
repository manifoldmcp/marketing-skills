# Site migrations

A domain change, new URL structure, replatform, redesign or merging a subdomain into the main site. Rankings survive when every URL that earns traffic or links has a one-to-one redirect to its closest new equivalent, and the new pages carry what the old ones did. Before launch the job is an inventory and a redirect map; after launch, a check that the redirects work and positions held. Setting redirects is the developer's job; never do it. Checked October 2026.

## What to expect

- **Domain moves recover slowly, and some never do.** In Dan Taylor's study of 892 domain migrations (Search Engine Journal, January 2025; data to October 2024), the new domain took an average 523 days to match the old domain's estimated organic traffic, and 17% had not after 1,000 days. The fastest took 19 to 33 days. The 2023 edition (171 migrations) had found 229 days and 42%. Caveats: estimated traffic from Ahrefs, not analytics; domain changes only; no split by type or by how well each was run; any core update in those 500 days counts against the move. Recovery correlated loosely with referring domains.
- **Google's own expectation is shorter**, for moves done right: for a medium site "a few weeks or more" for new URLs to replace old ones, longer for large sites. A path change on the same domain usually settles faster than a domain change.
- **A dip of a few weeks is normal.** A page that has not recovered in six to eight weeks is a fault to find, usually its redirect, its canonical or content that did not come across.
- Never promise a no-loss migration. Promise a map that covers every URL that matters and a check that catches what breaks.

## What fails

No study ranks migration faults by frequency. Google's site move guide names the first three; the rest are the faults practitioners report most:

- **Many old URLs redirected to one irrelevant page,** most often the homepage. Google can treat it as a soft 404, and the old page's rankings go with it.
- **Staging blocks left on:** `noindex` or a robots.txt disallow carried to production.
- **Redirects to URLs that do not exist,** chains of several hops, and 302s where 301 or 308 was meant.
- **Missing rows:** URLs with backlinks or long-tail traffic left out because the map was built from the sitemap alone.
- **Old signals left pointing back:** canonicals, `hreflang`, internal links and sitemaps still listing old URLs.
- **Changing everything at once.** New domain, new URLs, new templates and rewritten copy together make any loss impossible to pin down. Keep titles, headings and body content unless there is a reason to change them, and change them in a later release.
- **Change of Address on only one variant.** Since June 2026 Google's guide says to submit it for every verified variant of the old domain (www, apex, subdomains), even unused ones. Not needed for http to https or www to apex on the same domain.

Small and medium sites should move all at once; large sites can move a section at a time, which makes problems easier to find but is not a full rehearsal.

## Before launch

1. **Baseline the old site.** `seo_get_domain_overview` (15 credits), a crawl sized to the site (50 credits for 1,000 pages) kept as the health baseline, noting issues not to carry over.
2. **List every URL that matters.**
   - With Search Console: pages with any click in 12 months, `dimensions: ["page"]` and `limit: 1000`, following `cursor` (free). The fullest list.
   - Ranking URLs: `seo_get_ranked_keywords` with `limit: 1000` (55 credits), grouped by `url`, summing `traffic`; keep each URL's keywords and `rank` as the position baseline.
   - Linked URLs: `seo_get_backlinks` with `limit: 1000` (25 credits), live rows grouped by `url_to`, counting linking domains.
   - Plus the user's sitemap or CMS export for everything else.
3. **Build the map.** One row per URL that ranks, earns clicks or has links: old URL, new URL, 301 (or 308). Same content first, then closest topic, then parent category; never the homepage. Old URLs with no traffic, no links and no equivalent can return 404 or 410.
4. **Record what the pages carry.** `seo_get_page` (free) on the top 50 by traffic and links: `title`, `meta_description`, `h1`, `canonical`, `schema_types`.

Deliver: old URL, clicks or estimated traffic (say which), linking domains, top keyword and rank, new URL, status to return, notes. Budget about 145 credits.

## After launch

| When | Check | With |
|---|---|---|
| Launch day | New site crawl: new 4xx or 5xx, `non_indexable` (leftover staging `noindex`), `duplicate_titles`, `is_redirect` and `canonical_chain` from internal links still pointing at old URLs | `seo_run_technical_crawl` on the new domain |
| Launch day | Every row of the map: `final_url` equals the mapped URL and `status` is 200 | `seo_get_page` per old URL (free, rate limited) |
| Launch week | Sitemap read, top URLs indexed with the right canonical | `console_get_sitemaps`, `console_inspect_url` on the new property |
| Week 1 | Ten top keywords: the new URL ranks near the old rank | `seo_get_position` with the new domain (6 credits each) |
| Weeks 2 to 4 | URL by URL against the baseline; daily clicks on old and new properties | `seo_get_ranked_keywords` on the new domain; `console_get_search_analytics` by date |
| Weeks 6 to 8 | Pages not recovered: their redirect, canonical, content | `seo_get_page`, `console_inspect_url` |
| 12 months | Keep redirects at least a year (Google); longer is better for links | The developer |

- `seo_get_page` shows where a redirect ends, not whether it was a 301 or a 302 or how many hops it took. A host with a shell can read each hop with `curl -sIL`; otherwise the user runs a redirect checker. The crawl's `status_codes` counts 301 against 302 only for old URLs still linked inside the site.
- In the first weeks trust live positions and Search Console: `seo_get_ranked_keywords` is cached seven days and the provider's index refreshes on its own schedule, so it lags.
- The user, not a tool, runs Change of Address and submits the new sitemap.
- Links to old URLs pass through redirects, but a direct link does not depend on one being kept. After a domain change, ask the sites behind the strongest links (by `dr_from`) to update them: finding their contacts is in [link-building](../../link-building/SKILL.md).

Budget after launch about 165 credits.

## Without the connector

The method is the same by hand: export every URL with clicks from Search Console's Performance report (16 months), add a backlink tool's top linked pages and the sitemap, build the map in a spreadsheet, crawl the old URL list after launch in a desktop crawler in list mode to read each redirect's status and hops, and watch the Page indexing report on the new property weekly.
