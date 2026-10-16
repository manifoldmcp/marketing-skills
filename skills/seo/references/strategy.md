# An SEO plan

When the user wants a strategy rather than one job: what the site already wins, where competitors take the organic traffic it does not, which work gets the most traffic for the hours the team has, and a 30-60-90 day plan. It ends in a plan, not a keyword list. Checked October 2026.

## Settle first

Ask in one message; for anything unanswered use the default and say so.

- **Goal:** what organic search should bring (default: signups from commercial and transactional keywords) and how it is measured (Search Console clicks if connected, else estimated traffic and top-10 keywords).
- **Stage:** new site or established; `seo_get_domain_overview` answers it.
- **Team:** who writes, and whether anyone can change templates, redirects and speed. Default: the founder writes, no developer. Pace comes from the team: one good page a week beats five thin ones.
- **Competitors:** two or three that sell the same thing. `seo_get_serp_competitors` returns sites sharing results, often publishers; prefer the user's list or [competitor-intel](../../competitor-intel/SKILL.md).
- **Budget:** about 110 credits for the research below; horizon 90 days.

## Baseline (about 55 credits)

- `seo_get_domain_overview` on the site and each competitor (5 each): `domain_rank`, `organic_traffic`, `organic_keywords`, `positions`, `top_pages`.
- `seo_get_ranked_keywords` on the site, `limit: 300` (20): keywords at 4 to 20, which pages earn traffic, and the median `kd` of its top-10 keywords, its reach ([keyword floors](keyword-data.md#keyword-floors)).
- `seo_run_technical_crawl` sized to the site, `max_pages` up to 500 (15), read with `get_task`: `onpage_score`, `non_indexable`, `broken_links`, top `issues[]`.
- With Search Console (free): 90 days by page and by query, and the same 90 days a year earlier by date for clicks. These replace the estimates for the site; keep brand and non-brand apart.

## Gaps (about 55 credits)

- `seo_get_keyword_gap` per competitor (10 each): keep rows above the floors, count by `intent`, note the `competitor_url` pages carrying most.
- `seo_search_keywords` on the category the buyers search (10): `trend[12]` on the biggest terms for rising or falling demand.
- `seo_get_keyword_metrics` on "<competitor> alternative(s)" and "<product> vs <competitor>" for each competitor (10 for up to 100).
- `seo_get_serp` on the five biggest gap keywords (1 each): whether vendor pages rank or only publishers, forums and videos, and whether `features` include `ai_overview`.

## Choosing the work

Pick two to four, and say why each fits the numbers:

- **Technical first** when the crawl shows 5xx, noindex or canonical errors on pages that earn traffic, or more than about 5% of pages broken or non-indexable, or a migration falls inside the horizon: [technical-seo](../../technical-seo/SKILL.md). Nothing else works until pages can be indexed.
- **A fall already under way** (Search Console clicks down against the same 90 days a year earlier, near a [Google update](google-updates.md#confirmed-updates)): diagnose before planning, in [technical-seo](../../technical-seo/SKILL.md).
- **[Quick wins](quick-wins.md)** when 20 or more keywords rank 4 to 20 above the floors: the cheapest traffic there is.
- **[Refresh](refresh.md)** when older pages sit on page two or lost clicks; cannibalisation when two URLs rank for one main keyword.
- **[Content plan](content-plan.md)** then a [brief](briefs.md) per page when the gap holds clusters the site has no page for and an asset to anchor them.
- **[Comparison pages](comparison-pages.md)** when "alternative" or "vs" phrases for the competitors have volume.
- **Money pages** (pricing, product, top feature) below the top 3 for their keyword: the one-page checks in [quick wins](quick-wins.md#the-checks-in-order).
- **Links** when the site's `domain_rank` sits far below competitors': it will not win their head terms this quarter. Take the long tail under its reach and send the pages that need links to [link-building](../../link-building/SKILL.md).
- **AI answers** where `ai_overview` sits on most informational SERPs: weight commercial keywords higher here and send citation work to [ai-search](../../ai-search/SKILL.md).
- **[Tracking](tracking.md)** of the 10 to 20 keywords the plan targets, from day one.

What not to put in a 2026 plan: a page per keyword variant or city, self-ranking "best X" lists, date-only refreshes, or placements on big third-party sites to borrow their ranking ([policies](google-updates.md#policies-that-decide-what-not-to-build)).

## The 30-60-90 days

| Period | Typical work | KPI the tools re-measure |
|---|---|---|
| Days 1 to 30 | Technical blockers fixed, quick wins shipped, tracking baseline set | Fixes shipped; `non_indexable` and `onpage_score` on a re-crawl |
| Days 31 to 60 | First refreshes, first cluster or comparison pages published | Target keywords' positions; keywords each new URL ranks for |
| Days 61 to 90 | Second cluster, money pages optimised, links to the pages that need them | Search Console clicks (or estimated `organic_traffic`) and keywords in the top 3 and top 10 |

Give each period an owner and a target, and three first actions for this week. New pages take three to six months to settle, so the 30-day KPI is work shipped, and the 90-day KPI is rankings and clicks. Never promise a traffic number from estimates; set targets on Search Console clicks where connected. Size the page count to the team's hours.

## Deliverable

One document: inputs with defaults marked; a baseline table (site against each competitor: domain rank, estimated organic traffic, organic keywords, top-10 keywords, labelled as estimates); the gaps in three lines each; the chosen work with why; the 30-60-90 table; three first actions. Put the date, market and data source on it so a later run can compare.
