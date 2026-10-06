---
name: pick-channels
description: When the user wants to know which marketing channels to be on. Measures demand for the topic on Google, AI engines, Reddit, YouTube, TikTok, LinkedIn and Instagram with one or two cheap calls each, checks where competitors already show up and still post, and ends in a ranked channel table with the evidence and the effort each channel takes. Also use when the user mentions which channels should we be on, where does our audience hang out, TikTok or LinkedIn for us, what channel mix for a B2B startup, where should we post, or whether to be on a platform at all. A full plan by goal with a 30-60-90 goes to create-growth-plan, the plan for one platform already chosen to create-tiktok-plan, create-instagram-plan, create-youtube-plan, create-linkedin-plan, create-facebook-plan or create-reddit-plan, paid channels to create-paid-ads-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Marketing channels

Which channels to use is a question about where buyers already are and what they do there: search Google, ask ChatGPT, argue on Reddit, watch YouTube or TikTok, read LinkedIn. This skill measures each channel for the user's topic with one or two cheap calls, checks where competitors already show up, and ends in a ranked channel table with the evidence and the effort each channel takes.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `reddit_search_subreddits` (hosts often add a prefix, for example `mcp__manifold__seo_search_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The steps read `seo_*`, `aeo_*`, `reddit_*` and the social platforms. If some of these tool groups are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, carry on, and mark those channels "not measured" in the table rather than calling them weak.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the topic in the buyer's words, the audience, the competitors with their domains and handles, the team and the market) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Topic**: two or three phrases buyers use for the problem or the category ("expense tracking for freelancers", "invoice app"). Not the brand name: a new brand has no demand of its own yet.
- **Audience**: business or consumer, and the role or the person. It breaks ties between channels the evidence rates alike.
- **Competitors**: two or three, with their domains and, if the user knows them, their handles. Handles usually sit in the footer of the competitor's site (the host can open it if it has a browser), or show up as `author` in the search rows of steps 4 and 5. Default: the brands that recur in steps 2, 4 and 5.
- **Team**: who makes content and what they can make: writing, talking on camera, design. Default: one person, writing only, 4 hours a week. It sets the effort column.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 2 x 10 + 2 + 36 + 2 + 4 + 2 + 3 x (5 + 4 + 4) = 105 credits for two topic phrases and three competitors. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Google.** `seo_search_keywords` with each topic phrase as `seed` (10 credits each). Sum the `volume` of the keywords that fit, apply the Google floor (see Judgment), and note the question and comparison keywords (how, what, best, vs, alternatives). Then `seo_get_serp` for the two biggest keywords (1 credit each): a `video` entry in `features[]` means Google shows videos for the query, so YouTube content can rank there too; rows with `type: "discussions_and_forums_element"` are Reddit and forum threads Google puts on page one.
2. **AI engines.** `aeo_search_prompts` with `keyword` set to the main topic and `limit: 20` (36 credits). Count the prompts and read `ai_search_volume`. `cited_domains[]` shows which sources the engines lean on: youtube.com and reddit.com there point to those channels, and a competitor's domain there means it is already in the answers.
3. **Reddit.** `reddit_search_subreddits` with each topic phrase and `time_range: "year"` (1 credit each). Keep communities with 3 or more posts in the sample and read `subscribers`. Several such communities mean Reddit is a channel for answers and replies ([create-reddit-plan](../create-reddit-plan/SKILL.md) runs it); none means the conversation happens elsewhere.
4. **Video.** `youtube_search_videos` and `tiktok_search_videos` with each topic phrase, `since: "year"` and `sort: "popular"` (1 credit a page each). Read the views of the top 10 and who made them: brands, independent creators, or nobody relevant. Many views and few brands is an opening; when every top video is many months old, nobody serves the topic now.
5. **LinkedIn.** `linkedin_search_posts` with each topic phrase and `since: "month"` (1 credit a page). Count the posts, read likes and comments where published, and note who posts: vendors, practitioners or creators. For a consumer product, add `instagram_search_posts` with the category hashtag (1 credit a page).
6. **Competitor presence.** For each competitor:
   - `seo_get_domain_overview` on its domain (5 credits): `organic_traffic` and `organic_keywords` say whether search is one of its channels.
   - The profile on each platform where it has a handle (1 credit each): `youtube_get_channel`, `tiktok_get_profile`, `linkedin_get_company`, `twitter_get_profile`, or `instagram_get_profile` for a consumer brand. Read `followers` and `posts_count`.
   - A listing on the channels where the account is real (1 credit a page each): `youtube_get_videos`, `tiktok_get_videos`, `linkedin_get_company_posts`, `twitter_get_tweets`. The newest `created_at` says whether it still posts; views say whether anyone watches.

   Also mark where competitors turned up for free in steps 2, 4 and 5 (in `cited_domains[]`, or as `author` in search rows).
