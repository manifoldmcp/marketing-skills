---
name: create-facebook-plan
description: When the user wants a plan to grow on Facebook, for a page and the groups its buyers are in. Writes a Facebook strategy for local and small businesses and audiences over 35 from what the page does now, what earns a response for the pages and groups around it, and which tactics close the gap, ending in a 30-60-90 day plan with KPIs the tools can measure again. Also use when the user mentions a Facebook strategy, growing a Facebook page for a local business, Facebook for a shop, clinic or practice, using Facebook groups to get customers, or a 90-day Facebook plan. A plan for TikTok goes to create-tiktok-plan, Instagram to create-instagram-plan, YouTube to create-youtube-plan, LinkedIn to create-linkedin-plan; which platforms to be on at all to pick-channels; a competitor's page to audit-facebook-page, group research to mine-facebook-groups, Facebook ads to research-meta-ads.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Facebook plan

A Facebook plan is for the businesses whose buyers still live there: local and small businesses, and audiences over 35. It answers what the page does now, what earns a response for the pages and groups around it, and which Facebook tactics close the gap for the hours the team has. It pulls in other skills as tactics (page audits, group mining, comment mining, ads) and ends in a 30-60-90 day plan for the page and the groups, not a list of posts.

This skill also holds the [Facebook notes](references/platforms/facebook.md) that every skill reading Facebook follows.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `facebook_get_posts` and `facebook_get_group_posts` (hosts often add a prefix, for example `mcp__manifold__facebook_get_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `facebook_*` tools are not, the Facebook tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- The baseline reads ads with `ads_get_advertiser_ads`, and the group search uses `seo_get_serp`. If the `ads_*` tools are off, deliver without the ads; if the `seo_*` tools are off, ask the user for the group links.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the page, the area, the ICP, the competitors and their pages, the brand voice, the goal) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it. If the user has not chosen Facebook yet, or asks which platforms to be on, run [pick-channels](../pick-channels/SKILL.md) first.

- **Goal**: local footfall and calls, leads, sales, or community. Default: leads, measured in comments and messages the posts start.
- **Page**: the page name or URL. With no page yet, baseline the competitors only.
- **Area**: the town or region for a local business, or the niche for one that sells everywhere. It decides which groups count.
- **ICP**: who buys, so the groups and the topics are theirs.
- **Budget**: credits for the research (this plan costs about 40), hours a week, and any money for boosting or ads. Default: 300 credits, 3 hours a week, no paid budget.
- **Team**: who can be on camera, who answers comments and messages. Default: the owner, filmed on a phone.
- **Competitors**: two or three pages, ideally businesses in the same area or niche. Default: the pages the user names; ask if there are none, since Facebook has no page search here.
- **Groups**: public groups the buyers are in. Default: the ones the gaps step finds.
- **Horizon**: default 90 days.

## Steps

1. **Read the Facebook notes.** Before the first call, read the [Facebook notes](references/platforms/facebook.md): what the tools can read, how to read engagement, what each call costs and the handoff. They hold for every skill this plan pulls in.
2. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
3. **Baseline.** Say the cost first: about 28 credits for the user's page and three competitors. For each page, `facebook_get_profile` (1 credit) for `followers`, `industry` and `website`; `facebook_get_posts`, paging until `created_at` passes 90 days or 5 pages (1 credit a page), for posts a week, the share of videos (`media: "video"`), median `likes` plus `comments` per 1,000 followers and median video `views`; and `ads_get_advertiser_ads` with `platform: "facebook"`, the page name and `active_only: true` (1 credit) for the ads it runs now. Read the numbers as the [Facebook notes](references/platforms/facebook.md#engagement) say: Facebook rows mark no paid posts, so an organic post whose first line an ad's `body` repeats is one the page boosted; keep it out of the organic medians.
4. **Gaps.** About 12 credits more.
   - Format and topic gap: each competitor's posts at 3 times its own median engagement or more, by format (video or not), topic and first line, against what the user's page posts.
   - How winners talk: `facebook_get_transcript` on the three top competitor videos (1 credit each). Read the first spoken line and the offer.
   - What the audience asks: `facebook_get_comments`, one page on the three most commented competitor posts (1 credit each). Count the questions and the objections.
   - Groups: if the user named none, `seo_get_serp` for "<area> <category> group" and "<ICP> facebook group" (1 credit each) and keep the facebook.com/groups URLs. Then `facebook_get_group_posts`, two pages on the two groups that fit best (1 credit a page): how fast they post and what members ask or recommend.
5. **Tactics.** From the gaps, choose two or three of these skills and say why each fits the numbers:
   - [audit-facebook-page](../audit-facebook-page/SKILL.md) for the page audit when one competitor clearly beats the user on engagement per 1,000 followers and the user needs to know how, or for video transcripts when the competitors' best posts are videos: their hooks and claims, in words.
   - [mine-facebook-groups](../mine-facebook-groups/SKILL.md) when the buyers talk in groups: the questions to answer there and in posts.
   - [mine-facebook-comments](../mine-facebook-comments/SKILL.md) when the competitors' comments are full of questions or complaints.
   - [research-meta-ads](../research-meta-ads/SKILL.md) when the competitors run many long-lived ads: their presence on Facebook is paid, and the ads hold their best offers.
   - [create-instagram-plan](../create-instagram-plan/SKILL.md) when the same buyers are on Instagram: the reels made for one can run on both.
6. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week. By day 30, a cadence the team can hold (default three posts a week, at least one a video) on the topics that won for competitors, and one helpful answer a day in the chosen groups where their rules allow. By day 60, keep the format with the best median engagement and turn the top comment and group questions into posts. By day 90, a recurring series (a weekly tip, an offer, a customer story), and a boosting or ads test through the [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md) skill if there is budget.
   - KPIs the tools can measure again later: followers (`facebook_get_profile`), posts a week, median engagement per 1,000 followers and median video views (`facebook_get_posts`), and active ads (`ads_get_advertiser_ads`). Messages, calls, reach and clicks are not visible to the tools; the user reads them in Meta Business Suite.
7. **Deliver** one document: the inputs with defaults marked, a baseline table (the user against each competitor: followers, posts a week, video share, median engagement per 1,000 followers, median video views, active ads), the groups with their pace and top questions, the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions.

## Judgment

- **Facebook notes first.** They hold for every skill the plan pulls in.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Plans, not posts.** The deliverable is a plan with a link to every page, group and post it cites. Never post, comment, react, join a group, send a message or schedule anything. Draft a post or a reply only when the user asks, and base each on a pattern the evidence shows.
- **Measure again.** Every KPI in the plan is one the tools can read again later. A weekly check is the host's schedule, as in [monitor-competitors](../monitor-competitors/SKILL.md).
- A page's posts reach a small share of its followers. Facebook fills much of the feed with recommended posts from pages people do not follow, and reels travel furthest that way: a growth plan leads with short video.
- Engagement per 1,000 followers is the live number. A page with 20,000 followers and five reactions a post is not working, whatever its size.
- For a local business, groups often beat the page: a useful answer in a town group reaches more buyers than a post. Each group's rules decide whether the product may be named; the user reads them first. Never post on the user's behalf.
- Much of Facebook watches with the sound off, so text burned into a video carries the hook. Transcripts cannot see it; when a top video's transcript is thin, say so.
- The tools cannot see reviews and ratings, messages, reach or clicks. For a local business, reviews often matter more than posts; ask the user what theirs say.
- Personal profiles are not covered. If the founder posts from a personal profile, the user brings those numbers.

## Related skills

- Which platforms to be on at all: [pick-channels](../pick-channels/SKILL.md). The whole growth plan: [create-growth-plan](../create-growth-plan/SKILL.md).
- The same plan on another platform: [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-youtube-plan](../create-youtube-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md), [create-reddit-plan](../create-reddit-plan/SKILL.md).
- A competitor's page and its videos: [audit-facebook-page](../audit-facebook-page/SKILL.md). What people ask in Facebook groups and comments: [mine-facebook-groups](../mine-facebook-groups/SKILL.md), [mine-facebook-comments](../mine-facebook-comments/SKILL.md).
- A content calendar and repurposing: [create-content-calendar](../create-content-calendar/SKILL.md), [repurpose-content](../repurpose-content/SKILL.md).
- Ads on Facebook: [research-meta-ads](../research-meta-ads/SKILL.md), [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
