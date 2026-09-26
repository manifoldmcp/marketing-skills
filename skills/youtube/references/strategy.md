# YouTube strategy

A YouTube strategy answers three questions before anyone films: which topics already pull views in the category, which channels own them and how, and whether the team should build a channel, borrow other people's audiences, or both. It ends in a 90-day plan, not a list of videos.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: awareness, signups or leads, search traffic that keeps coming, or videos for sales to send. Default: signups, measured by views on videos that answer buying questions.
- **Stage**: no channel, a channel with a few videos, or an established one. Ask for the handle; `youtube_get_channel` in step 2 answers the rest.
- **ICP**: who buys, so the topics are what those people search, not what entertains everyone.
- **Budget**: credits for the research (this playbook costs about 35) and videos a month the team can make. Default: 1,000 credits and 2 videos a month.
- **Team**: who can be on camera, who edits, and whether anyone can appear as a podcast guest. Default: the founder on camera, no editor.
- **Competitors**: two or three channels in the category, by handle. Default: the three channels that appear most often in the step 2 searches.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 15 credits for the user and three competitors. `youtube_search_videos` for three category queries ("<category>", "<category> tutorial", "best <category>") with `since: "year"` (1 credit each) shows which channels own the category now; take the competitors from it if the user named none. Then, for the user's channel and each competitor: `youtube_get_channel` (1 credit each) for subscribers and video count, `youtube_get_videos` with the default sort (1 credit each) for cadence and the median views of the latest 10, and `youtube_get_videos` with `sort: "popular"` (1 credit each) for their all-time hits. Apply the [channel health](../SKILL.md#channel-health) rules to every channel.
3. **Gaps.** About 20 credits more.
   - Topic gap: from the competitors' popular and recent videos, name the topics and formats (tutorial, comparison, review, interview, teardown) that produced outliers, and mark the ones the user has not covered.
   - Demand gap: `seo_search_keywords` with `seed: "<category>"` (10 credits). Keep the questions and the "how to", "tutorial", "review", "vs" and "alternative" phrases; then `seo_get_serp` on the top 10 of them (1 credit each) and mark the ones with a `video` in `features`, where a video can also win Google traffic. Google volume is a proxy for YouTube demand; say so.
   - Freshness gap: queries from step 2 whose top results are all more than two years old, which an up-to-date video can take.
   - AI gap, only if the goal includes AI answers: whether AI engines cite YouTube for the category, from [AI-cited videos](ai-cited-videos.md). Do not run it here; name it as a tactic.
4. **Tactics.** Choose two or three from this group and say why each fits the numbers:
   - [Video ideas](video-ideas.md) when the team will publish: it turns the gaps into a ranked list of videos with the evidence for each.
   - [Competitor channels](competitor-channels.md) when one competitor clearly wins on YouTube and the user needs to know how.
   - [Comment mining](comment-mining.md) when the category has review and tutorial videos with busy comments: the questions there are the next videos and the objections are the scripts.
   - [Podcast guesting](podcast-guesting.md) when nobody can publish every month but the founder can talk: other people's audiences, no production.
   - [Find creators](find-creators.md) when a budget exists but no team: sponsor channels whose median views fit, instead of building one.
   - [AI-cited videos](ai-cited-videos.md) when the goal includes being named in ChatGPT, Perplexity or Google's AI answers.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - By day 30, the first videos from the video ideas list, each on a query with evidence, and the first podcast or creator pitches if chosen. By day 60, a steady cadence the team can keep and the formats that worked doubled. By day 90, a second round of ideas from comment mining and the step 3 gaps.
   - KPIs the tools can measure again later: subscribers (`youtube_get_channel`), median views of the latest 10 videos (`youtube_get_videos`), whether the user's videos appear on the first page of `youtube_search_videos` for the target queries, and, if chosen, YouTube citations for the user's prompts (`aeo_run_ai_answers`).
   - **Deliver** one document: the inputs with defaults marked, a baseline table (user against each competitor: subscribers, videos, uploads in the last 90 days, median views of the latest 10, views as a share of subscribers, best topics), the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and the three first actions.

## Judgment

- YouTube compounds slowly. The 30-day KPI is videos published and views on them; subscribers belong to the 90-day KPI.
- A small channel wins on search, not on the home feed: "how to", comparison and review videos keep getting found for years. Trend topics favour channels that already have an audience.
- Set cadence from the team's hours. Two good videos a month for a year beat eight in a month and then silence.
- For B2B, views from the right people matter more than subscribers. A tutorial with 2,000 views from buyers beats a vlog with 50,000 from anyone.
- If no one will appear on camera, the plan leads with creators, and a channel of screen-recorded tutorials still works. Podcast guesting needs a guest who will talk on video.
- `youtube_search_videos` returns regular videos, not Shorts. If the user asks about Shorts, say the data cannot compare them.
