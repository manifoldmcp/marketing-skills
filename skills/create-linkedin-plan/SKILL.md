---
name: create-linkedin-plan
description: When the user wants a plan to grow on LinkedIn, for a founder, the company page, or both. Writes a LinkedIn strategy from whose voice carries the account, what the ICP already engages with, and which tactics get the most reach and pipeline for the hours the team has, ending in a 30-60-90 day plan with KPIs the tools can measure again. Also use when the user mentions a LinkedIn strategy, founder-led content, personal branding or thought leadership on LinkedIn, growing a LinkedIn company page, LinkedIn for pipeline, or a 90-day LinkedIn plan. A plan for TikTok goes to create-tiktok-plan, Instagram to create-instagram-plan, YouTube to create-youtube-plan, Facebook to create-facebook-plan; post formats to find-linkedin-post-formats, posts to comment on to find-linkedin-posts-to-comment, topic leaders to find-linkedin-topic-leaders, a competitor's page to audit-linkedin-page.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# LinkedIn plan

A LinkedIn plan settles whose voice carries the account (a founder, the company page, or both), what the ICP already engages with on LinkedIn, and which LinkedIn tactics get the most reach and pipeline for the hours the team has. It pulls in other skills as tactics (topic leaders, post formats, posts to comment on, page audits) and ends in a 30-60-90 day plan, not a stack of drafts.

This skill also holds the [LinkedIn notes](references/platforms/linkedin.md) that every skill reading LinkedIn follows.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_search_posts` and `linkedin_get_post` (hosts often add a prefix, for example `mcp__manifold__linkedin_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `linkedin_*` tools are not, the LinkedIn tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the founder's profile, the company page, the ICP, the competitors, the brand voice, the goal) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it. If the user has not chosen LinkedIn yet, or asks which platforms to be on, run [pick-channels](../pick-channels/SKILL.md) first.

