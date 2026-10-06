---
name: find-content-ideas
description: When the user wants to know what to post across channels. Turns the questions people ask on Google, AI engines, Reddit, YouTube, TikTok and LinkedIn into content ideas with the channel, format and angle each fits and the evidence behind each. Also use when the user mentions content ideas, what to post, what should we post about, content pillars, topics our buyers care about, or we ran out of ideas. YouTube video ideas with proven views go to find-youtube-video-ideas; which LinkedIn post formats, hooks and lengths work to find-linkedin-post-formats; TikTok and Reels hooks to find-tiktok-hooks and find-instagram-hooks; what is trending to find-tiktok-trends and find-reels-trends; dated posts for the month to create-content-calendar; blog posts meant to rank to create-seo-content-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find content ideas

Good ideas answer questions people already ask. This skill collects the questions and the demand around a topic from Google, AI engines, Reddit, YouTube and TikTok, merges the same question across sources, and gives each idea the channel and format it fits and the evidence behind it. It ends in a table of evidence, not drafts.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `tiktok_search_videos` (hosts often add a prefix, for example `mcp__manifold__seo_search_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The steps cross tool groups: `seo_*`, `aeo_*`, `reddit_*` and the platform groups (`tiktok_*`, `youtube_*`, `linkedin_*`). If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run the steps the rest allow, and mark those channels as not measured rather than weak.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the niche, the ICP, customer language, the brand voice, the channels the user posts on, the competitors) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Topic**: one to three seeds in the buyer's words: the problem and the category ("cash flow for freelancers", "invoice software"). Ask; the brand name alone finds nothing for a new product.
- **Channels**: the ones the user posts on, or the ranked table from [pick-channels](../pick-channels/SKILL.md). Default: let the evidence pick, and say which channels it pointed to.
- **Audience**: who reads, and how much they know: new to the problem, comparing tools, or already a customer. It sets the angle.
- **Count**: default 20 ideas.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 2 x 10 + 36 + 2 + 3 + 6 + 1 = 68 credits for two seeds. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Check the job.** Videos for YouTube only are [find-youtube-video-ideas](../find-youtube-video-ideas/SKILL.md); which LinkedIn post structures, hooks and lengths work in the niche is [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md). Ideas across channels follow the steps below and the [Evidence](#evidence) rules.
2. **Questions on Google.** `seo_search_keywords` with each seed (10 credits each). Keep the keywords above the Google floor in [Evidence](#evidence) that read as a question or a choice: how, what, why, can, best, vs, alternatives, template, examples. Note `intent` (informational wants an answer, commercial wants a comparison) and a rising `trend[12]`.
3. **Questions to AI engines.** `aeo_search_prompts` with `keyword` set to the main seed and `limit: 20` (36 credits). Prompts are longer and more specific than keywords ("best invoicing app for a freelancer with clients in Europe"); each is a candidate idea as asked. `answer_preview` shows what the engines say now, which is the answer to beat.
4. **Questions on Reddit.** `reddit_search_posts` with the main seed, `sort: "relevance"` and `time_range: "year"` (1 credit a page, two pages), then order the rows by `comments` yourself. Check the rows are on topic, as the search rules in the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md) describe; other sorts can drop the query. Keep threads whose title is a question, a complaint or a choice, with 10 or more comments. For the three busiest, `reddit_get_comments` (1 credit each): where the replies disagree, there is an opinion post; where they repeat one answer, there is a how-to. Keep the thread's wording: it is the audience's own.
5. **What already works on video.** `youtube_search_videos` and `tiktok_search_videos` with the three strongest questions from steps 2 to 4, `since: "year"` and `sort: "popular"` (1 credit a page each). Read titles, views and authors. A question with strong Google or Reddit demand and no good video is an open slot; a question where the top videos have large views proves the format and shows the angle to beat.
6. **LinkedIn, for a business audience.** `linkedin_search_posts` with the main seed and `since: "month"` (1 credit a page). Read which posts earn comments: a practitioner's story, a framework, a hot take. Skip this step for a consumer product.
7. **Merge into ideas.** One idea per question, with every source that shows it. For each, choose:
   - **Channel and format**, from where the evidence is: Google volume above the floor means an article (hand it to [write-seo-brief](../write-seo-brief/SKILL.md)); strong videos mean a YouTube tutorial or a short video; a busy Reddit thread means a reply there and an opinion post elsewhere; LinkedIn posts with comments mean a text post or a carousel; an AI prompt means a page that answers it directly. Where the format is unclear, `seo_get_serp` on the question (1 credit) shows what Google rewards: a `video` entry in `features[]`, Reddit threads, or articles only.
   - **Angle**: an answer, a comparison, an opinion, a story, or data.
   Rank by the number of sources first, then by the strongest demand number.
8. **Deliver** a table of the top ideas (default 20): idea (a working title), the question as people ask it, sources with their numbers (Google volume, AI prompt, Reddit comments, top video views), channel, format, angle, one evidence link, and who covers it now (the top result's author or domain). The host writes drafts from a row only when the user asks.

## Judgment

- An idea found in two sources beats a bigger number in one. A question people search on Google, ask ChatGPT and argue about on Reddit is the safest idea in the table.
- The format follows the evidence, not habit. If Google and the video searches both answer a question with videos, a blog post will not win it.
- Do not copy the top video or post. Take the question and answer it better: newer, more specific, with the user's own data or experience.
- Commercial questions ("best X", "X vs Y", "alternatives to X") are the ones closest to a sale. They belong in the table, but comparison pages on the user's site are [plan-comparison-pages](../plan-comparison-pages/SKILL.md).
- A rising `trend[12]` is worth an early slot; a falling one needs a reason to stay.
- The tools see what people search, ask and watch, not what converts. Say which ideas the evidence supports, and say so when the user asks which idea or channel will bring customers; their own analytics answer that.
- Hooks for TikTok or Reels are [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md) and [find-instagram-hooks](../find-instagram-hooks/SKILL.md); a deep read of one platform is [find-youtube-video-ideas](../find-youtube-video-ideas/SKILL.md) or [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md). This skill mixes channels.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Run the cheap calls first: platform searches, listings, profiles, Reddit calls and transcripts cost 1 credit a page or a call; `seo_search_keywords` costs 10 for 100 rows; `aeo_search_prompts` is the costly one at 30 plus 30 per 100 rows (36 with `limit: 20`, 45 at the default 50), so run it once per topic, never per idea. Keywords and prompts this account already paid for are free for 7 days, platform searches for 6 hours, transcripts for 30 days.
- **Handoff.** The server does no content generation. When the user asks for drafts, the host writes them from the table: the question in the audience's own words. Never invent a number, a quote or a result the evidence does not hold, and never pass off a creator's post or words as the user's. Never post or schedule. A request to repeat this every week is the host's schedule.

## Evidence

- Every row names its evidence: the keyword and its `volume`, the prompt as asked, the thread URL and its comment count, the video URL and its views. A row with no evidence is marked as the user's call, not dropped silently.
- The sources count in different units, so each has its own floor:
  - Google: a keyword with `volume` of about 50 a month or more. Below that, even the first position brings a handful of visits; use the keyword as a phrasing, not as a topic to plan around.
  - AI engines: any prompt `aeo_search_prompts` returns, since the index holds only prompts it has seen answered. Rank by `ai_search_volume`, but never quote it as searches: it is a People Also Ask proxy.
  - Reddit: a thread with 10 or more comments in the last year, or a community with 3 or more posts in the `reddit_search_subreddits` sample.
  - Video and LinkedIn: compare views and likes only within one platform and one query. The median of the top 10 results is the bar a new post has to clear there.
- A question that shows up in two or more sources beats a bigger number in one. The overlap is the strongest signal these tools give.

## Related skills

- Four weeks of dated posts from these ideas: [create-content-calendar](../create-content-calendar/SKILL.md). One piece turned into posts for other channels: [repurpose-content](../repurpose-content/SKILL.md).
- Blog posts and a keyword-led content plan: [create-seo-content-plan](../create-seo-content-plan/SKILL.md); one article's brief: [write-seo-brief](../write-seo-brief/SKILL.md). The ideas here hand Google-bound ideas to those two.
- One platform in depth: [find-youtube-video-ideas](../find-youtube-video-ideas/SKILL.md), [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md).
- Hooks and trends on TikTok and Reels: [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md), [find-instagram-hooks](../find-instagram-hooks/SKILL.md), [find-tiktok-trends](../find-tiktok-trends/SKILL.md), [find-reels-trends](../find-reels-trends/SKILL.md).
- A plan for one platform: [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-youtube-plan](../create-youtube-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md). Which platforms to be on at all: [pick-channels](../pick-channels/SKILL.md).
- Pain points and customer language behind the content: [find-pain-points](../find-pain-points/SKILL.md). Reddit threads and LinkedIn posts to reply to: [find-reddit-threads](../find-reddit-threads/SKILL.md), [find-linkedin-posts-to-comment](../find-linkedin-posts-to-comment/SKILL.md).
