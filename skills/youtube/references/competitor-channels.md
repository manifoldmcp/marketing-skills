# Competitor channels

What a competitor's YouTube channel publishes, how often, what earns views and what those videos do that the user's do not. It ends in a comparison table and the gaps the user can take, not in a list of every video.

## Inputs to settle first

- **Competitors**: two or three channels, by handle. If the user gives company names, `youtube_search_videos` with the brand name (1 credit) and take the channel whose `author_name` matches; a channel that only reviews the brand is not theirs.
- **The user's channel**: the handle, to compare against. Optional.
- **Window**: the latest 20 videos for the current strategy, plus the most viewed for what has worked over time.
- **Budget**: a default run costs at most 3 x (1 + 2 + 1 + 8 + 3) + 3 = 48 credits for three competitors and the user. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Size each channel.** `youtube_get_channel` on each competitor and the user (1 credit each): subscribers (`followers`), total `views`, video count (`posts_count`) and when it joined (`created_at`).
2. **Read the current strategy.** `youtube_get_videos` with the default sort (1 credit a page), with a second page if the first holds fewer than 20 videos. From them: uploads per month over the last 90 days, the median views of the latest 10, the median as a share of subscribers, the length mix from `duration_s` (under 10 minutes, 10 to 30, over 30), and the topics and formats from the titles (tutorial, comparison, customer story, webinar recording, podcast, product update).
3. **Read what has worked.** `youtube_get_videos` with `sort: "popular"` (1 credit) for the all-time hits. Mark the outliers against the median from step 2 per the [outlier rule](../SKILL.md#channel-health), and note whether they are recent or years old.
4. **Check engagement on the shortlist.** `youtube_get_video` on the top five outliers and three recent videos (1 credit each): likes and comments per 1,000 views. Apply the [engagement rule](../SKILL.md#channel-health): high views with almost no likes often means ad-driven views. `is_ad: true` marks a video that declares a paid promotion.
5. **Read why the best ones work.** `youtube_get_transcript` on the top three outliers (1 credit each). Note the hook in the first 30 seconds (the first 80 words or so), the structure, and the call to action (a trial, a template, a demo booking).
6. **Deliver** a table with one column per channel, the user's included: subscribers, videos, uploads per month (last 90 days), median views of the latest 10, median as a share of subscribers, length mix, main formats, top three topics, and the top five videos (title, URL, views, outlier ratio, likes and comments per 1,000 views). Under it: the hooks and calls to action that recur in the winners, and three gaps: topics or formats that win for a competitor and that the user has not covered.

## Judgment

- Judge the current strategy on recent videos. A popular list full of five-year-old hits says what worked then; the latest median says what works now.
- A falling median across the latest videos means the channel's audience is fading, whatever its subscriber count. Say so; it is an opening.
- A product demo with a million views and a handful of comments was probably run as an ad. It shows ad spend, not demand.
- Big competitors often upload webinar recordings and event talks with low views. Leave those out of the median when judging the videos they make for YouTube.
- For their comments (objections, feature requests, praise), open [comment mining](comment-mining.md) on the top videos from this table.
- The channel is one channel. For the rest of a competitor's marketing (site, ads, pricing, positioning), use the [competitors](../../competitors/references/teardown.md) group.
