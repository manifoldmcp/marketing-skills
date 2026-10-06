---
name: analyze-viral-tiktok
description: When the user wants to know why one TikTok went viral. Sets the video against its account's own baseline, reads its transcript, its most liked comments, its share rate and its timing, checks whether it rode a trend, and ranks the likely causes with the evidence for each and what the user can repeat. Also use when the user mentions why did this TikTok go viral, why did this TikTok blow up, break down this TikTok, can we replicate this TikTok, or our TikTok got far more views than usual. One reel goes to analyze-viral-reel, one YouTube video to analyze-viral-youtube-video; the hooks across a niche's top TikToks to find-tiktok-hooks; what is rising on TikTok to find-tiktok-trends; a whole TikTok account to audit-tiktok-account; reusing the video on other channels to repurpose-content.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Analyze a viral TikTok

Why one TikTok did far better than its account usually does. It sets the video against the account's own baseline, reads its transcript and comments, and checks its timing. It hands back a table of likely causes with the evidence for each, and what the user can repeat.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_get_videos` (hosts often add a prefix, for example `mcp__manifold__tiktok_get_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `tiktok_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: this skill needs them.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the user's own handle, the audience's time zone) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Video**: the TikTok URL (required). The account comes from it.
- **Question**: why it worked, how the user could repeat it, or both. Default: both.
- **User's account**: the user's handle, if they want the "repeat it" part fitted to their own numbers. Optional.
- **Budget**: a default run costs about 23 credits (13 when the video is already in the account's listing, 33 with `ai_fallback`). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **The baseline.** The account's size and its usual numbers, read as the [TikTok notes](../create-tiktok-plan/references/platforms/tiktok.md#reading-the-numbers) say: `tiktok_get_profile` (1 credit) for followers; `tiktok_get_videos` with `sort: "latest"`, two pages (1 credit a page), for the median views, engagement rate, shares over views, comments over likes and `duration_s`; `tiktok_get_videos` with `sort: "popular"`, one page (1 credit): is this the account's one hit, or one of several?
2. **The video.** Take its row from the listings if it is there; if not, `tiktok_get_video` (estimated 10 credits, 1 when the vendor does not fetch the media). Work out its multiple of the median views, views over followers, and its engagement ratios against the account's medians. If `is_ad: true`, the video is an ad or a paid partnership and its views may have been bought: say so first, and treat every factor below as weaker evidence.
3. **What was said.** `tiktok_get_transcript` on the video (1 credit; `ai_fallback: true` makes it 11 when TikTok holds no transcript and the video is under 2 minutes). Read the hook (the first sentence), the structure (setup, turn, payoff, call to action), the topic and the length against the account's median. Then the same tool on three of the account's videos near its median (1 credit each) and say what differs. `NoData` means no speech: the hook is on screen; mark it.
4. **What people reacted to.** `tiktok_get_comments`, three pages (1 credit a page). Sort by likes. Count friends tagged (people sharing it), questions, disagreement, requests for a part 2, and any line people quote back. A liked comment that quotes the video names the line that landed.
5. **Timing.** From `created_at`, converted from UTC to the audience's time zone: the day and hour against the account's usual, the gap since its previous video, and the video's age (under 48 hours, the numbers are still moving). Then `tiktok_search_videos` on the topic with `since: "month"`, two pages (1 credit a page). If many videos on the topic came before it, it rode a trend; if few did, it may have started one.
6. **Deliver** a table: factor (hook, topic, format, length, timing, trend, what drove comments, share rate), this video, the account's usual, verdict (likely cause, possible, not a cause), and the evidence (a quote, a number, a link). Below it, three lines: what to repeat, how the user would do it on their account, and what cannot be copied.

## Judgment

- **One video proves little.** One video cannot prove a cause. Rank the factors by the strength of their evidence, and drop any factor the account's ordinary videos share: it is not what made this one different.
- **Beyond the followers.** Views above the follower count mean TikTok showed the video far beyond the account's followers. The cause is in the video, not the audience.
- **Shares and comments.** Shares over views well above the median mean people sent it on: it is useful or relatable. Comments over likes well above the median mean a debate: it divides people.
- **What is not there.** The biggest factors in distribution, watch time, completion or retention, and where the views came from, are not in the tools. Say so. For the user's own video, TikTok Studio has them.
- **Sound.** The rows carry no sound data. If the video rides a song or a sound, the user has to check it in the app.
- **No cause the data shows.** Some breakouts have no cause the data shows. When the evidence is thin, say so rather than invent one.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Call `tiktok_get_video` only when the video is not in the listing.
- **Handoff.** The deliverable is a table of evidence with a link to every video it cites. Never post, comment or schedule. Write a script or hooks only when the user asks, from what the table shows, and never pass off the creator's video or words as the user's.

## Related skills

- The same breakdown for one reel or one YouTube video: [analyze-viral-reel](../analyze-viral-reel/SKILL.md), [analyze-viral-youtube-video](../analyze-viral-youtube-video/SKILL.md).
- How the niche's top TikToks open: [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md). What is rising on TikTok: [find-tiktok-trends](../find-tiktok-trends/SKILL.md).
- A whole account's cadence and top videos: [audit-tiktok-account](../audit-tiktok-account/SKILL.md). Everything said under the video: [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md).
- Turning the user's own winner into posts for other channels: [repurpose-content](../repurpose-content/SKILL.md).
- A plan for the user's TikTok: [create-tiktok-plan](../create-tiktok-plan/SKILL.md).
