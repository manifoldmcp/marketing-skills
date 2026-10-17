# Diagnosing a traffic drop

A drop has a date, a place and a cause. Most "drops" brought to an agent in 2025 and 2026 are first a question of whether anything fell at all: Search Console changed what it counts twice in that period. Settle that, then date it, place it, and test the causes one at a time. Checked October 2026.

## Is it real?

Ask which number fell and where the user saw it. Clicks and sessions falling is a drop. Impressions or average position moving alone is often a measurement change.

- **Impressions fell, clicks held, position "improved".** The signature of a reporting change, not a ranking loss. Check the dates in [the timeline](timeline.md#reporting-changes-in-search-console) first.
- **Every channel fell on the same day** in Google Analytics (organic, direct, paid, email). The site or its tracking broke: a tag removed in a redesign, a consent banner change. Not Google.
- **Clicks fell, impressions held.** Real, but on the results page, not the index: an AI Overview, a new feature, a competitor's snippet, or the site's own title rewritten. Ranking higher does not bring those clicks back.
- **Analytics sessions against Search Console clicks** never match (consent, ad blockers, redirects). Compare when each line broke, never the totals.
- **Estimated traffic** (`organic_traffic`, `traffic`) is modelled monthly from rank, volume and a click curve. A 15% month-on-month move is within its noise; call it a drop only when measured data or live positions agree.

### The September 2025 impressions cliff

Around 10 to 12 September 2025 Google stopped honouring the `num=100` results parameter that rank trackers and scrapers used to pull 100 results in one request. Google called it "not an officially supported feature" and did not announce it.

- **In Search Console:** impressions fell and average position rose, with clicks unchanged. LOCOMOTIVE's analysis of 319 properties found 87.7% lost impressions and 77.6% lost unique ranking queries (reported by Search Engine Land, September 2025). The leading explanation, not confirmed by Google, is that bot requests for 100 results had been logging impressions at deep positions no person scrolled to. Reported mostly on desktop; in our own data mobile fell as much ([field notes](field-notes.md)).
- **In rank trackers and SEO databases:** getting results past page one now takes a request per page of ten, ten times the work for a top 100, so tools tracked less deeply or fewer keywords. Counts of keywords ranking past page two can step for collection reasons alone. A step in a third-party keyword count in September 2025 is not evidence of lost rankings until live positions confirm it.
- **What to do:** treat 12 September 2025 as a break in the impressions and position series. Compare clicks across it, never impressions or average position. Annotate it in Search Console (custom annotations exist since November 2025) and in any report.
- A second break: Google reports an impressions logging error from 13 May 2025 to 27 April 2026, also affecting CTR and position but not clicks. A year-over-year impressions comparison in 2026 crosses both.

## Date it and place it

- **Daily clicks over 16 months** (`console_get_search_analytics` with `dimensions: ["date"]`), then the same weeks a year earlier. A dip that recurs is seasonality; check Google Trends or `trend[12]` from `seo_get_keyword_metrics` for the keywords that lost.
- **The break's shape** says most about the cause (Google's own debugging guide, December 2025):

| Shape | Usual cause | What confirms it |
|---|---|---|
| A cliff on one day, site-wide | Site change (robots.txt, noindex, a redesign, a server or CDN rule), tracking, a manual action or security issue | The change log; Manual Actions and Security Issues reports; `seo_get_page` on top pages |
| A slide over one to three weeks, site-wide, starting on a dashboard date | A core or spam update | Start inside a rollout in [the timeline](timeline.md#ranking-updates); no site change; losses spread across many pages and competitors gain |
| A slow decline on some pages | Page-level fault that bites as Google recrawls (a stray `noindex`, a canonical pointing away), or content falling behind | `console_inspect_url` on the losers; what now ranks |
| The same dip every year | Seasonality | Year-over-year line; keyword trend |
| One page, one competitor now above | Content or intent, not a penalty | `seo_get_serp` on its keywords |
| Discover only | The February 2026 Discover update, or Discover's logging errors | `search_type: "discover"`; nothing in `web` |

- **Where:** pages, then queries per page, four weeks before against four after (`dimensions: ["page"]`, then `["query"]` with a page filter). Split by device and country: a fall in one narrows the cause at once. Split branded from non-branded (the Branded queries filter in Search Console, or a query filter with `operator: "excludingRegex"` and the brand in the API): a fall only in non-branded queries is rankings, a fall in branded ones is demand.
- **Google's advice after an update:** wait at least a full week after the rollout completes, then compare the week after with the week before it began. A small drop needs no drastic action; a pages-that-moved-down list is not a list of bad pages.

## Test the causes

One cause usually carries most of the loss. Check the site's own changes first: it explains most sudden drops and is the one cause no tool sees without the user.

1. **Demand.** Search Console year over year, or `seo_get_keyword_metrics` on the 20 keywords that lost most (10 credits): `trend[12]` falling in the same months means searchers left.
2. **Who took the positions.** `seo_get_serp` with `ai_overview: true` on the 10 biggest losers (2 credits each): a competitor that took several, a format change (lists, forum rows with `type: "discussions_and_forums_element"`, videos), or another URL of the user's own site. `seo_get_position` (6 credits) gives the current rank and `above[]` who is ahead.
3. **AI Overviews and features.** A non-null `ai_overview` on a query whose impressions held and clicks fell; check `ai_overview.references[]` for the site. Since August 2026 the Generative AI performance report in Search Console shows impressions in AI Overviews and AI Mode per page (impressions only, no clicks). What to do about it is in [ai-search](../../ai-search/SKILL.md).
4. **Technical breakage.** `seo_get_page` (free) on each losing page: `status`, `final_url`, `robots_meta`, `canonical`, and a `title` or `h1` that no longer matches what it lost. Several sections at once: a crawl ([audit](audit.md)). With Search Console, `console_inspect_url` on the five biggest losers. What each status means is in [indexing](indexing.md).
5. **Lost links.** `seo_get_backlink_summary` (10 credits) has no history: `broken_backlinks` above zero means links point at pages that now fail; removed links come from `seo_get_backlinks` with `status: "lost"`, each with `last_seen` for about when it went. Recovering them is [link-building](../../link-building/SKILL.md).
6. **An update.** Only after the above: the date falls inside a rollout, no site change explains it, and the loss is spread. Say what would confirm it (the next update's direction, competitors' movement), because a core update cannot be proven from one site's data.

## Deliver

A verdict in two lines, then a table: cause, evidence with its source (measured or estimated), pages affected, keywords affected, clicks lost, fix, owner. List the causes tested and ruled out, with the evidence, so nobody chases them again. Put the date, market and data source on it; the server keeps no history.

Recovery: a technical fix shows within days to weeks as pages are recrawled; a core update loss can take until the next core update, months. Say which the user has.

## Budget

Without Search Console about 195 credits (12-month history on the domain overview 71, ranked keywords 30, keyword trends 10, ten SERPs 20, five positions 30, a 500-page crawl 25, link summary 10); with it about 85, since the console tools are free. Say the estimate first and pass `max_credits`.

## Without the connector

In the Search Console UI: Performance, last 16 months, compare periods year over year, Pages tab sorted by clicks difference, then the Page indexing report and URL Inspection on the losers (its live test shows the page as Googlebot fetches it now). Read the [Status Dashboard](https://status.search.google.com/products/rGHU1u87FJnkP6W2GwMi/history) for update dates. Without Search Console, there is no reliable way to date a drop to the day; say so rather than inferring from third-party estimates.
