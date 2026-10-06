---
name: find-tiktok-trends
description: When the user wants to know what is trending on TikTok in their niche right now. Finds the formats, topics and lengths that several creators are using this week or month and that beat their own median views, from niche searches on TikTok, with example videos and a line on how the user could use each trend. Also use when the user mentions TikTok trends, what's trending on TikTok, which TikTok formats are blowing up right now, trending TikTok topics this week, or what to film next for TikTok. What is trending on Reels goes to find-reels-trends; how the top TikToks open to find-tiktok-hooks; why one TikTok went viral to analyze-viral-tiktok; ideas from search and Reddit demand to find-content-ideas; a 90-day TikTok plan to create-tiktok-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find TikTok trends

What is rising on TikTok in a niche right now: the formats, topics and lengths that several creators are using this week and that beat their own usual numbers. It hands back a short, dated table of trends, each with example videos and a line on how the user could use it.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos` and `tiktok_get_videos` (hosts often add a prefix, for example `mcp__manifold__tiktok_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `tiktok_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: this skill needs them.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the niche, the category and the problems it solves, the market and its language) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Niche**: three or four search terms: the category, a problem the product solves, a use case ("skincare routine", "acne", "glass skin"). Default: the category and two problems in the user's own words.
- **Window**: `since: "week"` by default. Use `since: "month"` for a slow niche or when the user asks about the month.
- **Language**: search has no country filter, so name the language to keep. Default: English.
- **Budget**: a default run costs about 4 x 2 + 10 + 10 = 28 credits for four terms. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the [TikTok notes](../create-tiktok-plan/references/platforms/tiktok.md)** before the first call. They say how to read TikTok's numbers and what each call costs.
2. **Pull what is new and popular.** `tiktok_search_videos` for each term with `since: "week"` and `sort: "popular"`, two pages each (1 credit a page). Drop rows with `is_ad: true`, duplicates across terms, and captions in other languages.
3. **Measure speed.** For each video, views per day since `created_at`. Rank on it: `sort: "popular"` ranks by likes, which favours the videos that had the most days to collect them.
4. **Separate the trend from the account.** For the authors of the 10 fastest videos, `tiktok_get_videos` with `sort: "latest"`, one page each (1 credit), and compare each video with its author's median views. A video at 3 times its author's median or more was lifted by what it did. A large account posting at its usual numbers is no evidence of a trend.
5. **Name the format.** `tiktok_get_transcript` on the 10 strongest outliers (1 credit each). With the caption, name each format: talking head, voiceover over footage, list, tutorial, before and after, reply to a comment, skit, photo carousel (`media: "image"`), or no speech (`NoData`: usually text on screen over music).
6. **Group.** Group the outliers by format, by topic (the words and hashtags that recur in `text`) and by length (`duration_s` under 15 seconds, 15 to 60, over 60). A group is a trend when it holds three or more videos from three or more authors in the window. The rows carry no sound data: say so, and point the user to the sound pages in the TikTok app or TikTok's Creative Center for sound trends.
7. **Deliver** a table: trend (a format, a topic or a length), what it is in one line, videos in the window, authors, median views, median views per day, median multiple of the author's own median, two example links with their first spoken or caption line, how the user could use it, and the date the data was pulled.

## Judgment

- One viral video is not a trend. Three authors doing the same thing and beating their own medians is.
- Trends on TikTok fade within one to three weeks. Date the table; a trend whose oldest example is two weeks old is probably at its peak.
- The trend is the format or the topic, never the video. Advise the user to make their own version, not a copy.
- A niche with fewer than about 20 results for a term in a week is too thin for a weekly read. Run it again with `since: "month"` and say the window changed.
- A business account on TikTok can use only the platform's commercial sound library, so a trend that rides a popular song may be closed to a brand. Flag the ones that depend on the audio.
- When the user also wants Reels, run [find-reels-trends](../find-reels-trends/SKILL.md) and keep the tables apart: views compare only within one platform.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Searches and listings this account already paid for are free for 6 hours, transcripts for 30 days.
- **Handoff.** The deliverable is a table of evidence. Never post or schedule. For the same read every week, the host schedules this skill and keeps the previous table; the server keeps no state.

## Related skills

- How the top TikToks open: [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md). Why one TikTok beat its account's usual numbers: [analyze-viral-tiktok](../analyze-viral-tiktok/SKILL.md).
- What is rising on Reels: [find-reels-trends](../find-reels-trends/SKILL.md).
- Ideas from what people search and ask: [find-content-ideas](../find-content-ideas/SKILL.md). Four weeks of dated posts: [create-content-calendar](../create-content-calendar/SKILL.md).
- A TikTok plan: [create-tiktok-plan](../create-tiktok-plan/SKILL.md). TikTok creators to make content with: [find-tiktok-creators](../find-tiktok-creators/SKILL.md).
