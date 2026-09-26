# Brand awareness

Awareness is how many buyers know the brand before they need it. The tools cannot survey people, but they measure what awareness leaves behind: people searching the brand name, AI answers naming it, reach on social, other people talking about it, and coverage linking to it. This playbook measures the brand against competitors on each, finds the weakest, and routes to the groups that move it.

## Inputs to settle first

- **Brand**: the name, its variants and the domain. If the name is a common word, add the qualifier people use ("linear app").
- **Competitors**: two or three the user wants to be named alongside.
- **Audience**: who should know the brand, so the platforms and questions fit them.
- **Channels**: what the team can do (video, writing, press, events, paid). Default: writing and founder posts.
- **Horizon**: default 90 days. Branded search moves slowly, so the first real read is at day 90.
- **Budget**: about 6 + 54 + 16 + 4 + 8 + 40 = 128 credits for the brand and three competitors. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Branded search.** `seo_get_keyword_metrics` with the brand and each competitor's name as `keywords` (about 6 credits). Read `volume` and `trend[12]`. The brand's share of branded search is its volume over the sum; the 12-month trend says whether the gap is closing.
2. **Share of voice in AI answers.** `aeo_run_ai_answers` with three questions a buyer in the category would ask, and `brands` set to the brand and the competitors (18 credits a prompt), then `get_task`. For each brand, the share of the 15 cells (prompt by engine) where it is mentioned. The full measurement is [visibility check](../../ai-search/references/visibility-check.md).
3. **Social reach.** The profile tools for each brand on the platforms the audience uses (`linkedin_get_company`, `instagram_get_profile`, `tiktok_get_profile`, `youtube_get_channel`, `twitter_get_profile`; 1 credit each): `followers` and `posts_count`.
4. **Other people talking.** `reddit_search_posts` with each brand name at the default relevance sort (1 credit each), and `youtube_search_videos` and `tiktok_search_videos` with each name and `since: "year"` (1 credit each). Count the rows from the last 12 months that are about the brand and not by it. Use the same query shape for every brand, since each search is a ranked sample (the [reddit router](../../reddit/SKILL.md) explains).
5. **Coverage.** `seo_get_backlink_summary` on each domain (10 credits each): `referring_domains`, the trace press and mentions leave.
6. **Choose the levers.** For the two weakest measures against the competitors, and what the team can do:
   - Press coverage and mentions: [PR strategy](../../link-building/references/pr-strategy.md).
   - AI answers: [AI search strategy](../../ai-search/references/strategy.md).
   - Reach through other people's audiences: [influencers strategy](../../influencers/references/strategy.md).
   - The brand's own social: the [LinkedIn](../../linkedin/references/strategy.md), [TikTok](../../tiktok/references/strategy.md), [Instagram](../../instagram/references/strategy.md) or [YouTube](../../youtube/references/strategy.md) strategy, with [channels](../../content/references/channels.md) to choose among them.
   - Paid reach: [paid ads strategy](../../paid-ads/references/strategy.md).
   - Keeping count: [brand mentions](../../monitoring/references/brand-mentions.md) and [AI visibility tracking](../../monitoring/references/ai-visibility-tracking.md).
7. **Deliver** a share-of-voice table (brand, branded searches a month and the 12-month trend, AI mentions out of 15, followers per platform, Reddit posts and videos about it in a year, referring domains), the two weakest measures with the reason, the chosen levers with the linked playbook, the KPIs to measure again at day 90 (the same calls, questions and competitors), and three first actions.

## Judgment

- Branded search is the cleanest awareness measure the tools have: nobody searches a name they do not know. It is monthly and slow; read it at day 90, not day 30.
- Followers measure a brand's own reach, not awareness. Mentions by others and branded search say more.
- Share of voice needs a fixed set: the same competitors, questions and queries every time, or the numbers move for the wrong reason.
- A common-word brand inflates its branded volume with unrelated searches. Use the qualified form, and check with `seo_get_serp` on the name (1 credit) that the results are about the brand.
- Awareness with nowhere to land leaks. Before paying for reach, make sure the brand's own search results (site, reviews, profiles) answer someone who looks it up.
- Counts from Reddit, TikTok and YouTube search are ranked samples, not totals. Compare brands only with the same queries run on the same day.
