---
name: find-reddit-threads
description: When the user wants recent Reddit threads to answer. Finds open threads from the last 7 days where someone asks for a tool in the category, describes the problem the product solves or compares competitors, reads each thread and its community's rules, and lists the angle a helpful reply could take. Also use when the user mentions Reddit threads to reply to, posts asking for a tool like ours, where should we comment on Reddit this week, Reddit conversations to join, or Reddit engagement. LinkedIn posts to comment on go to find-linkedin-posts-to-comment, a Reddit plan to create-reddit-plan, which subreddits to join to find-subreddits, Reddit threads AI answers cite to build-ai-citations, what people complain about to find-reddit-pain-points. The user replies from their own account; posting, replying and voting are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Reddit replies

Recent threads where a helpful reply from the user fits: someone asks for a tool in the category, describes the problem the product solves, or compares competitors. Each thread is read before it makes the list, so the list holds only threads that are open, not already answered well, and in communities that allow the reply. It ends in a table the user works through from their own account.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_get_new_posts` and `reddit_search_posts` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `reddit_*` tools are not, the Reddit tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the problem the product solves, what the product does and does not do, the competitors, customer language) from it; ask only for what it lacks. Use its customer language as the search phrasings and `match` terms. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first. After delivering, offer to write new customer language into `.agents/product-marketing.md` with [create-product-context](../create-product-context/SKILL.md).

## Inputs to settle first

- **Topic terms**: the problem in the buyer's words, category words, and competitor names. Up to 10 terms for `match`.
- **Communities**: from [find-subreddits](../find-subreddits/SKILL.md) or the user. Default: the top 5 there; if there are none, run its first search step (a few credits).
- **Window**: default the last 7 days, the most `reddit_get_new_posts` reaches.
- **Product facts**: what the product does and does not do, so each angle is honest.
- **Budget**: a default run costs about 5 + 4 + 20 x 2 + 5 = 54 credits (a check-in over 5 communities, 4 searches, 20 threads read with their comments, the rules of 5 communities). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Collect from the communities.** `reddit_get_new_posts` on the chosen subreddits in one call, with `since: "7d"` and `match` set to the topic terms (1 credit per subreddit per page). This is complete for the window, so nothing recent in those communities is missed. Read `coverage[]` and act on a cut window as the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching) say.
2. **Collect from the rest of Reddit.** `reddit_search_posts` with three or four question phrasings ("best <category> for", "alternative to <competitor>", "how do I <job>", "recommend a <category>"), `sort: "relevance"` and `time_range: "month"` (1 credit each), then keep only rows whose `created_at` is inside the window. Do not sort by `new` across all of Reddit: the search then drops the query, as the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching) say. Merge with step 1 and dedupe on `id`.
3. **Cut from the rows.** Without more calls, drop posts outside the window, `over_18` posts, vendor posts (launches, "I built", promotions: they are not asking), and posts that match a term but not the intent (a job ad, a meme, a news link). Keep posts that ask, compare or describe the problem. Cut to the 20 best, freshest first.
4. **Judge each thread.** `reddit_get_post` (1 credit): drop it if `locked` or `archived` is true, or the body is "[removed]" or "[deleted]"; read the full `body` for what the poster needs. `reddit_get_comments` (1 credit, one page): is there already a good answer (a top-level comment, `depth: 0`, with a high `score` that names a fitting tool or solves the problem); which products are recommended; has the poster replied to anyone. A thread with under 10 comments and no good answer is the best target; one with 200 comments buries a new reply.
5. **Check the rules.** For each community on the list, the verdict from the rules check in [find-subreddits](../find-subreddits/SKILL.md) (`reddit_get_subreddit`, 1 credit per community, free if read this week). Where product mentions are not allowed, keep the thread only as "answer without the product", or drop it.
6. **Deliver** a table: thread title, URL, subreddit, age in hours, comments, what the poster needs (one line), answered already (no, partly, yes), products already recommended, rules verdict (a link, a mention, or neither), and the angle: what a helpful reply would say, in one line, using a detail from the post. Freshest open unanswered threads first. Write no drafts unless the user asks; then follow the drafting rules in the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#handoff).

## Judgment

- Every Reddit call follows the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md): [search and watching](../create-reddit-plan/references/platforms/reddit.md#search-and-watching), the [floors](../create-reddit-plan/references/platforms/reddit.md#floors), the [evidence](../create-reddit-plan/references/platforms/reddit.md#evidence), the [credits](../create-reddit-plan/references/platforms/reddit.md#credits) and the [handoff](../create-reddit-plan/references/platforms/reddit.md#handoff).
- Help first. A reply worth suggesting answers the question in the thread on its own; the product comes in only where the community allows it and the asker would want it.
- Never post, reply, comment, vote or send a message. The deliverable is the table with a link to every thread; the user acts from their own account.
- Speed matters more than polish. The poster and the voters read replies in the first day or two; after a week only search visitors do, unless the thread ranks on Google or AI engines cite it ([build-ai-citations](../build-ai-citations/SKILL.md)).
- A thread answered well is not a target, unless the top answer is outdated or wrong. Then the angle is the correction.
- If the angle would fit any thread, it is spam. Every angle must use something only this post said.
- Mention the product only where it answers the question. A plain helpful answer with no product builds the history that makes later mentions credible.
- "Tools like <competitor>" and "alternative to <competitor>" posts have the highest intent. Put them first when they are fresh.
- Spread replies out. Many similar comments from one account in one day look like spam to Reddit's filters and to moderators.
- One run is a snapshot. To get this list every day or week, the host schedules it the way [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md) does; the server keeps no state.

## Related skills

- Which communities to watch, and what each allows: [find-subreddits](../find-subreddits/SKILL.md). A plan for Reddit as a whole: [create-reddit-plan](../create-reddit-plan/SKILL.md).
- Where to engage this week for business software, where buyers often post on LinkedIn: [find-linkedin-posts-to-comment](../find-linkedin-posts-to-comment/SKILL.md).
- Threads where people leave a named competitor: [find-competitor-complaints](../find-competitor-complaints/SKILL.md). The problems behind the threads, ranked: [find-reddit-pain-points](../find-reddit-pain-points/SKILL.md).
- Reddit threads that Google and AI engines show for the category: [build-ai-citations](../build-ai-citations/SKILL.md).