7. **Score each channel.** For Google, AI answers, Reddit, YouTube, TikTok, LinkedIn, and Instagram or X where they apply: demand (does it clear the floor for its source), competition (absent, present, strong), and effort for this team. A channel with demand and weak competition that the team can make content for ranks first. Recommend one primary and one secondary channel; list the rest as later.
8. **Deliver** a table: rank, channel, demand (the number and its source), best evidence (a keyword, a prompt, a thread or a video URL), competitors present (who, followers or traffic, still active or not), gap (open, contested, crowded), effort for this team (hours a week and the skill it needs), first move (the skill that starts it, such as [find-content-ideas](../find-content-ideas/SKILL.md) or the platform's plan, like [create-linkedin-plan](../create-linkedin-plan/SKILL.md)).

## Judgment

- Each source counts in its own unit, so each has its own floor:
  - Google: a keyword with `volume` of about 50 a month or more. Below that, even the first position brings a handful of visits.
  - AI engines: any prompt `aeo_search_prompts` returns, since the index holds only prompts it has seen answered. Rank by `ai_search_volume`, but never quote it as searches: it is a People Also Ask proxy.
  - Reddit: a thread with 10 or more comments in the last year, or a community with 3 or more posts in the `reddit_search_subreddits` sample.
  - Video and LinkedIn: compare views and likes only within one platform and one query. The median of the top 10 results is the bar a new post has to clear there.
- Do not add the units up. Searches, prompts, comments and views measure different things; each channel clears its own floor or does not, and the ranking comes from demand, competition and effort together.
- Every row names its evidence. The tools see what people search, ask and watch, not what converts; say so when the user asks which channel will bring customers.
- A crowded channel is not closed. Take it when demand is large and the team has an angle the incumbents lack; take the open channel first when the team is small.
- Two channels done every week beat five done badly. A team of one gets one primary channel and, at most, one secondary.
- A new category often has no Google volume yet (null or under the floor). Demand shows up first in Reddit threads and AI prompts; weigh those more for a new category.
- X cannot be measured here: there is no search, so only a competitor's X account can be sized. Say so rather than rating X low.
- Facebook groups can be measured only from group URLs the user supplies; [mine-facebook-groups](../mine-facebook-groups/SKILL.md) reads them.
- `seo_get_serp_competitors` returns sites that share search results, not business competitors. Do not use it to name competitors here.
- Say the estimate before the first paid call; `dry_run: true` prices any call for free. A result this account already paid for is free while cached (7 days for most search data).

## Related skills

- A plan by goal across paid and organic, with a 30-60-90: [create-growth-plan](../create-growth-plan/SKILL.md).
- Paid channels (Meta, Google, LinkedIn, TikTok ads): [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
- The strategy for the chosen channel: [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-youtube-plan](../create-youtube-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md), [create-facebook-plan](../create-facebook-plan/SKILL.md), [create-reddit-plan](../create-reddit-plan/SKILL.md), [create-seo-plan](../create-seo-plan/SKILL.md) or [create-ai-search-plan](../create-ai-search-plan/SKILL.md).
- What to post there: [find-content-ideas](../find-content-ideas/SKILL.md) and [create-content-calendar](../create-content-calendar/SKILL.md).
- The product, the audience and the competitors written down once: [create-product-context](../create-product-context/SKILL.md).
