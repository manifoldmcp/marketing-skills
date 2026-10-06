---
name: find-subreddits
description: When the user wants to know which subreddits to take part in and what each one allows. Finds the communities where the buyers ask questions, sized and ranked by how often the topic comes up in a week and how close the members are to the ICP, and reads each community's rules on self promotion into a verdict per action. Also use when the user mentions which subreddits to post in, where our audience hangs out on Reddit, find subreddits about X, best subreddits for our product, subreddit rules, is self promotion allowed there, can I post my product in r/SaaS, or will I get banned for posting a link. A full Reddit plan goes to create-reddit-plan, threads to answer this week to find-reddit-threads, Facebook groups to mine-facebook-groups, communities for a launch day to create-launch-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find subreddits

The communities where the user's buyers ask questions, ranked by how often the topic comes up and how close the members are to the ICP, with each community's stance on self promotion. It ends in a short list the other Reddit skills use (threads to reply, pain points, a launch, a watch), and, before anyone posts, a verdict per community and action from the rules the moderators wrote.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_subreddits` and `reddit_get_subreddit` (hosts often add a prefix, for example `mcp__manifold__reddit_get_subreddit`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `reddit_*` tools are not, the Reddit tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the problem the product solves, the ICP, the competitors, customer language) from it; ask only for what it lacks. Use its customer language as the topic phrasings and `match` terms. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Case**: find communities, or check what named communities allow. A request that names the subreddits and asks about posting ("can I post my product in r/SaaS") is the rules check alone, in [references/subreddit-rules.md](references/subreddit-rules.md), with its own inputs and a default cost of about 25 credits for 10 communities.
- **Topic**: the problem the product solves, in the buyer's words ("cold emails landing in spam", not "deliverability platform"). Ask for two to four phrasings.
- **ICP**: who buys (role, company size, industry), to tell a buyer community from a vendor, hobbyist or job-seeker one.
- **Competitors**: one or two names. Where they are discussed is where buying conversations happen.
- **Known communities**: any the user already reads. They are checked in step 4, not searched for.
- **Budget**: a default run costs about 6 + 12 + 10 = 28 credits (six searches, twelve communities read, one check-in over ten). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the case.** For named communities and a question about what they allow, open [the rules check](references/subreddit-rules.md) and follow it alone. Otherwise start at step 2.
2. **Sample the conversation.** `reddit_search_subreddits` for each topic phrasing and each competitor name, with `time_range: "year"` (1 credit each). Each call counts the communities behind one page of posts, so a quiet community can be missing: that is why several phrasings run. Keep `name`, `subscribers`, `posts_in_sample` and `example_post_url`.
3. **Merge and shortlist.** Merge by name and add up `posts_in_sample`: a community that shows up for several phrasings is where the topic lives. Drop general news and meme communities, `over_18` ones, other languages unless the market is there, and a competitor's own subreddit (a support forum for its users; note it for [find-competitor-complaints](../find-competitor-complaints/SKILL.md)). Add the user's known communities and keep up to 12.
4. **Size and read.** `reddit_get_subreddit` on each (1 credit, cached 7 days): `subscribers`, `weekly_active_users`, `weekly_contributions`, `description`, `rules` and `submit_text`. A community that returns `NoData` cannot be read (private, banned or misspelled): drop it. Apply the activity floor in the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#floors).
5. **Measure the topic.** `reddit_get_new_posts` on the top 10 in one call, with `since: "7d"` and `match` set to the topic terms and competitor names (1 credit per subreddit, 10 in all). The matching posts per community are how often the topic comes up in a week, complete for the window. Where `coverage[]` shows `complete: false`, the count covers only back to `covered_from`: report it as "at least" and scale it to a week, or rerun that community with `pages: 3`. Read the matching titles to see who posts: buyers asking, vendors promoting, or students and job seekers.
6. **Read the rules stance.** From the `rules` and `submit_text` already fetched, mark each community the way [the rules check](references/subreddit-rules.md) does: product mentions allowed, allowed in a set thread or with conditions, not allowed, or not stated. No extra call is needed; run the rules check in full before anyone posts.
7. **Deliver** a table: subreddit, subscribers, weekly contributions, topic posts in the last 7 days ("at least" where coverage was cut), who posts (buyers, vendors, mixed), self-promotion stance, an example thread URL, and one line on why it fits. Rank by topic posts a week first, fit to the ICP second.

## Judgment

- Every Reddit call follows the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md): [search and watching](../create-reddit-plan/references/platforms/reddit.md#search-and-watching), the [floors](../create-reddit-plan/references/platforms/reddit.md#floors), the [evidence](../create-reddit-plan/references/platforms/reddit.md#evidence), the [credits](../create-reddit-plan/references/platforms/reddit.md#credits) and the [handoff](../create-reddit-plan/references/platforms/reddit.md#handoff). Never post, reply or message the moderators for the user.
- Activity beats size. A community with 30 topic posts a week and 40,000 members is worth more than one with 3 million members where the topic comes up twice.
- Look for the ICP's job, not the product category: a sales tool's buyers are in sales communities, not only in communities about CRMs.
- A community where most matching posts are vendors promoting is a poor target: members are tired of pitches and moderators are strict.
- `reddit_search_subreddits` misses quiet communities by design. Ask the user which ones they read, and check them in step 4.
- Where competitors are discussed shows buying conversations; where only the problem is discussed shows research and content opportunities. Say which each community is.

## Related skills

- A plan for Reddit built on this list: [create-reddit-plan](../create-reddit-plan/SKILL.md). Threads to answer in these communities: [find-reddit-threads](../find-reddit-threads/SKILL.md).
- What people complain about in them: [find-reddit-pain-points](../find-reddit-pain-points/SKILL.md) and [find-competitor-complaints](../find-competitor-complaints/SKILL.md).
- A launch day across Reddit and other communities: [create-launch-plan](../create-launch-plan/SKILL.md), which uses this list.
- The same communities watched every day or week: [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
