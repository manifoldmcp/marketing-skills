---
name: find-linkedin-posts-to-comment
description: When the user wants recent LinkedIn posts to comment on, by topic leaders or by their buyers, where a comment from them adds value. Finds fresh posts on the topics the user can speak to, traces each author to a profile with followers, checks there is still room in the thread, and gives the angle the user could add in one line. Also use when the user mentions posts to comment on today, a LinkedIn commenting routine, social selling, or where to engage on LinkedIn this week. People posting about the problem the user solves go to find-linkedin-buyer-posts, the leaders of a topic to find-linkedin-topic-leaders, post formats to find-linkedin-post-formats, and a plan for the user's own LinkedIn to create-linkedin-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find LinkedIn posts to comment on

Show up where buyers and leaders already post on LinkedIn, by helping rather than advertising. This skill builds a short daily or weekly list of recent posts, by topic leaders or by the user's buyers, where a comment from the user adds something: a number, an example, a counterpoint. The tools find and rank the posts; the user reads the thread and writes the comment.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_search_posts` and `linkedin_get_profile` (hosts often add a prefix, for example `mcp__manifold__linkedin_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `linkedin_*` tools are not, the LinkedIn tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the problem the product solves, the ICP, the competitors, customer language) from it; ask only for what it lacks. Use its customer language as the search phrasings. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. After delivering, offer to write new customer language into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Inputs to settle first

- **Whose posts**: leaders on the topic (from [find-linkedin-topic-leaders](../find-linkedin-topic-leaders/SKILL.md), or found in step 1), buyers in the ICP, or both. Default: both.
- **Topics**: three to five searches the user can speak to with authority, in the buyer's words, not the product category alone.
- **ICP**: the titles and company types that buy, to sort authors into buyers, leaders and vendors.
- **The user's edge**: what they know that others do not (data from their product, a result, years in the role). Every angle in the table comes from it.
- **Window**: `since: "day"` for a daily list, `since: "week"` for a weekly one. Default: day.
- **Count**: default 10 posts a day. More than that becomes spam.
- **Budget**: a default run costs about 4 x 2 + 20 + 5 = 33 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find recent posts.** `linkedin_search_posts` for each topic with the window, two pages (1 credit a page). Search is LinkedIn's own ranked search, read the way the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md#what-linkedin-shows) say. For named leaders, there is no feed to list: search the topics they post about and keep the rows under their name.
2. **Check the authors.** Trace each author's handle from the post URL per the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md#what-linkedin-shows) and call `linkedin_get_profile` on the 20 most promising (1 credit each). From the `bio`, `followers` and `location`, sort each author: a buyer in the ICP, a leader with reach, or a vendor, consultant, competitor or recruiter (drop those).
3. **Check there is room.** From the row's `likes` and `comments` (or `linkedin_get_post`, 1 credit, when null): on a post with fewer than about 50 comments, the author and the readers still see a new one; past a few hundred, it sinks. Drop giveaways ("comment X to get the guide") and posts older than the window.
4. **Pick where a comment adds value.** Keep posts that ask a question, make a claim the user can back or test with data, or describe a problem the user knows well. Write the angle in one line: what the user can add. Never a pitch, a link to the product, or "great post".
5. **Deliver** a table: post URL, author, who they are (buyer, leader), followers, posted (approximate), likes, comments, why this post, and the angle. Sort buyers first, then by how recent the post is. Write no comment unless the user asks; then follow the [handoff](../create-linkedin-plan/references/platforms/linkedin.md#handoff) in the LinkedIn notes.

## Judgment

- Every LinkedIn call follows the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md): what the tools show, the [thresholds](../create-linkedin-plan/references/platforms/linkedin.md#thresholds), the credits and the handoff.
- Help first. A comment worth suggesting adds something on its own: a number, an example, a counterpoint. Never a pitch, a link to the product, or "great post".
- The comment is the user's own. Draft one only when asked, mark it a draft, and keep it to what the user actually knows.
- Never post, comment, like, connect or send a message. The deliverable is a list with a link to every post and person; the user acts from their own account.
- The comments on a post cannot be read here, so competitors' replies and what others already said are invisible. The user reads the thread before writing, so the comment does not repeat someone.
- A comment on a buyer's post is worth more than one on a famous creator's, even with ten times fewer readers: the buyer reads every comment.
- Consistency matters more than volume. Ten useful comments a day for a month builds a name; fifty in one day looks automated.
- Search is ranked and incomplete. The list is a sample of who is talking, not everyone on the topic.
- `created_at` is approximate to the unit LinkedIn shows, which is enough for "today" and "this week".
- A list on a schedule, every morning or every week, is the host's job, the way [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md) runs: the host runs this skill on its schedule and keeps the post URLs already listed. The server keeps no state.

## Related skills

- People posting about the problem the user solves: [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md). The leaders of a topic, whose posts to follow: [find-linkedin-topic-leaders](../find-linkedin-topic-leaders/SKILL.md).
- A plan for the user's own LinkedIn: [create-linkedin-plan](../create-linkedin-plan/SKILL.md). Post formats that work on LinkedIn: [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md).
- Email addresses and company records for the people found: [enrich-lead-list](../enrich-lead-list/SKILL.md).
- The same engagement on Reddit: [find-reddit-threads](../find-reddit-threads/SKILL.md). Creators to sponsor on TikTok, Instagram or YouTube: [find-creators](../find-creators/SKILL.md).
