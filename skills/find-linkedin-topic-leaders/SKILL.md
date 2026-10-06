---
name: find-linkedin-topic-leaders
description: When the user wants to know who leads the conversation on a topic on LinkedIn. Finds the people who post most and best on the topic from the authors behind LinkedIn's top search results, traces each to a profile, sizes them by followers and by the engagement their posts get, and labels each a creator, practitioner, vendor or buyer. Also use when the user mentions top LinkedIn voices, LinkedIn influencers or thought leaders in a niche, who owns a topic on LinkedIn, or LinkedIn people to learn from, comment on or partner with. Posts to comment on today go to find-linkedin-posts-to-comment, people posting about the problem to find-linkedin-buyer-posts, post formats to find-linkedin-post-formats, and creators to sponsor on TikTok, Instagram or YouTube to find-creators.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find LinkedIn topic leaders

The people who post most and best on a topic on LinkedIn are the ones to learn from, comment on or partner with. This skill finds the authors behind the top search results, traces them to their profiles, and sizes them by followers and by the engagement their posts get. It ends in a ranked table with a profile link and a reason for each.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_search_posts` and `linkedin_get_profile` (hosts often add a prefix, for example `mcp__manifold__linkedin_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `linkedin_*` tools are not, the LinkedIn tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the problem the product solves, the ICP, the competitors, customer language) from it; ask only for what it lacks. Use its customer language as the search phrasings. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Topic**: three to five searches in the words the ICP uses ("revops", "sales compensation", "outbound SDR"), not the product category alone.
- **ICP**: the titles and company types that buy, to spot buyers among the leaders.
- **Window**: default `since: "month"` for who is active now; `since: "year"` for who has owned the topic longer.
- **Who counts**: everyone, or only practitioners (people who do the job) rather than creators, vendors and consultants. Default: everyone, labelled in step 5.
- **Count**: default the top 20.
- **Budget**: a default run costs about 4 x 3 + 25 + 5 = 42 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the topic.** `linkedin_search_posts` for each search, three pages (1 credit a page). Search is LinkedIn's own ranked search, read the way the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md#what-linkedin-shows) say. Keep every row with its `author_name`, `url`, `likes`, `comments` and `created_at`.
2. **Group by author.** Trace each author's handle from the post URL per the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md#what-linkedin-shows). Per author: posts in the results (how much), median likes and comments (how well), and the best post. Keep authors with at least two posts in the results, or one post in the top tenth by engagement. Set company pages aside unless the user wants them.
3. **Read the profiles.** `linkedin_get_profile` on the top 25 authors (1 credit each): `followers`, `location` and the `bio`. Compute engagement per 1,000 followers per the [thresholds](../create-linkedin-plan/references/platforms/linkedin.md#thresholds).
4. **See their range.** A person's feed cannot be listed. For the top five, `linkedin_search_posts` on the topic they post about most with `since: "year"` (1 credit each) and keep the rows under their name: whether they post on the topic every week or had one hit.
5. **Label each.** From the `bio` and the posts: creator (writes for an audience), practitioner (writes about their own work), vendor or consultant (writes to sell), or buyer (in the user's ICP). Mark competitors and their employees.
6. **Deliver** a table: name, profile URL, label, `bio` in a line, followers, posts in the results, median likes and comments, engagement per 1,000 followers, best post (URL, first line, likes, comments), and why they matter: learn from, comment on, partner with, or watch as a competitor. Write no comment or message unless the user asks; then follow the [handoff](../create-linkedin-plan/references/platforms/linkedin.md#handoff) in the LinkedIn notes.

## Judgment

- Every LinkedIn call follows the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md): what the tools show, the [thresholds](../create-linkedin-plan/references/platforms/linkedin.md#thresholds), the credits and the handoff.
- "Posts most" means most in the ranked results, which favour posts that already did well. A steady poster with modest numbers can be missing; a second search with narrower terms finds more. The table is a sample of who is talking, not everyone on the topic.
- Followers are reach; engagement per 1,000 followers is resonance. A leader with 8,000 followers and high resonance is often a better partner than a famous one who posts about everything.
- A post with far more comments than likes is often a "comment X to get the guide" giveaway. Discount it per the [thresholds](../create-linkedin-plan/references/platforms/linkedin.md#thresholds).
- Buyers who post about the topic are worth more to the user than creators with bigger audiences. Keep them in the table even when their numbers are small.
- The comments on a leader's posts, where much of the value is, cannot be read here. The user reads the threads of the top posts before engaging.
- Never post, comment, like, connect or send a message. The deliverable is a list with a link to every post and person; the user acts from their own account. Help first: a comment worth suggesting adds a number, an example or a counterpoint, never a pitch.
- `created_at` is approximate to the unit LinkedIn shows.

## Related skills

- Recent posts by these leaders to comment on: [find-linkedin-posts-to-comment](../find-linkedin-posts-to-comment/SKILL.md). People posting about the problem the user solves: [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md).
- A plan for the user's own LinkedIn: [create-linkedin-plan](../create-linkedin-plan/SKILL.md). The post formats and hooks that work in the niche: [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md).
- Creators to sponsor on TikTok, Instagram or YouTube: [find-creators](../find-creators/SKILL.md). Emails for the people found here: [enrich-lead-list](../enrich-lead-list/SKILL.md).
