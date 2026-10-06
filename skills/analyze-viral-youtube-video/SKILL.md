---
name: analyze-viral-youtube-video
description: When the user wants to know why one YouTube video took off. Sets the video against its channel's own baseline in views per day, reads its transcript, its most liked comments and its timing, checks whether it rode a trend, and ranks the likely causes with the evidence for each and what the user can repeat. Also use when the user mentions what made this YouTube video take off, why did this video blow up on YouTube, break down this YouTube video, can we replicate this video, or our YouTube video got far more views than usual. One TikTok goes to analyze-viral-tiktok, one reel to analyze-viral-reel; a whole channel to audit-youtube-channel; video ideas for a channel to find-youtube-video-ideas; reusing the video on other channels to repurpose-content.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Analyze a viral YouTube video

Why one YouTube video did far better than its channel usually does. It sets the video against the channel's own baseline, reads its transcript and comments, and checks its timing. It hands back a table of likely causes with the evidence for each, and what the user can repeat.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `youtube_get_videos` (hosts often add a prefix, for example `mcp__manifold__youtube_get_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `youtube_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: this skill needs them.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the user's own channel, the audience's time zone) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Video**: the YouTube URL (required). The channel comes from it.
- **Question**: why it worked, how the user could repeat it, or both. Default: both.
- **User's channel**: the user's handle, if they want the "repeat it" part fitted to their own numbers. Optional.
- **Budget**: a default run costs about 16 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **The baseline.** The channel's size and its usual numbers, read as the [YouTube notes](../create-youtube-plan/references/platforms/youtube.md#channel-health) say: `youtube_get_channel` (1 credit); `youtube_get_videos` with the default sort and with `sort: "popular"` (1 credit each) for the median views of the latest 10 and the all-time hits. Compare views per day since upload, since views are lifetime. Listings carry no likes or comments, so `youtube_get_video` on three videos near the median (1 credit each).
2. **The video.** Take its row from the listings if it is there, and `youtube_get_video` (1 credit) for its likes and comments. Work out its multiple of the median views per day, views over subscribers, and its engagement ratios against the channel's medians. If `is_ad: true`, the video declares a paid promotion and its views may have been bought: say so first, and treat every factor below as weaker evidence.
3. **What was said.** `youtube_get_transcript` on the video (1 credit). Read the hook (the first sentence), the structure (setup, turn, payoff, call to action), the topic and the length against the channel's median. Then the same tool on three of the channel's videos near its median (1 credit each) and say what differs. `NoData` means no captions: the hook is on screen or in the title; mark it.
4. **What people reacted to.** `youtube_get_comments`, three pages (1 credit a page). Sort by likes. Count friends tagged (people sharing it), questions, disagreement, requests for a part 2, and any line people quote back. A liked comment that quotes the video names the line that landed.
5. **Timing.** From `created_at`, converted from UTC to the audience's time zone: the day and hour against the channel's usual, the gap since its previous video, and the video's age (under 48 hours, the numbers are still moving). Then `youtube_search_videos` on the topic with `since: "month"`, two pages (1 credit a page). If many videos on the topic came before it, it rode a trend; if few did, it may have started one.
6. **Deliver** a table: factor (hook, title, topic, format, length, timing, trend, what drove comments), this video, the channel's usual, verdict (likely cause, possible, not a cause), and the evidence (a quote, a number, a link). Below it, three lines: what to repeat, how the user would do it on their channel, and what cannot be copied.

## Judgment

- **One video proves little.** One video cannot prove a cause. Rank the factors by the strength of their evidence, and drop any factor the channel's ordinary videos share: it is not what made this one different.
- **Beyond the subscribers.** Views above the subscriber count mean YouTube showed the video far beyond the channel's subscribers. The cause is in the video, not the audience.
- **Comments over likes.** Comments over likes well above the median mean a debate: it divides people.
- **Title and thumbnail.** On YouTube a title and thumbnail can carry a video the content does not explain. The tools see the title, not the thumbnail; say so when the transcript shows nothing unusual.
- **What is not there.** The biggest factors in distribution, watch time, completion or retention, and where the views came from, are not in the tools, and neither is click-through rate. Say so. For the user's own video, YouTube Studio has them.
- **Sound.** The rows carry no sound data. If a Short rides a song or a sound, the user has to check it in the app.
- **No cause the data shows.** Some breakouts have no cause the data shows. When the evidence is thin, say so rather than invent one.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Handoff.** The deliverable is a table of evidence with a link to every video it cites. Never post, comment or schedule. Write a script or hooks only when the user asks, from what the table shows, and never pass off the creator's video or words as the user's.

## Related skills

- The same breakdown for one TikTok or one reel: [analyze-viral-tiktok](../analyze-viral-tiktok/SKILL.md), [analyze-viral-reel](../analyze-viral-reel/SKILL.md).
- A whole channel's cadence and top videos: [audit-youtube-channel](../audit-youtube-channel/SKILL.md). Everything said under the video: [mine-youtube-comments](../mine-youtube-comments/SKILL.md).
- Video ideas from what the niche searches and watches: [find-youtube-video-ideas](../find-youtube-video-ideas/SKILL.md).
- Turning the user's own winner into posts for other channels: [repurpose-content](../repurpose-content/SKILL.md).
- A plan for the user's channel: [create-youtube-plan](../create-youtube-plan/SKILL.md).
