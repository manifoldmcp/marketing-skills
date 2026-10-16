# Field notes

What Manifold has seen in its own runs of the backlink tools. One site and a few hundred domains: read these as evidence of how the fields behave, not as market benchmarks. Newest first.

## One startup's link profile, read every way (15 October 2026)

Target: folk.app, a CRM startup (domain rank 353, Ahrefs DR 72). Calls: `seo_get_referring_domains` with `limit: 1000` (25 credits), `seo_get_backlinks` with `limit: 1000` (25), `seo_get_backlink_summary` (10), and `seo_get_domain_ratings` on the top 200 linking domains plus ten well-known sites (7). 67 credits in all.

**Domain rank and Ahrefs DR agree on order, not on scale.** Across 197 domains rated both ways (notion.site subdomains left out, since Ahrefs gives them Notion's 92), Spearman 0.83.

| domain_rank band | Domains | Median Ahrefs DR | Range |
|---|---|---|---|
| 0 | 28 | 0 | 0 to 18 |
| 1 to 99 | 42 | 2.4 | 0 to 19 |
| 100 to 199 | 40 | 13 | 0 to 39 |
| 200 to 299 | 47 | 33 | 0.3 to 67 |
| 300 to 399 | 22 | 52.5 | 1.9 to 79 |
| 400 to 499 | 9 | 52 | 2 to 80 |
| 500 to 699 | 12 | 91 | 11 to 94 |
| 700+ | 7 | 95 | 91 to 100 |

Big platforms for reference: google.com 938, github.com 864, wikipedia.org 811, reddit.com 799, medium.com 742, g2.com 701, nytimes.com 686, hubspot.com 683, producthunt.com 672, forbes.com 644, techcrunch.com 636. Six domains had domain rank 200+ with DR under 10 (AI-tool directories, a status checker): where the two disagree, a link network is the usual reason.

**The row rank on referring domains is not the site's rank.** Each `seo_get_referring_domains` row's `rank_passed` is the rank that domain's links pass to the target, not the domain's own: it correlated 0.08 with the same domain's rank from `seo_get_domain_ratings`, and was lower for 139 of 197. Product Hunt: 158 on the row, 672 rated. HubSpot: 47 against 683. The rows are sorted by it, so the first 300 rows are not the 300 strongest linkers. Rate every candidate before filtering.

**Removed links need `status: "lost"`.** `lost` was false on all 1,000 referring-domain rows and all 1,000 backlink rows, because both calls listed live links, the default. With `status: "lost"` (checked on buffer.com the same day) the backlink rows come back with `lost` true and a `last_seen` date, and the referring domains are the ones that dropped a link (see [lost links](lost-links.md#links-other-sites-removed)).

**A thousand backlink rows can be a few dozen sites.** The 1,000 rows came from 106 domains; zapier.com's app directory alone supplied 548, each to a UTM-tagged signup URL, and four bing.com hosts 26. Grouping by `url_to` without stripping query strings would name a Zapier tracking URL as the site's most-linked page. 72% of rows were nofollow; 9 rows pointed at 404 pages.

**The profile is mostly directories and noise.** About a quarter of the top 200 linking domains read by name as AI-tool directories, SaaS lists or "alternatives" sites. 708 of the 1,000 rows had a row rank of 20 or less. 169 of 1,000 had `spam_score` above 30 and 55 above 60, among them a run of numbered seo-anomaly .xyz domains; 101 of the 169 were first seen in 2026. 788 of the 1,000 domains were first seen in 2026: a startup in the AI tools era collects directory links fast, and they are not the links to copy.

**Totals.** 19,834 backlinks from 3,349 referring domains (2,620 registered domains, which is what the row list counts), dofollow share 0.57, 879 links marked sponsored (4.4%), 21 broken backlinks to 43 broken pages.
