# Reels trends

What is rising on Reels in a niche right now: formats, topics and lengths that several accounts are using this week and that beat their own usual numbers. A dated table, each trend with example reels and a line on how the user could use it.

## Method

1. **Pull recent reels.** `instagram_search_posts` for three or four niche tags with `since: "week"`, two pages each (1 credit a page). Keep `media: "video"`; drop `is_ad: true`, duplicates across tags and captions in other languages (search has no country filter). A tag with fewer than about 20 reels in a week is too thin: rerun with `since: "month"` and say the window changed.
2. **Measure speed:** views per day since `created_at`, and rank on it. Take views from the author's listing when the search row shows null or looks low; search rows lag.
3. **Separate the trend from the account.** For the authors of the 10 fastest reels, one page of `instagram_get_reels` (1 credit each) and each reel's multiple of its author's median. At 3 times or more, the reel was lifted by what it did; a large account posting at its usual numbers is no evidence of a trend.
4. **Name the format** with `instagram_get_transcript` on the 10 strongest outliers (1 credit each) and the caption: talking head, voiceover over footage, tutorial, list, before and after, day in the life, POV or skit, or no speech (`NoData`). Reels over two minutes have no transcript; name their format from the caption.
5. **Group** the outliers by format, by topic (recurring words and tags in `text`), by length (`duration_s` under 15 seconds, 15 to 60, over 60) and by sound (`music` in the search rows: one track across several outliers is a sound trend). A group is a trend when it holds three or more reels from three or more accounts in the window.
6. **Deliver:** trend, what it is, reels, accounts, median views, median views a day, median multiple, two example links with their first line, how the user could use it, and the date pulled. About 28 credits for four tags.

## Judgment

- One viral reel is not a trend; three accounts beating their own medians with the same thing is.
- Trends fade within one to three weeks. Date the table; a trend whose oldest example is two weeks old is probably at its peak.
- The trend is the format or topic, never the reel. Copying the reel itself is unoriginal content under the April 2026 rules and can cost the account its recommendations (see [ranking](ranking.md#originality)); a Remix that transforms the source is the allowed way to join someone else's.
- Business accounts get a narrower music library than personal ones, so a trend that rides a popular song may be closed to a brand. Flag trends that depend on the audio and have the user check the track in the app.
- Hashtag search sees only tagged posts, and the five-tag cap means fewer tags per post: the sample leans to accounts that still tag. Say so.
- When the user also wants TikTok, run that read through [tiktok](../../tiktok/SKILL.md) and keep the tables apart: views compare only within one platform.
- For the same read every week, the host schedules it and keeps the previous table; the server keeps no state.
