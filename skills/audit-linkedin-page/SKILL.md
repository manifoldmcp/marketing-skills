---
name: audit-linkedin-page
description: When the user wants to know what a LinkedIn company page does and what works for it, their own or a competitor's. Checks whether the page says who it is for, reads its cadence and topic mix, measures likes and comments on its latest posts against two peer pages, and finds who else posts about the company, ending in a scorecard and fixes. Also use when the user mentions a LinkedIn company page audit, a competitor's LinkedIn page, what a brand posts on LinkedIn, is our LinkedIn page working, or benchmarking our company page against competitors. TikTok goes to audit-tiktok-account, Instagram to audit-instagram-account, YouTube to audit-youtube-channel, Facebook to audit-facebook-page, X to audit-x-account; a LinkedIn plan to create-linkedin-plan; post formats across the niche to find-linkedin-post-formats; a rival beyond social to tear-down-competitor.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Audit a LinkedIn company page

An audit of one LinkedIn company page, the user's or a competitor's: whether the page says who it is for, how often it posts and about what, which posts earn engagement against the page's own median, and how it compares with two peers. It ends in a scorecard, with a link to every post it cites, and a short list of fixes.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_get_company` (hosts often add a prefix, for example `mcp__manifold__linkedin_get_company`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `linkedin_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the user's own company page, the competitors and their pages) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Page**: the company slug (the part after `/company/`) or the full page URL.
- **Whose**: the user's own page (the output is fixes) or a competitor's (the output is what to learn and what to avoid).
- **Peers**: two companies to compare with. Default: the competitors the user names.
- **Window**: default the latest 20 posts on the page, 10 on each peer.
- **Budget**: a default run costs about (1 + 3 + 20) + 2 x (1 + 1 + 10) + 2 = 50 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the platform notes** before the first call: [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md).
2. **Read the page.** `linkedin_get_company` (1 credit): `bio` (the description or tagline), `website`, `industry`, `employees` and `location`. Check that the bio says who the company serves and what it does in its first line, that the website is set, and that the industry is the one buyers would expect. The page's follower count is not published here, nor its logo, banner or button; list those for the user to check by eye.
3. **Read the feed.** `linkedin_get_company_posts`, paging until it has the latest 20 posts (1 credit a page, about three). From `created_at` (approximate), posts a week over the last 90 days. From `text`, the topic of each post: product news, customer story, thought leadership, hiring and culture, events, reshared press. Note the length and the hook of each.
4. **Measure engagement.** The feed carries no engagement, so `linkedin_get_post` on each of the latest 20 (1 credit each): `likes` and `comments`. Median per post, and the top five and bottom five. What the top five share (a topic, a person featured, a number in the first line) is the page's best evidence.
5. **Compare with peers.** For each peer: `linkedin_get_company` (1 credit), one page of `linkedin_get_company_posts` (1 credit) and `linkedin_get_post` on its latest 10 (1 credit each). Same measures.
6. **See who talks about the company.** `linkedin_search_posts` with the company name and `since: "month"`, two pages (1 credit a page): employees, customers and partners posting about it. A page whose people post about it reaches far beyond its own feed.
7. **Deliver** a scorecard: item (bio, website, industry, employees, posts a week, median likes, median comments, topic mix, best topic, mentions by others last month), this page, each peer, and a verdict per row. Then the top five posts (URL, first line, likes, comments), and five fixes for the user's page, or five lessons from a competitor's.

## Judgment

- **Own baseline.** Judge a post against its own page's median, never against LinkedIn at large; a peer against the user only over the same window.
- **The people behind the page.** Company pages reach fewer people than the people behind them. A page with modest engagement and employees who post about it is healthier than a page shouting alone.
- **Median, not the best post.** A page with one viral post and twenty posts under ten likes has a cadence problem, not a success. Six outliers mean a repeatable topic; one means luck.
- **Who the likes come from.** Hiring and culture posts often earn the most likes from employees and candidates, and the least from buyers. Separate them when judging what works for pipeline.
- **Headcount lags.** `employees` is LinkedIn's own headcount, not the company's. It lags hires and exits.
- **Dates are approximate.** `created_at` is approximate. Posts a week over 90 days is reliable; the exact day a post went out is not. A rival whose cadence fell over the last month may have cut its effort: say so, it is an opening.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Handoff.** The deliverable is a scorecard with a link to every post it cites. Never post, comment, like, connect or message. A weekly watch of a rival's posts is the host's schedule, through [monitor-competitors](../monitor-competitors/SKILL.md).

## Related skills

- A LinkedIn plan for a founder or a company page: [create-linkedin-plan](../create-linkedin-plan/SKILL.md).
- The post formats, hooks and lengths that win across the niche, not one page: [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md). Who leads the conversation on a topic: [find-linkedin-topic-leaders](../find-linkedin-topic-leaders/SKILL.md).
- The same audit on other platforms: [audit-x-account](../audit-x-account/SKILL.md), [audit-youtube-channel](../audit-youtube-channel/SKILL.md), [audit-facebook-page](../audit-facebook-page/SKILL.md), [audit-instagram-account](../audit-instagram-account/SKILL.md), [audit-tiktok-account](../audit-tiktok-account/SKILL.md). Run each and set the results side by side, comparing pages only within one platform.
- For a competitor's whole marketing beyond this page: [tear-down-competitor](../tear-down-competitor/SKILL.md). Their LinkedIn ads: [research-linkedin-ads](../research-linkedin-ads/SKILL.md).
- Turning the user's own posts into posts for other channels: [repurpose-content](../repurpose-content/SKILL.md).
