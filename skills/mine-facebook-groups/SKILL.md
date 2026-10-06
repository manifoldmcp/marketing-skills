---
name: mine-facebook-groups
description: When the user wants to know what members of Facebook groups ask, complain about and recommend. Reads the posts and discussions of public Facebook groups the user links, or finds some through Google, and clusters them into themes with counts, the brands members recommend, quotes with links, and a list of open posts where the user could help. Also use when the user mentions Facebook group research, Facebook group listening, what people ask in a Facebook group, or the groups where a niche such as Airbnb hosts, parents or local trades talks. Pains ranked across Reddit and every platform go to find-pain-points, Reddit alone to find-reddit-pain-points, comments under Facebook page posts to mine-facebook-comments, a Facebook page audit to audit-facebook-page, a Facebook plan to create-facebook-plan, posting a launch in groups to create-launch-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Mine Facebook groups

What members of Facebook groups ask, complain about and recommend. Many buyers (local businesses, parents, trades, hobbyists, niche professionals) talk in Facebook groups more than anywhere else. There is no group search and no keyword search here, so the job starts from group links; it ends in a table of themes with quotes and links, and a short list of posts where the user could help. In a cross-source run its raw quotes feed the clustering in [find-pain-points](../find-pain-points/SKILL.md).

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `facebook_get_group_posts` and `facebook_get_comments` (hosts often add a prefix, for example `mcp__manifold__facebook_get_group_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `facebook_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop. If only `seo_get_serp` is missing, step 1 cannot find groups: ask the user for group links instead.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the problem and category words, the ICP, the competitors, customer language already collected) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. After delivering, offer to write the pains and the customer language into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md), which says which section each result fills.

## Inputs to settle first

- **Groups**: links to public groups (`facebook.com/groups/...`). Ask the user first: they usually know the groups their customers are in. If they have none, step 1 finds some.
- **Topic**: the problem and category words and competitor names that mark a relevant post. There is no filter parameter on the group tool, so these guide the reading.
- **Depth**: default 5 pages of posts per group, newest first.
- **Budget**: a default run over three groups costs about 3 x 5 + 15 + 3 = 33 credits (five pages of posts per group, fifteen posts' comments, three searches for groups when needed). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find groups if the user has none.** `seo_get_serp` for "<topic> facebook group", "<ICP> facebook group" and "<city or niche> <topic> group" (1 credit each at the default depth, or `depth: 20` for 2). Keep the facebook.com/groups URLs. Google shows only some groups, and only public groups can be read; ask the user to add any they belong to.
2. **Pull the posts.** `facebook_get_group_posts` with the group's `url` (1 credit per page of results), newest first, paging with `meta.cursor` up to 5 pages. Dedupe on `id`. A private group, or a link that is not a group, returns nothing: tell the user and move on.
3. **Keep what matters.** Keep posts that ask a question, describe a problem, ask for a recommendation, or name the category or a competitor. Drop promotions, giveaways and off-topic posts. Note how many days the 5 pages covered: that is the group's pace.
4. **Read the discussions.** `facebook_get_comments` on the 15 kept posts with the most `comments` (1 credit each, one page). Note what members recommend, which tools and brands they name and how often, and the objections to each.
5. **Cluster.** Group the kept posts and comments into themes by the underlying problem, not by the words used: questions people repeat, pains, tools recommended (a count per brand), and how members talk about competitors. Count distinct posts per theme.
6. **Deliver** two tables. Themes: theme, posts (count), a representative quote with the post link, brands named (with counts), and the implication (a message, a content idea, a feature). Open posts: the recent posts that ask something the user's product or knowledge answers and have few comments, with URL, group, age and comments. Say that each group's own rules decide whether the user may answer with the product, and how many posts across how many groups the tables rest on.

## Judgment

- Every finding carries its evidence: a verbatim quote with its post link. Members are private people: quote without names, paraphrase in public copy, and never turn group members into a lead list.
- No tool reads a group's rules. Before the user posts, they read the group's About section. Many groups ban promotion outside a set thread or day, and admins remove members for it. Not stated is not permission.
- The pace tells how useful a group is. If 5 pages reach back only two days, the group is busy and the sample is recent; if they reach back six months, the group is quiet and is not worth watching.
- Group posts are newest first but not a complete window. A count is a count in the sample: use it to rank themes against each other, never to size a market. To watch a group over time, the host pages back to the last `id` it saw and stores it: see [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
- A group run by a vendor or a competitor skews toward that vendor. Say who runs it when the group name or its top posts show it.
- A feature request is a pain stated as a solution: record the problem behind it. A post asking for a recommendation is the strongest intent signal a group carries.
- Never post, comment, join a group or contact anyone. The deliverable is the tables; the user decides whether to answer the open posts. Posting a launch in groups is [create-launch-plan](../create-launch-plan/SKILL.md), which uses this list.
- Credits: if the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.

## Related skills

- Pains ranked across Reddit, Facebook groups and social comments, which uses this skill for groups: [find-pain-points](../find-pain-points/SKILL.md).
- Comments under a Facebook page's own posts: [mine-facebook-comments](../mine-facebook-comments/SKILL.md). The same job on Reddit: [find-reddit-pain-points](../find-reddit-pain-points/SKILL.md).
- A Facebook page's performance: [audit-facebook-page](../audit-facebook-page/SKILL.md). A Facebook plan: [create-facebook-plan](../create-facebook-plan/SKILL.md).
- Launching in the groups found here: [create-launch-plan](../create-launch-plan/SKILL.md). Content ideas from the questions found here: [find-content-ideas](../find-content-ideas/SKILL.md).
