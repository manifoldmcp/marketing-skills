# YouTube as a source for AI answers and Google

YouTube videos are now one of the main ways a brand gets into Google's AI answers, and being talked about on YouTube tracks AI visibility more closely than any other signal measured. What the evidence says, which videos get cited, and how to find the ones that matter for the user's buyer questions. Checked October 2026. Measuring AI answers in general (prompt sets, noise, engines) belongs to [ai-search](../../ai-search/SKILL.md).

## What the evidence says

- **YouTube is the domain Google's AI answers cite most.** It is the most-cited domain in AI Overviews and growing (+34% over six months; 18% of cited URLs that rank outside Google's top 100 are YouTube videos; Ahrefs, March 2026). AI Mode cites it heavily too.
- **Being talked about on YouTube tracks AI visibility best of any signal tested.** Across 75,000 brands, the number of YouTube videos mentioning a brand in the title, transcript or description correlated with its AI visibility at Spearman ρ about 0.74, against 0.66 to 0.71 for branded web mentions and 0.27 to 0.33 for Domain Rating ([Ahrefs](https://ahrefs.com/blog/ai-brand-visibility-correlations/), 12 December 2025; brands with DR over 40). Ahrefs' own caveat: correlation, and possibly just a marker of brand strength. Read it as "brands buyers discuss on YouTube are the brands AI names", not "a video makes AI name you".
- **Engines differ.** Across engines Reddit and YouTube were the only domains in every engine's top five (Peec AI, 30M sources, March 2026). In Manifold's October 2026 runs YouTube was 20% of AI Overview citations and 15% of AI Mode's, and none of ChatGPT's, Claude's, Gemini's or Perplexity's (five prompts, two runs; see [ai-search](../../ai-search/SKILL.md)). Check per engine before promising a YouTube route into ChatGPT.
- **Google's rankings no longer predict AI Overview citations** (about 38% of cited URLs rank in the top 10, Ahrefs, January 2026). That applies to videos: a video does not need to rank to be cited.

## Which videos get cited

From the 14 YouTube videos Google's AI answers cited in our runs ([field notes](field-notes.md)); a small sample, but consistent:

- **Small and recent.** Median 613 views; 12 of 14 under 3,100. Channels of 1,050 to 98,300 subscribers. 10 of 14 uploaded in 2026 and none before August 2024; 8 put the year in the title.
- **Comparisons and "best X" lists**, seven to sixteen minutes long, plus two Shorts (83 and 106 seconds). All named the category and the buyer's question in the title.
- **Not YouTube's own ranking.** Only 5 of 14 were in YouTube's top 20 for the same query; two were YouTube's first result.
- **A moment, not just a video.** 11 of 21 citation links carried a timestamp, and 8 of 12 long videos had chapters in the description. Some timestamps matched a chapter start, others did not, so Google appears to cite the passage it used. How it picks the moment is not documented.
- **Anyone's channel.** Mostly independent reviewers, plus two publishers and two vendors, and 10 of 14 descriptions carried affiliate or tracking links. The vendors: NetHunt's own comparison that includes NetHunt was cited by AI Mode on both runs, and MailerLite's own Short by AI Overview. One cited video declared a paid promotion.

So the bar for being cited is not a big channel. It is a clear, current, chaptered video answering the exact buyer question, with the brand said aloud in it (transcripts are what engines can read).

## Finding the videos that matter for the user

About 75 credits for ten buyer questions with an existing AI answer run; without one, a baseline run first (see [ai-search](../../ai-search/SKILL.md), about 270 credits).

1. **Read the answers.** `get_task` on the run's `task_id` (free; results kept 30 days). Keep every citation whose domain is youtube.com (any subdomain) or youtu.be; for each video count the engines and prompts citing it, and note whether the user is among the row's `mentions[]`.
2. **Check Google.** `seo_get_serp` with `ai_overview: true` on the short search twin of each prompt ("best crm for agencies"). Keep youtube.com organic results, youtube.com `references` in the overview, and whether `features` includes `video`. The video block's row carries no video URLs, so which videos fill it is unknown; `youtube_search_videos` on the same query shows the likely ones. Mark them likely.
3. **Read each video**, up to 15: `youtube_get_video`, `youtube_get_channel` on its `author`, and `youtube_get_transcript` to see whether it names the user and which competitors, its format, and how current its facts are.
4. **Choose a route per video:**
   - **Already in:** it names the user fairly. Keep the creator's facts current.
   - **Get featured:** an active creator ([channel data](channel-data.md#channel-health)), a review, comparison or list format, and the user missing. The ask is an update, a pinned correction, or a place in the next video, sponsored if there is budget. Sizing, vetting and briefing creators belongs to [creators](../../creators/SKILL.md); finding the contact to [link-building](../../link-building/SKILL.md).
   - **Make a better video:** the cited video is old, from a paused or tiny channel, or a competitor's own. Given what gets cited, a small channel's honest comparison with chapters can enter, so this route is open to a brand with no audience ([video ideas](video-ideas.md)).

**Deliver** a table: video URL, title, channel, subscribers, views, age, cited by (engines, prompts), on Google (organic rank, overview reference, likely in the video block), names the user, competitors named, route, next step.

## Judgment

- A video cited by two or more engines, or for two or more prompts, is a target; one cited once is noise unless it is the only source for a prompt that matters. Citation sets churn week to week.
- A competitor's own video cannot be joined; the answer is a better video of the user's own.
- A creator who reviews tools for a living may ask a fee. A paid mention must be disclosed as a paid promotion on YouTube; never suggest hiding it, and never offer payment for an undisclosed mention.
- A self-ranking video is not a safe bet. NetHunt's was cited, but on Google a vendor's own "best X" page has tended to see its first pick left out of the answer ([ai-search](../../ai-search/SKILL.md)). An honest comparison that names real trade-offs is the version worth making.
- Never post, comment or contact anyone. Deliver the table.