- **Mode**: founder-led (a person's profile), the company page, or both. Ask for the profile URL and the company page. Default: founder-led, with the page resharing.
- **Goal**: pipeline and leads, hiring, awareness, or investors. Default: pipeline, measured by conversations the posts start.
- **Stage**: pre-launch, early customers, or established. It decides whether the story is building in public or results.
- **ICP**: the titles and companies that buy, so the topics are theirs and not the founder's peers'.
- **Budget**: credits for the research (this plan costs about 60) and hours a week. Default: 500 credits, 3 posts a week and 20 minutes a day for comments.
- **Team**: who posts, who writes, whether employees will reshare. Default: the founder writes and posts, no ghostwriter.
- **Competitors**: two or three companies, and two or three people in the space whose audience overlaps. Default: the companies the user names, and the people from the gaps step.
- **Horizon**: default 90 days.

## Steps

1. **Read the LinkedIn notes.** Before the first call, read the [LinkedIn notes](references/platforms/linkedin.md): what LinkedIn shows, the thresholds, what each call costs and the handoff. They hold for every skill this plan pulls in.
2. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
3. **Baseline.** Say the cost first: about 30 credits for both modes.
   - Founder: `linkedin_get_profile` on the founder (1 credit) for `followers` and the `bio`. There is no way to list a person's posts, so ask for the URLs of their last 10 and read each with `linkedin_get_post` (1 credit each): median likes and comments, and how often they post. With no URLs, `linkedin_search_posts` on the founder's two main topics with `since: "month"` (1 credit a page) and keep the rows whose `author_name` matches; mark the baseline as partial.
   - Company: `linkedin_get_company` on the page and each competitor (1 credit each) for the `bio`, `website`, `industry` and `employees`; `linkedin_get_company_posts` for each (1 credit a page) for posting cadence and topics; `linkedin_get_post` on the page's last 10 posts (1 credit each) for engagement, since the feed carries none.
4. **Gaps.** About 30 credits more.
   - Topic gap: `linkedin_search_posts` for four ICP topics with `since: "month"`, two pages each (1 credit a page). Name the topics and angles with the most engagement, and whether the founder or the page posts on any of them.
   - Voice gap: the authors of the top posts, traced to a profile per the [LinkedIn notes](references/platforms/linkedin.md#what-linkedin-shows) (1 credit each, top 10): their followers and engagement per 1,000 followers against the founder's. These are the people to learn from and comment on.
   - Page gap, in company mode: `linkedin_get_post` on the strongest competitor's last 10 posts (1 credit each): what they post that earns more than the user's page.
5. **Tactics.** From the gaps, choose two or three of these skills and say why each fits the numbers:
   - [find-linkedin-topic-leaders](../find-linkedin-topic-leaders/SKILL.md) when the founder does not know who owns the conversation: the people to learn from, comment on and partner with.
   - [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md) when the founder posts but engagement is low: the structures, hooks and lengths that work in this niche.
   - [find-linkedin-posts-to-comment](../find-linkedin-posts-to-comment/SKILL.md) when the founder has under a few thousand followers: comments on bigger posts in the niche reach the audience before the founder's own posts can.
   - [audit-linkedin-page](../audit-linkedin-page/SKILL.md) for the company page audit in company mode, or when a competitor's page clearly outperforms.
   - [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md) when the goal is pipeline: people already asking for what the user sells.
6. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - By day 30, a posting cadence the founder keeps (default three a week) on the two topics with the most engagement, and a daily list of posts to comment on. By day 60, the formats that worked doubled and one recurring series. By day 90, a second topic, and in company mode the page resharing the founder with its own weekly post.
   - KPIs the tools can measure again later: founder followers (`linkedin_get_profile`), median likes and comments on the last 10 posts (`linkedin_get_post` on each URL; the host keeps the URLs, since no feed can be listed), company page posts a week (`linkedin_get_company_posts`) and their median engagement (`linkedin_get_post`), and how often the founder appears among the authors of the top results for the topic searches (`linkedin_search_posts`). Conversations and leads the posts start are not visible to the tools; the user counts them.
7. **Deliver** one document: the inputs with defaults marked, a baseline table (founder or page against each competitor and leader: followers where published, posts a week, median likes and comments), the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions.

## Judgment

- **LinkedIn notes first.** They hold for every skill the plan pulls in.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Plans, not posts.** The deliverable is a plan with a link to every profile, page and post it cites. Never post, comment, like, connect, message or schedule anything. Draft a post or a comment only when the user asks, and base each on a pattern the evidence shows.
- **Measure again.** Every KPI in the plan is one the tools can read again later. A weekly check is the host's schedule, as in [monitor-competitors](../monitor-competitors/SKILL.md).
- For an early B2B company, a person's posts usually reach far more people than the company page's. Lead with the founder; the page reshares and covers hiring and product news.
- Consistency beats volume. Three posts a week for 90 days beat daily posts for three weeks and then silence.
- Engagement from peers and other founders feels good and rarely buys. Weight topics by what the ICP engages with, and ask the user who writes to them after a post.
- Comments come before followers. A founder with 800 followers grows faster through ten thoughtful comments a day on the right posts than through a fourth weekly post.
- The tools cannot see the founder's feed, the post type, or who engaged. Say what the plan assumes because of it, and let the user fill those gaps with what they see in their own analytics.

## Related skills

- Which platforms to be on at all: [pick-channels](../pick-channels/SKILL.md). The whole growth plan: [create-growth-plan](../create-growth-plan/SKILL.md). Outbound to the same buyers: [create-outbound-plan](../create-outbound-plan/SKILL.md).
- The same plan on another platform: [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-youtube-plan](../create-youtube-plan/SKILL.md), [create-facebook-plan](../create-facebook-plan/SKILL.md), [create-reddit-plan](../create-reddit-plan/SKILL.md).
- On LinkedIn: [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md), [find-linkedin-topic-leaders](../find-linkedin-topic-leaders/SKILL.md), [find-linkedin-posts-to-comment](../find-linkedin-posts-to-comment/SKILL.md), [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md). A content calendar and repurposing: [create-content-calendar](../create-content-calendar/SKILL.md), [repurpose-content](../repurpose-content/SKILL.md).
- A competitor's company page: [audit-linkedin-page](../audit-linkedin-page/SKILL.md). Ads on LinkedIn: [research-linkedin-ads](../research-linkedin-ads/SKILL.md), [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
- Podcasts the founder could guest on: [find-podcasts](../find-podcasts/SKILL.md).
