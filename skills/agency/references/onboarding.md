# Onboarding a client

The first weeks set two things: the numbers every later report is measured against, and the expectations the client will judge the agency by. Both decide whether the client is still there in a year. Checked October 2026.

## Why the first 90 days matter

- Of small businesses that dropped an agency, 56% did it 6 to 12 months in, and 60% gave lack of return as the reason (vcita, 500 US SMBs, April 2025). The first report that shows nothing moving is the riskiest moment.
- 24% of agencies name expectations misaligned at onboarding as a reason clients leave (AgencyAnalytics, 494 agency staff, 2026). Write down, in the onboarding document, what will and will not move in 30, 60 and 90 days.
- Clients leave over delivery and value more than budget ([retention and pricing](retention-and-pricing.md)). The baseline is what lets the agency show value later; a baseline missing the client's own conversions cannot.

## The tracked set

Fix it once and re-measure exactly it every month.

- **Domain, market, device.** One `location`, `language` and `device`, recorded with every number. A report run from another location compares nothing.
- **The one goal** the retainer is judged on: organic traffic, leads, AI answers, links, paid or social. Measure only the channels in the retainer: a baseline full of numbers nobody owns becomes a report of excuses.
- **Three competitors**: from the pitch, or the top three real businesses from `seo_get_serp_competitors` (10 credits).
- **10 to 30 keywords** the client can win within the retainer: a few head terms for the story, most ranking 4 to 30, where movement shows within months. Twenty keywords on page five show no progress for a year. To pick from `seo_get_ranked_keywords`, pull every row: they come by `traffic`, highest first, so keywords ranking 4 to 30 with little traffic yet sit deep in the list.
- **3 to 10 AI prompts**: buyer questions written with the client; the prompt method is in [ai-search](../../ai-search/SKILL.md).
- **The client's own data**: Search Console and GA4 access on day one (both free to read), and a named person who will supply leads, sales or revenue each month. Leads, sales and revenue sit in the client's CRM, which no tool here reads.
- **Add, never swap.** A keyword or prompt added later is a new row with its own start month. A number measured another way is a different number, not a change.

## The baseline (about 337 credits at defaults)

Only for the channels in the retainer:

- **Search**: `console_get_search_analytics` for the last 16 months if granted (free), which shows seasonality in real clicks; `analytics_get_report` for sessions and `keyEvents` by `sessionDefaultChannelGroup` (free). `seo_get_domain_overview` with `history: true` on the client (61) and without on each competitor (5 each); `seo_get_backlink_summary` on the client (10).
- **Rankings**: `seo_get_position` per tracked keyword (6 each): `rank` and `url`.
- **AI answers**: `aeo_run_ai_answers` with the prompts and all four brands (18 a prompt), then `get_task`; record per cell mentioned and cited. `aeo_get_site_readiness` (free).
- **Site health**: `seo_run_technical_crawl` with `max_pages: 500` (15), then `get_task`: `onpage_score`, `broken_links`, `non_indexable`.
- **Paid**: `ads_get_advertiser_ads` on Meta and Google (1 each): what runs and since when, not spend.
- **Social**: the profile tool per platform run (1 each): `followers`, `posts_count`.

Keep the 12 months before the engagement: without them the first seasonal dip reads as the agency's fault. Annotate the two breaks in search data ([search data](search-data.md#two-breaks-in-the-data)) if the 12 months cross them.

## Store it with the host

One record: the date, the tracked set, and every number with the tool and params that produced it. The host keeps it (a file in the client's folder, a sheet, project memory); if it cannot, give the user the record as a table to store and paste back. AI answer task results expire after 30 days, so store the mentions and citations themselves, not the `task_id`.

## The first 90 days

Open the plan from the topic that owns the goal, with the intake settled and the baseline already bought (repeat calls with the same params within a week are cached and free): [seo](../../seo/SKILL.md), [ai-search](../../ai-search/SKILL.md), [link-building](../../link-building/SKILL.md), [paid-ads](../../paid-ads/SKILL.md), [outbound](../../outbound/SKILL.md), a social platform's topic, or [growth-strategy](../../growth-strategy/SKILL.md) when there are several goals or none.

| Period | Agency work | What the client sees |
|---|---|---|
| Days 1 to 30 | Access granted, baseline stored, fixes that block everything (crawl errors, AI crawlers blocked, tracking broken), the plan agreed | The baseline document and a one-page plan; the first report date |
| Days 31 to 60 | The plan's first work shipped and logged | First monthly report: work done beside leading indicators (rank moves on keywords 4 to 30, pages indexed, AI mentions), honest that outcomes lag |
| Days 61 to 90 | Second round of work; first re-measure against the baseline | Second report and a 90-day review: what moved, what did not and why, the next quarter's plan |

## Setting expectations in writing

- Rankings move over months; new links reach the backlink index weeks late; AI answers vary from run to run, so the AI number is a mention rate with a margin of error.
- Clicks can fall while visibility holds, because AI Overviews take clicks ([search data](search-data.md)). Agree now which outcome counts: the client's leads or conversions, not raw traffic.
- Agree the report cadence and format with the client. Monthly is the default (69% of agencies), but a call carries as much weight as a static report: about a third of clients prefer each of static reports, calls and live dashboards (AgencyAnalytics, 2026).

## Deliver

One onboarding document: the tracked set, a baseline table (metric, client, each competitor, tool, date), the 12-month line, the 90-day table above filled in for this client, the expectations in writing, and the date of the first report. Never send it; offer the host's document or email tool.
