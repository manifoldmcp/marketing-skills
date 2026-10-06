---
name: find-youtube-video-ideas
description: When the user wants to know which YouTube videos to make. Ranks video ideas by evidence that people watch them, outlier videos that beat their channel's median views, Google demand for the same question and a gap in what already ranks, and gives each a working title, the proof and the angle the existing videos miss. Also use when the user mentions YouTube video ideas, what should we make videos about, YouTube topics that get views, YouTube keyword research, what videos to make about a topic, or which YouTube topics are underserved. Ideas across channels go to find-content-ideas; LinkedIn post formats to find-linkedin-post-formats; why one YouTube video beat its channel to analyze-viral-youtube-video; a plan for the channel to create-youtube-plan; questions viewers ask in comments to mine-youtube-comments.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find YouTube video ideas

A ranked list of videos worth making, each backed by evidence that people watch it: outlier videos on YouTube, Google demand for the same question, and a gap in what already ranks. It ends in a table of ideas with a working title, the proof and the angle, not in a content calendar.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `youtube_search_videos` and `seo_search_keywords` (hosts often add a prefix, for example `mcp__manifold__youtube_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The steps use the `youtube_*` and `seo_*` tool groups. If other manifold tools are there but `youtube_*` is not, it is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off): say so and stop. Without `seo_*`, run the YouTube steps and mark the Google columns as not measured.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the niche, the ICP, customer language, the user's YouTube channel, the competitors) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Topic**: the category and the jobs the buyer does ("cold email", "bookkeeping for freelancers"). Two or three seed phrases.
- **Audience**: who should watch, so a popular topic for the wrong people is dropped.
- **Channel**: the user's handle, if they have one, to skip topics they already covered.
- **Competitors**: optional; their names add "<competitor> review" and "<competitor> vs" queries.
- **Market**: `location` and `language` for the Google calls if not the United States and English.
- **Budget**: a default run costs about 10 + 10 x 2 + 15 + 10 + 9 + 1 = 65 credits for ten queries. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the [YouTube notes](../create-youtube-plan/references/platforms/youtube.md)** before the first call. They say how to read YouTube's numbers and what each call costs.
2. **Find the questions.** `seo_search_keywords` with `seed: "<topic>"` (10 credits for 100 rows). Keep the phrases a video answers: questions, "how to", "tutorial", "for beginners", "review", "vs", "alternative", "template", "example". Take the top 10 by volume, plus any competitor queries. The volume is Google's, a proxy for YouTube demand; say so in the table.
3. **See what YouTube shows.** For each of the 10 queries, `youtube_search_videos` twice (1 credit each): with `since: "year"` for current demand, and with `since: "all"` and `sort: "popular"` for the evergreen winners. Read `views`, `created_at`, `duration_s`, `author` and the title in `text`.
4. **Find the outliers.** For the channels behind the top results, up to 15, `youtube_get_videos` with the default sort (1 credit each) gives the channel's median views of its latest 10. Mark every result with 3 times its channel's median or more: the [outlier rule](../create-youtube-plan/references/platforms/youtube.md#channel-health). An outlier from a small channel is the strongest proof: the topic did the work, not the audience.
5. **Check Google.** `seo_get_serp` on the 10 queries (1 credit each). A `video` in `features` means Google shows videos for the query, so a good video can earn Google traffic too. YouTube URLs among the organic `results` rank on their own.
6. **Find the angle.** For the three strongest ideas, `youtube_get_transcript` on the top three videos (1 credit each). Note what they cover, what they skip, and how old their facts are (prices, screenshots, versions). For busy comment sections, [mine-youtube-comments](../mine-youtube-comments/SKILL.md) finds the questions viewers still ask.
7. **Score and cut.** Rank by: outlier evidence first, then Google volume with a video feature, then a freshness gap (the top results are more than two years old), then fit with the audience and the user's product. Drop topics the user's channel already covers well (one `youtube_get_videos` with `sort: "popular"` on it, 1 credit), and topics where the only outliers come from channels with millions of subscribers and nothing smaller ranks.
8. **Deliver** a table: idea (working title), query, Google volume, video feature on Google (yes, no), top video (URL, views, age, length), best outlier (channel, median views, the video's views, ratio), typical length of the top results, format (tutorial, comparison, review, list, teardown), and the angle: what the existing videos miss.

## Judgment

- An outlier beats a big number. A video with 40,000 views on a channel whose median is 2,000 says more about demand than 2 million views on a channel whose median is 1.5 million.
- Compare views per month since upload, not lifetime views. A three-year-old video with 300,000 views earned less a month than a six-month-old one with 100,000.
- Many YouTube topics have no Google volume at all, and some Google queries never get a video. Treat a query with volume, a video feature and outliers as the best case; treat one signal alone as a hint.
- A Google keyword under about 50 searches a month is a phrasing, not a topic to plan around. Every row names its evidence: the query and its `volume`, the video URL and its views.
- Tutorials, comparisons and reviews match buyers; entertainment topics with huge views rarely do. Keep the audience test strict for B2B.
- Do not copy the winning titles. Give the angle the existing videos miss; the user writes the title.
- Search results are ranked and incomplete. An idea missing from them is not proof nobody made it.
- The tools see what people search and watch, not what converts. Say so when the user asks which video will bring customers; their own analytics answer that.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Run the cheap calls first: YouTube searches, listings, transcripts and `seo_get_serp` cost 1 credit a page or a call; `seo_search_keywords` costs 10 for 100 rows. Keywords this account already paid for are free for 7 days, platform searches for 6 hours, transcripts for 30 days.
- **Handoff.** The server does no content generation. When the user asks for scripts or outlines, the host writes them from the table. Never invent a number or a result the evidence does not hold, and never pass off a creator's video or words as the user's. Never post or schedule.

## Related skills

- Ideas across channels, not only YouTube: [find-content-ideas](../find-content-ideas/SKILL.md). Four weeks of dated posts: [create-content-calendar](../create-content-calendar/SKILL.md).
- Why one video beat its channel's usual numbers: [analyze-viral-youtube-video](../analyze-viral-youtube-video/SKILL.md). Questions in a video's comments: [mine-youtube-comments](../mine-youtube-comments/SKILL.md).
- A plan for the channel: [create-youtube-plan](../create-youtube-plan/SKILL.md). One channel audited, the user's or a competitor's: [audit-youtube-channel](../audit-youtube-channel/SKILL.md). Creators to work with: [find-youtube-creators](../find-youtube-creators/SKILL.md).
- One video turned into posts for other channels: [repurpose-content](../repurpose-content/SKILL.md).
