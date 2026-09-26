# SEO strategy

A strategy answers three questions before anyone writes a page: what the site already wins, where competitors get the organic traffic the site does not, and which of this group's playbooks gets the most traffic for the hours the team has. It ends in a 90-day plan, not a keyword list.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: what organic search should bring (default: signups from commercial and transactional keywords) and how success is measured (default: estimated organic traffic and keywords in the top 10, or clicks if Search Console is connected).
- **Stage**: a new site with almost no rankings, or an established one. `seo_get_domain_overview` in step 2 answers it if the user does not know.
- **ICP**: who buys, so the keywords are the words those people search with.
- **Budget**: credits for the research (this playbook costs about 110) and writing hours per week. Default: 1,000 credits and one new or refreshed page a week.
- **Team**: who writes, and whether anyone can change templates, redirects and page speed. Default: the founder writes; no developer time.
- **Competitors**: two or three that sell the same thing. Default: the top three from `seo_get_serp_competitors` that are real businesses, not publishers or marketplaces.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults. Check for the `console_*` tools as the [router](../SKILL.md#search-console) says: with Search Console the site's own numbers are measured, without it every number is an estimate.
2. **Baseline.** Say the cost first: about 55 credits for the site and three competitors.
   - `seo_get_domain_overview` on the site and each competitor (5 credits each): `domain_rank`, `organic_traffic`, `organic_keywords`, `positions` and `top_pages`.
   - `seo_get_ranked_keywords` on the site with `limit: 300` (20 credits): how many keywords rank 4 to 20, which pages earn the traffic, and the median `kd` of its top-10 keywords, which sets its reach (the [keyword floors](../SKILL.md#keyword-floors)).
   - `seo_run_technical_crawl` with `max_pages` sized to the site, up to 500 (15 credits), then `get_task` (free) after `poll_after_s`: `onpage_score`, `non_indexable`, `broken_links` and the top `issues[]`.
   - With Search Console: `console_get_search_analytics` with `dimensions: ["page"]` and then `dimensions: ["query"]` for the last 90 days (free), which replaces the estimates for the site.
   - `seo_get_serp_competitors` on the site (10 credits) only if the user named no competitors.
3. **Gaps.** About 55 credits more.
   - Keyword gap: `seo_get_keyword_gap` with each competitor (10 credits each). Keep the rows that clear the floors, count them by `intent`, and note the `competitor_url` pages that carry the most.
   - Market: `seo_search_keywords` on the category the ICP searches for (10 credits). Read `trend[12]` on the biggest terms for rising or falling demand.
   - Comparison demand: `seo_get_keyword_metrics` on "<competitor> alternative" and "<competitor> vs" for each competitor (10 credits for up to 100 phrases).
   - Format: `seo_get_serp` on the five biggest gap keywords (1 credit each). Note whether vendor pages rank or only publishers, forums and videos, and whether `features` includes `ai_overview`.
4. **Tactics.** Choose two to four from this group and say why each fits the numbers:
   - [Audit](audit.md) first when the crawl shows 5xx pages, noindex or canonical errors on pages that earn traffic, or more than about 5 percent of pages broken or non-indexable. Nothing else works until pages can be indexed.
   - [Quick wins](quick-wins.md) when 20 or more keywords rank 4 to 20 above the floors: the cheapest traffic there is.
   - [Traffic drop](traffic-drop.md) when the user reports a fall, or the history shows one.
   - [Content refresh](content-refresh.md) when older pages rank on page two or lost traffic.
   - [Content plan](content-plan.md), then a [brief](brief.md) per page, when the keyword gap holds clusters the site has no page for.
   - [Comparison pages](comparison-pages.md) when "alternative" or "vs" phrases for the competitors have volume.
   - [Optimize a page](optimize-page.md) for each money page (pricing, product, top feature) that ranks below the top 3 for its keyword.
   - [Migration](migration.md) when a redesign, replatform or domain change falls inside the horizon: it comes before everything else.
   - [Rank check](rank-check.md) to record the starting positions of the 10 to 20 keywords the plan targets.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week. A typical shape: by day 30, technical blockers fixed and quick wins shipped; by day 60, the first refreshes and the first cluster published; by day 90, the second cluster and the money pages optimized. Size the page count to the team's writing hours.
   - KPIs the tools can measure again later: estimated `organic_traffic` and `positions` (top 3, top 10) from `seo_get_domain_overview` (5 credits); the positions of the target keywords with `seo_get_position` (6 credits each); the keywords each new page ranks for with `seo_get_ranked_keywords` on its URL (10 credits); `onpage_score` and `non_indexable` from a second crawl; clicks and impressions by page when Search Console is connected. For weekly measurement, the `monitoring` group's [rank tracking](../../monitoring/references/rank-tracking.md).
   - **Deliver** one document: the inputs with defaults marked, a baseline table (site against each competitor: domain rank, estimated organic traffic, organic keywords, keywords in the top 10), the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table (period, work, owner, KPI, target), and the three first actions.

## Judgment

- SEO moves over months. New pages take three to six months to settle; the 30-day KPI is work shipped and technical fixes, the 90-day KPI is rankings and traffic.
- A site whose domain rank sits far below its competitors' will not win their head terms this quarter. Take the long tail under its reach, and send the pages that need links to the `link-building` group.
- The pace comes from the team, not the keyword list. One good page a week beats five thin ones.
- Estimated traffic is directional. Compare the site with its competitors on the same estimate, and never promise the user a traffic number from it.
- Where `ai_overview` sits on most informational SERPs, clicks to those pages will be lower than the volume suggests. Weight commercial keywords higher, and point the user to the `ai-search` group for being cited in the answer.
- If the baseline shows a site that cannot be indexed, the whole plan is the audit until that is fixed.
