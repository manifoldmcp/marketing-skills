---
name: audit-youtube-channel
description: When the user wants to know what a YouTube channel publishes and what works for it, their own or a competitor's. Reads each channel's size, upload cadence, length mix, formats, median views of its latest videos, its all-time hits, engagement on the outliers and the hooks and calls to action in their transcripts, side by side with rivals. Also use when the user mentions a YouTube channel audit, a YouTube channel teardown, break down a competitor's YouTube, what a brand posts on YouTube and how often, their most viewed videos, or benchmarking our channel against competitors. TikTok goes to audit-tiktok-account, Instagram to audit-instagram-account, Facebook to audit-facebook-page, LinkedIn to audit-linkedin-page, X to audit-x-account; a plan to grow on YouTube to create-youtube-plan; one viral video to analyze-viral-youtube-video; a rival beyond social to tear-down-competitor.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Audit a YouTube channel

What a competitor's YouTube channel publishes, how often, what earns views and what those videos do that the user's do not. Each channel is judged against its own baseline and against the others. It ends in a comparison table, with a link to every video and channel it cites, and the gaps the user can take, not in a list of every video.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `youtube_get_channel` (hosts often add a prefix, for example `mcp__manifold__youtube_get_channel`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `youtube_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the user's own channel, the competitors and their channels) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Competitors**: two or three channels, by handle. If the user gives company names, `youtube_search_videos` with the brand name (1 credit) and take the channel whose `author_name` matches; a channel that only reviews the brand is not theirs. If they do not know their competitors, [find-competitors](../find-competitors/SKILL.md) finds them first.
- **The user's channel**: the handle, to compare against. Optional.
- **Window**: the latest 20 videos for the current strategy, plus the most viewed for what has worked over time.
- **Budget**: a default run costs at most 4 x (1 + 2 + 1 + 8 + 3) = 60 credits for three competitors and the user. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the platform notes** before the first call: [YouTube notes](../create-youtube-plan/references/platforms/youtube.md).
2. **Size each channel.** `youtube_get_channel` on each competitor and the user (1 credit each): subscribers (`followers`), total `views`, video count (`posts_count`) and when it joined (`created_at`).
3. **Read the current strategy.** `youtube_get_videos` with the default sort (1 credit a page), with a second page if the first holds fewer than 20 videos. From them: uploads per month over the last 90 days, the median views of the latest 10 (leaving out videos under 14 days old), the median as a share of subscribers, the length mix from `duration_s` (under 10 minutes, 10 to 30, over 30), and the topics and formats from the titles (tutorial, comparison, customer story, webinar recording, podcast, product update).
4. **Read what has worked.** `youtube_get_videos` with `sort: "popular"` (1 credit) for the all-time hits. Mark the outliers against the median from step 3 per the [outlier rule](../create-youtube-plan/references/platforms/youtube.md#channel-health), and note whether they are recent or years old.
5. **Check engagement on the shortlist.** `youtube_get_video` on the top five outliers and three recent videos (1 credit each): likes and comments per 1,000 views. Apply the [engagement rule](../create-youtube-plan/references/platforms/youtube.md#channel-health): high views with almost no likes often means ad-driven views. `is_ad: true` marks a video that declares a paid promotion.
6. **Read why the best ones work.** `youtube_get_transcript` on the top three outliers (1 credit each). Note the hook in the first 30 seconds (the first 80 words or so), the structure, and the call to action (a trial, a template, a demo booking). `NoData` means no captions: take the hook from the title and mark it.
7. **Deliver** a table with one column per channel, the user's included: subscribers, videos, uploads per month (last 90 days), median views of the latest 10, median as a share of subscribers, length mix, main formats, top three topics, and the top five videos (title, URL, views, outlier ratio, likes and comments per 1,000 views). Under it: the hooks and calls to action that recur in the winners, and three gaps: topics or formats that win for a competitor and that the user has not covered.

## Judgment

- **Own baseline.** Judge a video against its own channel's median, never against YouTube at large; a rival against the user only over the same window.
- **Recent over popular.** Judge the current strategy on recent videos. A popular list full of five-year-old hits says what worked then; the latest median says what works now.
- **A fading channel.** A falling median across the latest videos means the channel's audience is fading, whatever its subscriber count. Say so; it is an opening.
- **Paid apart.** Videos with `is_ad: true` stay out of the medians: their views may have been bought. A product demo with a million views and a handful of comments was probably run as an ad. It shows ad spend, not demand.
- **Recordings apart.** Big competitors often upload webinar recordings and event talks with low views. Leave those out of the median when judging the videos they make for YouTube.
- **Repeatable, not lucky.** The count of outliers says more than the single biggest video: six mean a repeatable format, one means luck. Formats a channel keeps uploading usually work for it; one it tried once and dropped probably did not.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Handoff.** The deliverable is a table with a link to every video and channel it cites. Never post, comment, subscribe or message. A weekly watch of a rival's uploads is the host's schedule, through [monitor-competitors](../monitor-competitors/SKILL.md).

## Related skills

- A plan for the user's own channel: [create-youtube-plan](../create-youtube-plan/SKILL.md). Video ideas from what the niche searches and watches: [find-youtube-video-ideas](../find-youtube-video-ideas/SKILL.md).
- Why one video beat its channel's usual numbers: [analyze-viral-youtube-video](../analyze-viral-youtube-video/SKILL.md).
- The same audit on other platforms: [audit-tiktok-account](../audit-tiktok-account/SKILL.md), [audit-instagram-account](../audit-instagram-account/SKILL.md), [audit-facebook-page](../audit-facebook-page/SKILL.md), [audit-linkedin-page](../audit-linkedin-page/SKILL.md), [audit-x-account](../audit-x-account/SKILL.md). Run each and set the results side by side, comparing channels only within one platform.
- The comments under the top videos (objections, feature requests, praise): [mine-youtube-comments](../mine-youtube-comments/SKILL.md).
- The channel is one channel. For the rest of a competitor's marketing (site, ads, pricing, positioning): [tear-down-competitor](../tear-down-competitor/SKILL.md).
- Turning the user's own videos into posts for other channels: [repurpose-content](../repurpose-content/SKILL.md).
