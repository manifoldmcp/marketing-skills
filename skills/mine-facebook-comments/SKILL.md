---
name: mine-facebook-comments
description: When the user wants to know what people say in the comments under Facebook posts. Reads the comments under a Facebook page's most discussed posts and reels, the user's own page or a competitor's, and codes them into questions, objections, complaints, praise, requests and competitors named, with counts, likes, verbatim quotes and the questions nobody answered. Also use when the user mentions what people say in the comments on our Facebook posts, Facebook comment analysis, complaints under a competitor's Facebook page, or customer language from Facebook. Facebook groups go to mine-facebook-groups, comments on TikTok to mine-tiktok-comments, on Instagram to mine-instagram-comments, on YouTube to mine-youtube-comments, pains ranked across Reddit and every platform to find-pain-points, doubts before buying across sources to find-objections, how a Facebook page performs to audit-facebook-page.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Mine Facebook comments

What people say under Facebook posts: questions, objections, complaints, praise and the words they use. On the user's own page it finds questions to answer and objections for the FAQ; on a competitor's page, what its customers complain about and ask for. Facebook has no comment search, so the job reads the comments of chosen posts and ends in a table of themes with counts, verbatim quotes and what to do with each.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `facebook_get_posts` and `facebook_get_comments` (hosts often add a prefix, for example `mcp__manifold__facebook_get_comments`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `facebook_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the category and the problem in the buyer's words, the competitors and their pages, the user's own page) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first. After delivering, offer to write the objections and the customer language into `.agents/product-marketing.md` with [create-product-context](../create-product-context/SKILL.md).

## Inputs to settle first

- **Posts**: post or reel URLs, or a page (name or URL) to pick posts from. Default: the 15 posts with the most comments among the page's last 50 or so.
- **Whose page**: the user's own, a competitor's, or a group's posts (found with [mine-facebook-groups](../mine-facebook-groups/SKILL.md)). It decides what to look for.
- **Question**: what the user wants to learn: objections before buying, questions, feature requests, what confuses people, or the language for copy. Default: all of them.
- **Depth**: one page of comments per post, and a second on posts with many comments.
- **Budget**: a default run costs about 5 + 15 x 2 = 35 credits (five pages of posts, two pages of comments on each of 15 posts). Say the figure before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the posts.** From a page: `facebook_get_posts` with `handle` or `url` (1 credit per page of results), paging with `meta.cursor` to about 50 posts, and rank them by `comments`. Take the top 15, leaving out giveaways and contests, whose comments are entries rather than opinions. From URLs, use them as given.
2. **Pull the comments.** `facebook_get_comments` with each post's `url` (1 credit per page of results). Take a second page with `meta.cursor` on posts with more than about 50 `comments`. Keep `text`, `likes`, `replies`, `depth` and `created_at`.
3. **Clean.** Drop comments that only tag a friend, emoji-only comments and spam. Keep the page's own replies apart: they show how the brand answers, not what customers think. Count the tag-only comments separately as a sign people share the post.
4. **Code and cluster.** Tag each comment: question, objection (price, trust, fit, effort), complaint, praise, request, or a competitor named. Cluster into themes, count comments per theme, and weight by `likes`: a like on a comment is agreement.
5. **Deliver** a table: theme, type, comments (count), total likes, posts it appears on, two or three verbatim quotes with the post link, and the implication (an FAQ answer, an ad angle, a product fix, a content idea, an objection for sales). Below it, the phrases people repeat, and how many posts and comments the table rests on. On the user's own page, add the questions nobody from the page answered, with links, for the user to answer.

## Judgment

- Quote comments word for word, with the link. The value is the commenter's language, and a paraphrase loses it.
- One page of comments is not all of them. Report how many were read against each post's `comments` count. No search reaches across comments, so the table covers only the posts picked; pick more posts rather than more pages when a theme is thin.
- Comments on ads and boosted posts skew toward objections and trolling. Facebook rows do not mark them, so ask the user which posts were boosted and keep those apart from organic posts.
- Comments under a brand's own posts skew to fans and support questions. A theme found on one post belongs to that post; report themes that recur across three or more posts first.
- A question asked under many posts is a gap on the website or in the onboarding, not only a comment to answer.
- A comment with 500 likes outweighs fifty comments with none: likes show how many people agree.
- Commenters are private people. Quote them as evidence for the user; in public copy, paraphrase and never name the commenter. Never turn commenters into a lead list.
- Reactions on the post measure attention, not intent to buy. A question asking for a recommendation is the strongest intent a comment carries.
- Never reply, post or contact anyone. The deliverable is a table for the user. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back; `dry_run: true` prices any call for free.

## Related skills

- Problems ranked across Reddit and every platform, which uses this skill for Facebook posts: [find-pain-points](../find-pain-points/SKILL.md).
- What members of Facebook groups ask and recommend: [mine-facebook-groups](../mine-facebook-groups/SKILL.md).
- The same job on other platforms: [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md), [mine-instagram-comments](../mine-instagram-comments/SKILL.md) and [mine-youtube-comments](../mine-youtube-comments/SKILL.md).
- The doubts that stop a purchase, from Google, Reddit and reviews: [find-objections](../find-objections/SKILL.md).
- How the page itself performs: [audit-facebook-page](../audit-facebook-page/SKILL.md). Content ideas from the questions found here: [find-content-ideas](../find-content-ideas/SKILL.md).
