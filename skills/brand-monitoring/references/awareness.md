# Measuring awareness against competitors

Awareness is how many buyers know the brand before they need it. Without a survey panel the tools measure what awareness leaves behind: people searching the name, AI answers naming it, other people talking about it, coverage linking to it, and the brand's own reach. This is the scorecard, how to read it, and a 90-day plan for the weakest measures. Checked October 2026.

## The scorecard

Measure the brand and two or three competitors the user wants to be named alongside, with the same calls on the same day.

| Measure | Call | What it reads | Trap |
|---|---|---|---|
| Branded search share | `seo_get_keyword_metrics`, `ai_volume: true` (about 11 credits for a dozen names) | Share of the set's `volume`, with `trend[12]` | Buckets, lag, common-word names ([share of search](share-of-search.md)) |
| Own branded impressions | `console_get_search_analytics`, query filter on the brand (free) | Daily trend for the user only | Undercounts rare queries; no competitors |
| AI share of voice | `aeo_run_ai_answers`, three buyer prompts, `brands` set to all of them (27 credits a prompt) | Share of answers naming each brand | A small set is noisy; method in [ai-search](../../ai-search/SKILL.md) |
| Other people talking | `reddit_search_posts`, `youtube_search_videos`, `tiktok_search_videos` per name with `since: "year"` (1 credit each) | Rows in a year about the brand and not by it | Ranked samples, fuzzy matching ([monitoring](monitoring.md#reading-the-rows)) |
| Coverage | `seo_get_backlink_summary` per domain (10 each) | `referring_domains` | Slow; reflects years, not quarters |
| Own reach | `linkedin_get_company`, `instagram_get_profile`, `tiktok_get_profile`, `youtube_get_channel`, `twitter_get_profile` (1 each) | `followers`, `posts_count` | Reach the brand owns, not awareness |

About 160 credits for the brand and three competitors. Say the estimate before the first paid call.

## Reading it

- **Branded search and AI share of voice are the two that measure awareness.** Branded search says buyers know the name; AI share of voice says the engines buyers ask know it, and brands ChatGPT recommends see the visit come through branded search (Similarweb, June 2026). Mentions by others and coverage are the inputs that move both. Followers are the brand's own reach and the weakest proxy.
- **Find the two weakest measures against the competitors,** not against an absolute bar. A brand at 5% of branded search in a set where the leader has 70% has a salience problem no single channel fixes in 90 days; a brand at parity on search but absent from AI answers has a narrower, faster problem.
- **Fix the landing before buying reach.** Someone who hears of the brand searches it. The brand's own page one (site, reviews, profiles) and its AI answers have to answer them; check with `seo_get_serp` on the name and a brand prompt on `aeo_run_ai_answers` before paying for reach.
- **Same set, every time.** The same competitors, prompts, queries, location and qualifiers, or the numbers move for the wrong reason. The host stores the table; the server keeps no history.
- **Branded search moves slowly.** Expect the first real read at day 90, and treat a one-bucket move as the smallest visible change.

## Choosing the levers

Match the two weakest measures to what the team can do (video, writing, press, events, paid):

- Press coverage and mentions: [digital-pr](../../digital-pr/SKILL.md).
- AI answers: [ai-search](../../ai-search/SKILL.md).
- Other people's audiences: [creators](../../creators/SKILL.md).
- The brand's own social: [linkedin](../../linkedin/SKILL.md), [tiktok](../../tiktok/SKILL.md), [instagram](../../instagram/SKILL.md), [youtube](../../youtube/SKILL.md), with [growth-strategy](../../growth-strategy/SKILL.md) to choose among channels.
- Paid reach: [paid-ads](../../paid-ads/SKILL.md). Broad-reach media moved share of search more than targeted media in the IPA's think tank data (2021).
- Communities: [reddit](../../reddit/SKILL.md).

## The 90 days

| Period | Typical work | KPI to re-measure |
|---|---|---|
| Days 1 to 30 | Scorecard baseline; own page one and brand AI answers fixed; mention monitoring set up ([monitoring](monitoring.md)); first lever started | Brand prompt answers correct; monitoring running with `coverage[]` complete for watched communities |
| Days 31 to 60 | Levers running for the two weakest measures; mentions by others logged weekly | Mentions by others per week, against the baseline on the same queries |
| Days 61 to 90 | Re-run the scorecard with the same calls and set | Branded search share (three-month average), AI share of voice with its margin, referring domains |

Give each period an owner and three first actions for this week. A geo holdout, where the lever allows one, turns the day-90 read from a before-and-after into a measured lift ([brand lift](brand-lift.md)).

## Setting targets honestly

- Promise inputs (coverage earned, creators booked, AI answers corrected), not a share of search. Share of search follows inputs over quarters, and its lead time for B2B software has not been published.
- Set a branded search target at least one bucket above today, or it cannot be seen.
- On AI share of voice, a target smaller than the margin of error on the prompt set is noise ([ai-search](../../ai-search/SKILL.md)).
