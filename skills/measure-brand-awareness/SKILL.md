---
name: measure-brand-awareness
description: When the user wants more people to know the brand. Measures the brand against competitors on what awareness leaves behind (branded search volume and its trend, AI volume, share of voice in ChatGPT, Gemini, Claude, Perplexity and AI Overviews, followers on social, Reddit posts and videos by others, referring domains), finds the two weakest measures and picks the levers that move them. Also use when the user mentions a brand awareness plan, nobody knows who we are, share of voice against competitors, grow branded search, get our name out there, or getting the brand known. Tracking mentions over time goes to monitor-brand-mentions, AI share of voice alone to check-ai-visibility, a press plan to create-digital-pr-plan, a growth plan by any other goal to create-growth-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Brand awareness

Awareness is how many buyers know the brand before they need it. The tools cannot survey people, but they measure what awareness leaves behind: people searching the brand name, AI answers naming it, reach on social, other people talking about it, and coverage linking to it. This skill measures the brand against competitors on each, finds the weakest, and routes to the skills that move it.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_keyword_metrics` and `aeo_run_ai_answers` (hosts often add a prefix, for example `mcp__manifold__seo_get_keyword_metrics`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The steps read `seo_*`, `aeo_*`, `reddit_*` and the social platforms. If some of these tool groups are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, carry on, and mark those measures "not measured" rather than calling them weak.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand and its domain, the competitors, the audience, the social accounts and the channels the team can run) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Brand**: the name, its variants and the domain. If the name is a common word, add the qualifier people use ("linear app").
- **Competitors**: two or three the user wants to be named alongside.
- **Audience**: who should know the brand, so the platforms and questions fit them.
- **Channels**: what the team can do (video, writing, press, events, paid). Default: writing and founder posts.
- **Horizon**: default 90 days. Branded search moves slowly, so the first real read is at day 90.
- **Budget**: about 10 + 54 + 16 + 4 + 8 + 40 = 132 credits for the brand and three competitors. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Branded search.** `seo_get_keyword_metrics` with the brand and each competitor's name as `keywords` and `ai_volume: true` (about 10 credits). Read `volume` and `trend[12]`. The brand's share of branded search is its volume over the sum; the 12-month trend says whether the gap is closing. `ai_volume` is the same share in AI engines: how often they see each name.
2. **Share of voice in AI answers.** `aeo_run_ai_answers` with three questions a buyer in the category would ask, and `brands` set to the brand and the competitors (18 credits a prompt), then `get_task`. For each brand, the share of the 15 cells (prompt by engine) where it is mentioned. The full measurement is [check-ai-visibility](../check-ai-visibility/SKILL.md).
3. **Social reach.** The profile tools for each brand on up to four platforms the audience uses (`linkedin_get_company`, `instagram_get_profile`, `tiktok_get_profile`, `youtube_get_channel`, `twitter_get_profile`; 1 credit each): `followers` and `posts_count`.
4. **Other people talking.** `reddit_search_posts` with each brand name at the default relevance sort (1 credit each), and `youtube_search_videos` and `tiktok_search_videos` with each name and `since: "year"` (1 credit each). Count the rows from the last 12 months that are about the brand and not by it. Use the same query shape for every brand, since each search is a ranked sample (the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md) explains).
5. **Coverage.** `seo_get_backlink_summary` on each domain (10 credits each): `referring_domains`, the trace press and mentions leave.
6. **Choose the levers.** For the two weakest measures against the competitors, and what the team can do:
   - Press coverage and mentions: [create-digital-pr-plan](../create-digital-pr-plan/SKILL.md).
   - AI answers: [create-ai-search-plan](../create-ai-search-plan/SKILL.md).
   - Reach through other people's audiences: [create-influencer-plan](../create-influencer-plan/SKILL.md).
   - The brand's own social: [create-linkedin-plan](../create-linkedin-plan/SKILL.md), [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md) or [create-youtube-plan](../create-youtube-plan/SKILL.md), with [pick-channels](../pick-channels/SKILL.md) to choose among them.
   - Paid reach: [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
   - Keeping count: [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md) and [check-ai-visibility](../check-ai-visibility/SKILL.md) on a schedule.
7. **Deliver** a share-of-voice table (brand, branded searches a month and the 12-month trend, AI volume, AI mentions out of 15, followers per platform, Reddit posts and videos about it in a year, referring domains), the two weakest measures with the reason, the chosen levers with the linked skill, the KPIs to measure again at day 90 (the same calls, questions and competitors), and three first actions.

## Judgment

- Branded search is the cleanest awareness measure the tools have: nobody searches a name they do not know. It is monthly and slow; read it at day 90, not day 30.
- Followers measure a brand's own reach, not awareness. Mentions by others and branded search say more.
- Share of voice needs a fixed set: the same competitors, questions and queries every time, or the numbers move for the wrong reason.
- A common-word brand inflates its branded volume with unrelated searches. Use the qualified form, and check with `seo_get_serp` on the name (1 credit) that the results are about the brand.
- Awareness with nowhere to land leaks. Before paying for reach, make sure the brand's own search results (site, reviews, profiles) answer someone who looks it up.
- Counts from Reddit, TikTok and YouTube search are ranked samples, not totals. Compare brands only with the same queries run on the same day.
- Say the estimate before the first paid call; `dry_run: true` prices any call for free. The server keeps no state: the host keeps the table so day 90 can compare.

## Related skills

- A growth plan when the goal is not only awareness: [create-growth-plan](../create-growth-plan/SKILL.md).
- New mentions of the brand, every day or week: [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
- AI share of voice in depth, once or tracked: [check-ai-visibility](../check-ai-visibility/SKILL.md).
- Press and coverage: [create-digital-pr-plan](../create-digital-pr-plan/SKILL.md).
- A launch that concentrates attention on a date: [create-launch-plan](../create-launch-plan/SKILL.md).
