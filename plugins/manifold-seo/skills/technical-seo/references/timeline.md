# Dated events that move the numbers

Google updates, Search Console reporting changes and SERP changes from January 2025 to October 2026, with what each does to the data. Match a break in the user's line against this table before calling it a drop. Checked 15 October 2026; the authoritative lists are Google's [Search Status Dashboard](https://status.search.google.com/products/rGHU1u87FJnkP6W2GwMi/history) for ranking updates and Search Console's [data anomalies page](https://support.google.com/webmasters/answer/6211453) for reporting faults. Read both for anything after this date.

## Ranking updates

Start date and length of the rollout, from the Status Dashboard (US Pacific time).

| Update | Started | Rollout |
|---|---|---|
| March 2025 core update | 13 Mar 2025 | 14 days |
| June 2025 core update | 30 Jun 2025 | 17 days |
| August 2025 spam update | 26 Aug 2025 | 27 days |
| December 2025 core update | 11 Dec 2025 | 18 days |
| February 2026 Discover core update | 5 Feb 2026 | 22 days (Discover only) |
| March 2026 spam update | 24 Mar 2026 | under a day |
| March 2026 core update | 27 Mar 2026 | 12 days |
| May 2026 core update | 21 May 2026 | 12 days |
| June 2026 spam update | 24 Jun 2026 | 2 days |
| August 2026 spam update | 18 Aug 2026 | 3 days |
| September 2026 spam update | 24 Sep 2026 | 14 days (ended 8 Oct) |

- Core updates came two to six months apart in this period. Google says recovery can take "several months" and may mean waiting for the next core update, and that smaller core updates now run between the named ones unannounced (core updates doc, December 2025). An unexplained move on no listed date can still be an update.
- The February 2026 update touched Discover, not web search. A publisher whose Discover traffic fell sees nothing in the `web` search type.

## Reporting changes in Search Console

These change the numbers without changing anything real.

| Date | Change | What it does to the data |
|---|---|---|
| 13 May 2025 to 27 Apr 2026 | Logging error on impressions | Impressions, CTR and average position misreported for almost a year; clicks unaffected. Google says impressions may show a decrease. Never compare impressions across 27 April 2026 without saying so. |
| 16 Jun 2025 | AI Mode counted in Performance totals | AI Mode impressions and clicks sit inside `web`, with no filter in the main report. |
| 9 Sep 2025 | Docs removed for course info, estimated salary, learning video, special announcement and vehicle listing markup | Those rich results stop; their search appearance rows fall to nothing. |
| about 10 to 12 Sep 2025 | Google stops honouring `num=100` | Desktop impressions fall sharply, average position "improves", clicks hold. See [traffic drops](traffic-drops.md#the-september-2025-impressions-cliff). |
| Nov 2025 (all eligible sites by Mar 2026) | Branded queries filter | Splits branded from non-branded with an AI classifier (typos and product names included). Domain-level properties only, enough volume needed; not named for the API. |
| Dec 2025 | Weekly and monthly views | Smoother lines for spotting a break. |
| 28 Feb and 1 Mar 2026 | Bulk data export days missing | Not recoverable for some properties. |
| 7 May 2026 | FAQ rich results stop appearing | FAQ appearance impressions drop to zero; the pages themselves are unaffected. |
| 3 Jun 2026 (all sites 31 Aug 2026) | Generative AI performance reports | Separate impressions view for AI Overviews, AI Mode and Discover's AI features, by page, country, device and date. Impressions only; the data stays inside the main totals too. |
| 2026, several days | Discover logging errors | 7 to 8 May, 21 May, 24 June, 13 August 2026: Discover clicks and impressions under-reported. |

## Crawling and rendering documentation

| Date | Change |
|---|---|
| Dec 2025 | Pages with a non-200 status may not be rendered; `noindex` handled through JavaScript is "not well defined"; canonical rules for JavaScript sites; crawl budget, faceted navigation, status code and DNS docs moved to Google's crawling site. |
| 3 Feb 2026 | Googlebot's fetch limit documented as 2MB per file (PDFs 64MB); 15MB is now the default for Google's other crawlers. |
| Jun 2026 | Site move guide: submit Change of Address for every variant of the old domain. |
| 10 Jul 2026 | Canonical troubleshooting: re-evaluation can take up to two weeks. |

Detail and what to do about each is in [indexing](indexing.md) and [migrations](migrations.md).
