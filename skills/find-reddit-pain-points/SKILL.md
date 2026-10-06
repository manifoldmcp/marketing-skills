---
name: find-reddit-pain-points
description: When the user wants the problems people describe on Reddit, in their own words. Searches Reddit posts and comments for how people talk about the problem, reads the richest threads, and clusters every statement of a problem into themes with thread counts, intensity, verbatim quotes with links, what people tried, and the phrases they repeat. Also use when the user mentions Reddit pain points, what Redditors complain about, customer language from Reddit, or voice of the customer from subreddits. Pains ranked across Reddit and other platforms go to find-pain-points, Facebook groups to mine-facebook-groups, comments under social posts to mine-tiktok-comments, mine-instagram-comments, mine-youtube-comments or mine-facebook-comments, complaints about a named competitor to find-competitor-complaints, threads to reply in to find-reddit-threads, which subreddits to use to find-subreddits.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find Reddit pain points

The problems people describe in the user's space on Reddit, in their own words, clustered into themes with counts, quotes and links. Reddit is where people describe a problem before they know what to buy, so it gives the language for positioning, copy, content and the roadmap. It ends in a table of themes; in a cross-source run its raw quotes feed the clustering in [find-pain-points](../find-pain-points/SKILL.md).

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_posts`, `reddit_search_comments` and `reddit_get_comments` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `reddit_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: this skill has no other source.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the problem the product solves in the buyer's words, the ICP, the competitors, customer language already collected) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. After delivering, offer to write the pains and the customer language into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md), which says which section each result fills.

## Inputs to settle first

- **Space**: the job the buyer is trying to get done and the problem around it, not only the category name. Ask for two to four phrasings ("chasing late invoices", "freelance bookkeeping").
- **Communities**: default the top three from [find-subreddits](../find-subreddits/SKILL.md); if there are none, run its first search step (a few credits).
- **Window**: default the past year, for current language and tools. Older pains may be solved already.
- **Competitors**: optional. Complaints about named competitors are [find-competitor-complaints](../find-competitor-complaints/SKILL.md); here they only tag a theme.
- **Budget**: a default run costs about 12 + 3 + 15 + 3 = 33 credits (twelve post searches, three comment searches, fifteen threads read, three cut posts read in full). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the problem, not the product.** `reddit_search_posts` with six phrasings built from how people talk about the problem: "struggling with <job>", "how do you <job>", "<job> is killing me", "hate <task>", "frustrated with <category>", "is there a better way to <job>", with `time_range: "year"` and the default relevance sort (1 credit each). Then the two best phrasings with `subreddit` set to each of the three communities (6 credits). Apply the on-topic check in the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching).
2. **Search the comments.** `reddit_search_comments` with three of the phrasings and `time_range: "all"` (1 credit each), keeping the rows whose `created_at` is inside the window. People describe a pain in replies ("same here, we lost a client over this") more than in titles.
3. **Read the richest threads.** Take the 15 threads with the most `comments` whose title or body states a problem, and call `reddit_get_comments` on each (1 credit, one page). Call `reddit_get_post` (1 credit) only where the row's body was cut at 2,000 characters and the rest matters. Read the busiest first, and stop early once five threads in a row add no new problem; say so.
4. **Extract and cluster.** From posts and comments, pull every statement of a problem: what went wrong, what they tried, what it costs them (hours, money, a client, a risk), and what they wish existed. Cluster into five to ten themes by the underlying problem, not by the words used: "takes forever to load" and "so slow on my phone" are one theme. Count distinct threads per theme: twenty agreeing replies in one thread are one thread with strong agreement, so note the top `score` as intensity.
5. **Deliver** a table: theme, the problem in one line in the buyer's words, threads (count), intensity (top score in the theme), two or three verbatim quotes each with its link, what people tried (workarounds and tools named), and the implication (a message, a feature, a content idea). Under the table, list the exact phrases people repeat, for copy and keywords, and how many threads the table rests on.

## Judgment

- Every finding carries its evidence: a verbatim quote with its link. Leave usernames out of the table, per the [evidence](../create-reddit-plan/references/platforms/reddit.md#evidence) rules in the Reddit notes, and paraphrase in public copy.
- Counts come from ranked samples. Write "in 15 threads read", never "most people on Reddit". Use counts to rank themes against each other, never to size a market; for how many people have the problem, use [check-demand](../check-demand/SKILL.md).
- Every search follows the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching): the sort and `time_range` to pass, the on-topic check, and what to do when a search fails.
- Separate the pain from the requested fix. People ask for features; the problem behind the ask is the finding ("I wish it had X" becomes the job X would do).
- A detailed post with numbers ("this costs us six hours a week") outweighs ten one-line agreements. Specific beats loud: a single thread with 400 upvotes is one conversation.
- A request for a recommendation ("is there a tool that...") is the strongest intent signal a post carries. A theme carried by such requests is worth flagging even when few threads hold it.
- Skip planted posts: a "problem" post whose top reply is a product link from an account that only posts about that product.
- Reddit skews toward technical, price-sensitive and hands-on users. If the ICP is enterprise buyers, say the themes may over-weight price and under-weight procurement and security. Where a post shows the author's role, count the authors who fit the ICP separately.
- Never post, reply or contact anyone. The deliverable is a table for the user. Listening to the same topics on a schedule belongs to [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
- Credits: if the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. A result this account already paid for is free while cached.

## Related skills

- Pains ranked across Reddit, Facebook groups and social comments, which uses this skill for Reddit: [find-pain-points](../find-pain-points/SKILL.md).
- Which subreddits to read: [find-subreddits](../find-subreddits/SKILL.md). Threads to reply in: [find-reddit-threads](../find-reddit-threads/SKILL.md). A Reddit plan: [create-reddit-plan](../create-reddit-plan/SKILL.md).
- Complaints about named competitors: [find-competitor-complaints](../find-competitor-complaints/SKILL.md). The doubts that stop a purchase: [find-objections](../find-objections/SKILL.md).
- Whether demand exists: [check-demand](../check-demand/SKILL.md). Who the buyers are: [build-personas](../build-personas/SKILL.md).
