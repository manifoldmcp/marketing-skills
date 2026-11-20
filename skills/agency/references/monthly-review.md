# Monthly client review

The monthly document answers three questions for the client: what moved, why, and what happens next month. It re-measures the tracked set from [onboarding](onboarding.md) with the same calls and params, sets it against last month and the baseline, and hands the tables to the host to lay out and send. Checked October 2026.

## What clients want from it

Agency-side evidence only: no survey asks clients directly (AgencyAnalytics, 494 agency staff, February to April 2026, unless noted).

- **Outcomes first.** Clients track conversions (44%) and leads (22%) most closely; traffic 6%. Their top questions: does marketing connect to revenue (55%), how is AI used in the work (52%), why is organic traffic falling (42%), why are ads costing more (37%), how reliable is the data (35%). A report that opens on sessions answers none of them.
- **The relationship outweighs the format.** Agencies rank strong relationships (80%) and communication (69%) as what keeps clients, far above transparent reporting (32%). Clients split about evenly between a static report (35%), a call (35%) and a live dashboard (27%). Walk the client through it; do not only send it.
- **Accuracy is the floor.** 76% of agencies call accurate reporting extremely important to retention. One wrong number found by the client costs more than any chart earns.
- **Expectations moved.** 44% of agencies say clients expect faster turnaround and 36% more proactive recommendations. End with decisions for the client, not a recap.
- **Show what the agency did.** Clients leave over delivery and value (Setup, January 2026; see [retention and pricing](retention-and-pricing.md)). The tools see results, not work: put the work log beside the results every month.

## Inputs

- **The stored record** from onboarding and every month since. Without a baseline there is nothing to compare: onboard now, and this month becomes the baseline.
- **Period**: default the last full calendar month, measured on the 3rd or later (Search Console lags about two days).
- **Work done** in the month: pages, fixes, links, campaigns, from the agency.
- **Channels**: the ones in the retainer. Measure nothing else.

## Re-measure (about 343 credits per client per month)

At 20 keywords, 5 prompts and 3 competitors; each extra keyword adds 6 credits a month, each extra prompt 27. The same calls as the baseline with the same params:

- **The client's own numbers, first**: `console_get_search_analytics` this month and last (free): clicks, impressions, CTR, position. `analytics_get_report` with sessions, `keyEvents` and revenue by `sessionDefaultChannelGroup`, against last month and the same month last year (free). Leads and revenue from the client's CRM go in rows the client fills.
- **Estimates, labelled**: `seo_get_domain_overview` on the client and each competitor without history (15 each); the stored months are the history.
- **Rankings**: `seo_get_position` per keyword (6 each); note when `url` differs from last month.
- **Links**: `seo_get_backlink_summary` (10), and `seo_get_referring_domains` for rows with `first_seen` in the month (won), and again with `status: "lost"` for domains that dropped a link (lost): 12 credits each for the default 100 rows, sorted by the rank each passes to the site rather than by the domain's own, so pass `limit: 1000` (25 credits) when `referring_domains` is over 100, or the new small ones are missed. Count won domains only when they clear the quality floors in [link-building](../../link-building/SKILL.md). The index lags the web by weeks: links built late in the month may show next month.
- **AI answers**: `aeo_run_ai_answers` with the tracked prompts and brands (27 a prompt), then `get_task`.
- **Paid and social** when in the retainer: `ads_get_advertiser_ads` and the profile tools (1 credit each).
- **AI crawler access**: `aeo_get_site_readiness` (free). A check with `tier: "blocks_citations"` that now shows `ok: false` (often a CDN or firewall change) is the first line of the review.

## Real move or noise

Call a move real only when it clears the noise; anything smaller is "flat":

- a keyword moving 3 or more positions, or crossing into or out of the top 3 or top 10 (`seo_get_position` is one day's reading);
- estimated traffic moving 10% or more (and even then it can be the model);
- AI mentions moving 3 or more cells of 25, or a smaller move that holds for three months;
- Search Console clicks: compare with the same month last year as well as last month, because seasonality is larger than most monthly moves.

## Explain each real move

Tie it to the work log or to an outside cause, and say so when the tools cannot tell:

- **A Google update in the month.** No tool reads it: check Google's Search Status Dashboard and name the update when the dates match.
- **Clicks down, rank held.** An AI Overview above the client: `seo_get_serp` with `ai_overview: true` on the top keywords (2 credits each) shows whether one now shows and whether it cites them ([search data](search-data.md)).
- **A competitor's jump**: its overview and the keywords it gained.
- **A ranking page changed**: `url` from `seo_get_position` differs from last month's, often cannibalisation or a migration.
- **Seasonality**: the baseline's 12-month line and last year's same month.
- **A data break**: the September 2025 impressions step and the May 2026 GA4 channel ([search data](search-data.md#two-breaks-in-the-data)).

## Deliver

Tables for the host to lay out:

- **Summary**: the outcome metric against target, three wins, one problem, next month's three actions, and one decision the client needs to make.
- **KPIs**: metric, baseline, last month, this month, same month last year, change, note. Client's own numbers first; estimates labelled.
- **Work done**: what shipped, linked to the rows it should move.
- **Rankings**: keyword, volume, baseline rank, last month, now, ranking URL.
- **AI answers**: prompt, engines naming the client at baseline and now, competitors named, with the three-month mention rate.
- **Links**: domain, domain rank, won or lost.

Show losses beside wins: the client finds a drop on their own, and a hidden one costs their trust. For each problem, name the topic that fixes it ([seo](../../seo/SKILL.md), [ai-search](../../ai-search/SKILL.md), [technical-seo](../../technical-seo/SKILL.md) and the rest). Store this month's numbers with the host for next month. To produce it monthly without being asked, the host schedules it; the server schedules nothing. Never send it; offer the host's document or email tool.

## Without the connector

The client's own Search Console and GA4 exports carry most of the review. Check the tracked keywords by hand in a private window from the agreed location, ask the AI prompts logged out on each engine, and say which rows were not measured this month rather than leaving them blank.
