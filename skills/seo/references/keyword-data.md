# Keyword data and what a ranking is worth

What the `seo_*` numbers really are, where they mislead, the floors that came from real use, and how many clicks a position earns now that AI Overviews sit on most informational results. Checked October 2026.

## Volume

- **It is Google Keyword Planner's number.** In our test the monthly `trend[12]` from the default Labs source matched Keyword Planner (`source: "google_ads"`) value for value, one month behind, and both came in Planner's fixed buckets (10, 20, 30, 40, 50, 70, 90, 110, 140, 170, 210, 260, 320 ...) ([field notes](field-notes.md)). The headline `volume` differs between the two because their twelve-month windows differ. Treat any volume as a bucket, not a count: 70 and 90 are the same evidence.
- **Null means no data, never zero.** Each source had nulls the other filled: Labs had none for two long-tail "alternatives" phrases, Keyword Planner had none for five new product terms Labs put at 90 to 480. When a high-intent keyword comes back null, try the other source (24 credits flat) or judge it by intent.
- **New and fast-growing terms lag.** The trend shows the past twelve months, ending a month or more ago. For a category that did not exist a year ago, `trend[12]` rising matters more than the average.
- **Summed volumes overstate the prize.** Variants of one query share searchers, and Google groups close variants. A page earns a share of its cluster, not the total.
- **Search Console impressions are not volume.** In one case in our data a page averaging position 3.7 got 22 US impressions in a month on a query Planner put at 110; impressions depend on position, device, country and whether anyone scrolls. Use impressions to rank the user's own opportunities, and volume to compare keywords the site does not rank for yet.

## Difficulty and reach

`kd` (0 to 100) models how many links a page needs to rank. It says nothing about intent or content. Read it against what the site already wins: the median `kd` of its top-10 keywords in `seo_get_ranked_keywords` is its reach.

## Estimated traffic

`traffic` per keyword and `organic_traffic` per domain are rank times volume times a click curve, from an index refreshed on the provider's schedule. On a small site of our own the estimate was 14 times the clicks Search Console recorded, almost all of it from one page that had since lost its US rankings ([field notes](field-notes.md)). Estimates are fit for comparing sites on the same model and for direction, never for promising the user a number. `keyword_changes` (new, up, down, lost) shows how much the index moved at its last update; a large `lost` means the snapshot is stale for that site.

## Keyword floors

These came from running the old job skills on real sites; keep them unless the user's data says otherwise.

- **Striking distance** is rank 4 to 20. At 1 to 3 little rank is left to gain (CTR may be); beyond 20 a page needs new content, not a fix.
- **Volume.** Skip keywords under about 50 a month, except high intent ones (a competitor's name with alternative, vs or pricing; "X software for Y"), where 20 buyers beat 2,000 browsers.
- **Difficulty.** Aim at or below the site's reach, and up to about 10 above it for pages that will get links. A site with no top-10 rankings starts under KD 20.
- **Intent.** When the goal is signups or sales, commercial and transactional `intent` come first; informational keywords earn reach and feed the money pages. Another brand's navigational keywords are off limits except on comparison pages.
- **Format.** The format of Google's top three (list, guide, tool, product page, forum thread, video) is the format to match. A page in the wrong format does not rank by adding words.
- **One page per intent.** Google's AI optimisation guide (updated July 2026) says outright that a page per query variation is scaled content abuse and that its systems understand synonyms. Two keywords share a page when three or more of their top 10 URLs are the same (`seo_get_serp`, 1 credit each).

## Clicks per position

Advanced Web Ranking, Google US, over 1.75M SERPs a month, July 2026 (published September 2026):

| Desktop position | CTR, all SERPs | CTR with an AI Overview |
|---|---|---|
| 1 | 20.0% | 9.7% |
| 2 | 10.4% | 3.7% |
| 3 | 3.9% | 1.7% |
| 4 | 1.7% | 1.3% |
| 5 | 1.1% | 1.0% |
| 6 to 10 | 0.5% to 0.7% | 0.3% to 0.6% |

Mobile position 1 was 35.7%, and 12.0% under an AI Overview.

- **The top three take nearly all the clicks.** Positions 1 to 3 add up to about 34% CTR on desktop; 4 to 10 together to about 6%. A move from 8 to 5 is worth little; a move from 5 to 3 roughly triples clicks; 3 to 1 is about five times.
- **AI Overviews halve the top of the curve.** Ahrefs found position-one CTR on informational keywords with an overview 58% below what it would have been without one (300K keywords, December 2025 against December 2023), up from 34.5% a year earlier. Seer Interactive's panel (53 brands) put organic CTR on overview queries at 2.36% in February 2026 against 3.82% without one, and found being cited in the overview roughly doubles CTR against not being cited.
- **Clicks below volume.** Where `serp_features[]` or `features` include `ai_overview` on most of a cluster's results, expect well under half the clicks the curve above gives at the same position. Weight commercial keywords, which show overviews less, and send citation work to [ai-search](../../ai-search/SKILL.md).
- **Brand queries sit on another curve.** Navigational CTR at position 1 is far higher; never mix brand and non-brand when reading a site's CTR.

## Reading `rank`

`rank` in `seo_get_position` and `seo_get_serp` counts every item on the page, features included; `organic_rank` counts organic results only. A `rank` of 7 can be the third organic result under an overview and a video pack. Trend `organic_rank`; report both where they differ. Null means not in the top 100 scanned.
