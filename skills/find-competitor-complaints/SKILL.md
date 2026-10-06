---
name: find-competitor-complaints
description: When the user wants to know what people dislike about named competitors and why they leave. Reads Reddit posts and comments for complaints, switch stories and alternative-to threads, clusters them by competitor and theme with the push, pull, habit and anxiety behind each switch and the products people move to, and lists fresh threads the user could answer. Also use when the user mentions what people hate about a competitor, alternative to X threads, people switching from X, why customers leave X, churn reasons of a rival, or competitor complaints on Reddit. Problems in the space with no competitor named go to find-pain-points, the user's competitive set to find-competitors, turning complaints into positioning to find-positioning, a sales battlecard to write-battlecard, threads to answer with no competitor named to find-reddit-threads.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Competitor complaints

What people dislike about named competitors, what makes them leave, and where they go instead, from Reddit posts and comments. It ends in a table by competitor and theme with quotes and links, and a short list of fresh "alternative to X" threads the user could answer.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_posts` and `reddit_get_comments` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `reddit_*` tools are not, the Reddit tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: every step reads Reddit.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the competitors, what the user's product does better and worse, the differentiation) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. After delivering, offer to write the switching dynamics and the customer language into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Inputs to settle first

- **Competitors**: two to four names, with the short names people use. A brand name that is also a common word ("Close", "Notion", "Monday") needs the category word in every query.
- **User's product**: what it does better and worse, so each complaint is marked as one the user answers or not.
- **Window**: default the past year. Older complaints may be fixed.
- **Budget**: a default run for three competitors costs about 3 x (4 + 2 + 8) + 3 + 5 + 3 = 53 credits (per competitor four post searches, two comment searches and eight threads read, then a 7-day check-in on three communities, five open threads checked and the rules of those communities). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the switch phrasings.** Per competitor, `reddit_search_posts` with four of "alternative to <X>", "<X> alternatives", "switched from <X>", "leaving <X>", "<X> vs", "<X> pricing" and `time_range: "year"` (1 credit each). Apply the on-topic check in the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching).
2. **Search the comments.** Per competitor, `reddit_search_comments` with two of "moved off <X>", "cancelled <X>", "<X> support", "<X> price increase" and `time_range: "all"` (1 credit each), keeping the rows whose `created_at` falls in the window. Comments hold the candid lines that posts dress up.
3. **Read the threads.** For the eight strongest threads per competitor (the most `comments`, a clear complaint or switch), `reddit_get_comments` (1 credit each). Note the four forces of the switch: the push (what failed, and the trigger: a price rise, a missing feature, support, reliability, a contract), the pull (where they went and why), the habit (what kept them: data, a contract, a trained team) and the anxiety (what they feared about moving). Note whether anyone defends the competitor.
4. **Cluster by competitor and theme.** Use themes such as price and packaging, a missing feature, reliability, support, complexity, contract or lock-in, and data or privacy. Count distinct threads per theme, and count the destinations people name ("moved to Y").
5. **Mark the open threads.** Search rows are ranked and miss most fresh posts, so check the three communities where the complaints clustered: `reddit_get_new_posts` with `since: "7d"` and `match` set to the competitor names (1 credit per subreddit per page), complete for the window. Keep the posts asking for an alternative or describing a switch, up to five. `reddit_get_post` on each (1 credit) and drop any that is `locked` or `archived`; then the rules verdict for their communities from the rules check in [find-subreddits](../find-subreddits/SKILL.md). Judge them further with [find-reddit-threads](../find-reddit-threads/SKILL.md) if the user wants to answer.
6. **Deliver** two tables. Complaints: competitor, theme, threads, intensity (top `score`), two quotes with links, destinations named, what holds people back from leaving (habit and anxiety), whether the user's product answers it (yes, partly, no), and the message angle. Open threads: title, URL, subreddit, age, comments, competitor, rules verdict.

## Judgment

- Every Reddit call follows the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md): [search and watching](../create-reddit-plan/references/platforms/reddit.md#search-and-watching), the [evidence](../create-reddit-plan/references/platforms/reddit.md#evidence) (quote verbatim with the link, no usernames), the [credits](../create-reddit-plan/references/platforms/reddit.md#credits) and the [handoff](../create-reddit-plan/references/platforms/reddit.md#handoff). Counts are counts in a ranked sample: compare themes with each other, never with a total.
- Unhappy customers post; happy ones rarely do. Say what the competitor's unhappy users say, never that the competitor is failing.
- Date every complaint. A pricing complaint from before the competitor's last price change may be stale.
- A complaint the user's product shares is not an angle. Mark it "no" and say so; the user needs to know before a prospect raises it.
- The destinations people name are the real competitive set. A surprise there goes to [find-competitors](../find-competitors/SKILL.md).
- In a reply, answer the question and state the difference; never attack the competitor. Threads close ranks against a vendor who does. Never post or reply for the user.

## Related skills

- Turning the themes into positioning or pricing: [find-positioning](../find-positioning/SKILL.md) and [compare-messaging](../compare-messaging/SKILL.md). A one-page sales sheet against one rival: [write-battlecard](../write-battlecard/SKILL.md).
- The full competitive set, classified: [find-competitors](../find-competitors/SKILL.md).
- Problems in the space with no competitor named: [find-pain-points](../find-pain-points/SKILL.md). Doubts before buying: [find-objections](../find-objections/SKILL.md).
- Answering the open threads: [find-reddit-threads](../find-reddit-threads/SKILL.md).
