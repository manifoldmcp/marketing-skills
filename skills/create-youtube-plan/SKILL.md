---
name: create-youtube-plan
description: When the user wants a plan to grow on YouTube. Writes a YouTube strategy from which topics already pull views in the category, which channels own them and how, and whether to build a channel, borrow other people's audiences, or both, ending in a 30-60-90 day plan with KPIs the tools can measure again. Also use when the user mentions a YouTube strategy, should we start a YouTube channel, grow our YouTube channel, YouTube SEO, a YouTube content plan, or a 90-day YouTube plan. A plan for TikTok goes to create-tiktok-plan, Instagram to create-instagram-plan, LinkedIn to create-linkedin-plan, Facebook to create-facebook-plan; which platforms to be on at all to pick-channels; YouTube video ideas to find-youtube-video-ideas, a competitor's channel to audit-youtube-channel, podcast guesting to find-podcasts.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# YouTube plan

A YouTube plan answers three questions before anyone films: which topics already pull views in the category, which channels own them and how, and whether the team should build a channel, borrow other people's audiences, or both. It pulls in other skills as tactics (video ideas, competitor channels, podcasts, creators) and ends in a 30-60-90 day plan, not a list of videos.

This skill also holds the [YouTube notes](references/platforms/youtube.md) that every skill reading YouTube follows.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `youtube_search_videos` and `youtube_get_channel` (hosts often add a prefix, for example `mcp__manifold__youtube_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `youtube_*` tools are not, the YouTube tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- The demand gap uses `seo_search_keywords` and `seo_get_serp`. If the `seo_*` tools are off, say so and rank on YouTube evidence alone.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the channel handle, the ICP, the competitors and their channels, the brand voice, the goal) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it. If the user has not chosen YouTube yet, or asks which platforms to be on, run [pick-channels](../pick-channels/SKILL.md) first.

- **Goal**: awareness, signups or leads, search traffic that keeps coming, or videos for sales to send. Default: signups, measured by views on videos that answer buying questions.
- **Stage**: no channel, a channel with a few videos, or an established one. Ask for the handle; `youtube_get_channel` in the baseline answers the rest.
- **ICP**: who buys, so the topics are what those people search, not what entertains everyone.
- **Budget**: credits for the research (this plan costs about 38) and videos a month the team can make. Default: 300 credits and 2 videos a month.
- **Team**: who can be on camera, who edits, and whether anyone can appear as a podcast guest. Default: the founder on camera, no editor.
- **Competitors**: two or three channels in the category, by handle. Default: the three channels that appear most often in the baseline searches.
- **Horizon**: default 90 days.

## Steps

1. **Read the YouTube notes.** Before the first call, read the [YouTube notes](references/platforms/youtube.md): how to read the data, channel health, what each call costs and the handoff. They hold for every skill this plan pulls in.
2. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
3. **Baseline.** Say the cost first: about 15 credits for the user and three competitors. `youtube_search_videos` for three category queries ("<category>", "<category> tutorial", "best <category>") with `since: "year"` (1 credit each) shows which channels own the category now; take the competitors from it if the user named none. Then, for the user's channel and each competitor: `youtube_get_channel` (1 credit each) for subscribers and video count, `youtube_get_videos` with the default sort (1 credit each) for cadence and the median views of the latest 10, and `youtube_get_videos` with `sort: "popular"` (1 credit each) for their all-time hits. Apply the [channel health](references/platforms/youtube.md#channel-health) rules to every channel.
4. **Gaps.** About 23 credits more.
   - Topic gap: from the competitors' popular and recent videos, name the topics and formats (tutorial, comparison, review, interview, teardown) that produced outliers, and mark the ones the user has not covered.
   - Demand gap: `seo_search_keywords` with `seed: "<category>"` (10 credits). Keep the questions and the "how to", "tutorial", "review", "vs" and "alternative" phrases; then `seo_get_serp` on the top 10 of them (1 credit each) and mark the ones with a `video` in `features`, where a video can also win Google traffic. Google volume is a proxy for YouTube demand; say so.
   - Freshness gap: `youtube_search_videos` on the three baseline queries with `since: "all"` (1 credit each): a query whose top five are all more than two years old is one an up-to-date video can take.
   - AI gap, only if the goal includes AI answers: whether AI engines cite YouTube for the category, from the YouTube videos part of [build-ai-citations](../build-ai-citations/SKILL.md). Do not run it here; name it as a tactic.
5. **Tactics.** From the gaps, choose two or three of these skills and say why each fits the numbers:
   - [find-youtube-video-ideas](../find-youtube-video-ideas/SKILL.md) when the team will publish: it turns the gaps into a ranked list of videos with the evidence for each.
   - [audit-youtube-channel](../audit-youtube-channel/SKILL.md) for competitor channels when one competitor clearly wins on YouTube and the user needs to know how.
   - [analyze-viral-youtube-video](../analyze-viral-youtube-video/SKILL.md) when a competitor has an outlier video worth repeating.
   - [mine-youtube-comments](../mine-youtube-comments/SKILL.md) when the category has review and tutorial videos with busy comments: the questions there are the next videos and the objections are the scripts.
   - [find-podcasts](../find-podcasts/SKILL.md) when nobody can publish every month but the founder can talk: other people's audiences, no production.
   - [find-youtube-creators](../find-youtube-creators/SKILL.md) when a budget exists but no team: sponsor channels whose median views fit, instead of building one.
   - [build-ai-citations](../build-ai-citations/SKILL.md) for AI-cited videos when the goal includes being named in ChatGPT, Perplexity or Google's AI answers.
6. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - By day 30, the first videos from the video ideas list, each on a query with evidence, and the first podcast or creator pitches if chosen. By day 60, a steady cadence the team can keep and the formats that worked doubled. By day 90, a second round of ideas from comment mining and the step 4 gaps.
   - KPIs the tools can measure again later: subscribers (`youtube_get_channel`), median views of the latest 10 videos (`youtube_get_videos`), whether the user's videos appear on the first page of `youtube_search_videos` for the target queries, and, if chosen, YouTube citations for the user's prompts (`aeo_run_ai_answers`).
7. **Deliver** one document: the inputs with defaults marked, a baseline table (user against each competitor: subscribers, videos, uploads in the last 90 days, median views of the latest 10, views as a share of subscribers, best topics), the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions.

## Judgment

- **YouTube notes first.** They hold for every skill the plan pulls in.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Plans, not posts.** The deliverable is a plan with a link to every channel and video it cites. Never upload, post, comment, subscribe, send a pitch or schedule anything. Write titles or scripts only when the user asks, and base each on a pattern the evidence shows.
- **Measure again.** Every KPI in the plan is one the tools can read again later. A weekly check is the host's schedule, as in [monitor-competitors](../monitor-competitors/SKILL.md).
- YouTube compounds slowly. The 30-day KPI is videos published and views on them; subscribers belong to the 90-day KPI.
- A small channel wins on search, not on the home feed: "how to", comparison and review videos keep getting found for years. Trend topics favour channels that already have an audience.
- Set cadence from the team's hours. Two good videos a month for a year beat eight in a month and then silence.
- For B2B, views from the right people matter more than subscribers. A tutorial with 2,000 views from buyers beats a vlog with 50,000 from anyone.
- If no one will appear on camera, the plan leads with creators, and a channel of screen-recorded tutorials still works. Podcast guesting needs a guest who will talk on video.
- The tools cannot see click-through rate, average view duration or which title and thumbnail a channel is testing. They live in YouTube Studio; ask the user to read them at each review.
- `youtube_search_videos` returns regular videos, not Shorts. If the user asks about Shorts, say the data cannot compare them.

## Related skills

- Which platforms to be on at all: [pick-channels](../pick-channels/SKILL.md). The whole growth plan: [create-growth-plan](../create-growth-plan/SKILL.md).
- The same plan on another platform: [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md), [create-facebook-plan](../create-facebook-plan/SKILL.md), [create-reddit-plan](../create-reddit-plan/SKILL.md).
- Video ideas and viral breakdowns: [find-youtube-video-ideas](../find-youtube-video-ideas/SKILL.md), [analyze-viral-youtube-video](../analyze-viral-youtube-video/SKILL.md). A content calendar and repurposing: [create-content-calendar](../create-content-calendar/SKILL.md), [repurpose-content](../repurpose-content/SKILL.md).
- A competitor's channel: [audit-youtube-channel](../audit-youtube-channel/SKILL.md). What people ask in YouTube comments: [mine-youtube-comments](../mine-youtube-comments/SKILL.md).
- Other people's audiences: [find-podcasts](../find-podcasts/SKILL.md), [find-youtube-creators](../find-youtube-creators/SKILL.md), [find-creators](../find-creators/SKILL.md) across platforms, [create-influencer-plan](../create-influencer-plan/SKILL.md).
- Videos that AI engines cite: [build-ai-citations](../build-ai-citations/SKILL.md). Videos that rank on Google: [create-seo-plan](../create-seo-plan/SKILL.md).
