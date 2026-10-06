---
name: mine-instagram-comments
description: When the user wants to know what people say in the comments under Instagram posts and reels. Reads the comments under the niche's top Instagram posts and reels, from hashtags, a competitor's account or the user's own, and codes them into questions, objections, desires, comparisons and experiences, with counts, likes, verbatim quotes and the words people repeat. Also use when the user mentions mining Instagram comments, objections under a competitor's Instagram posts or Reels, what people ask in IG comments, or customer language from Instagram. Comments on TikTok go to mine-tiktok-comments, on YouTube to mine-youtube-comments, on Facebook to mine-facebook-comments, pains ranked across Reddit and every platform to find-pain-points, doubts before buying across sources to find-objections, how an Instagram account performs to audit-instagram-account, hooks to find-instagram-hooks.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Mine Instagram comments

The questions, objections, desires and words people use in the comments under the Instagram posts and reels that matter. Comments are where buyers say what they doubt and what they want, in their own language. It ends in a table of themes with counts, verbatim quotes and what to do with each, from a few hundred comments rather than a guess.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `instagram_search_posts` and `instagram_get_comments` (hosts often add a prefix, for example `mcp__manifold__instagram_get_comments`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `instagram_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the category and the problem in the buyer's words, the competitors and their accounts, the user's own accounts) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. After delivering, offer to write the objections and the customer language into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Inputs to settle first

- **Source**: two or three hashtags for the category and the problem, plus a competitor's brand tag: posts about rivals collect the sharpest objections. Or competitor handles, to read the comments on their own posts, or the user's own handle, or post URLs.
- **Question**: what the user wants to learn: objections before buying, questions, feature requests, what confuses people, or the language for copy. Default: all of them.
- **Window**: `since: "year"` by default; `since: "month"` for a new product or a news-driven topic.
- **Sample**: 15 posts with two pages of comments each by default, without replies.
- **Budget**: a default run costs about 3 x 2 + 15 x 2 = 36 credits. Each page with `include_replies: true` costs 15 instead of 1. Say the figure before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the posts.** `instagram_search_posts` for each hashtag with `since: "year"`, two pages each (1 credit a page). For handles, `instagram_get_posts`, two pages each (1 credit a page). From URLs, use them as given. Keep the 15 posts with the most `comments`, from different authors. Skip giveaways ("tag three friends to win") and contests: their comments are entries, not opinions. Label any row with `is_ad: true`: comments on an ad are objections to the ad.
2. **Pull the comments.** `instagram_get_comments` on each post, two pages (1 credit a page). Keep `text`, `likes` and `replies`. Turn on `include_replies: true` (15 credits a page, charged even when no reply comes back) only for a post whose top comments show many `replies` and where the thread matters, such as a brand answering objections.
3. **Clean.** Drop spam, emoji-only comments and comments that only tag a friend (count those: they show people sharing the post). Count, then drop, one-word keyword comments ("LINK", "GUIDE") that trigger an automated DM: they show demand for the offer, not an opinion. Keep the post author's own comments apart: they answer, they do not ask.
4. **Code each comment.** One type per comment: question, objection, desire (the outcome they want), comparison (names a competitor or an alternative), or experience (they used it, good or bad). Group them into themes, and weight each theme by the likes on its comments: likes show how many people agree.
5. **Pull the language.** List the exact phrases people repeat for the problem and the outcome across posts. These are the words for hooks, captions, ads and landing pages.
6. **Deliver** a table: theme, type, comments, total likes, posts it appears on, two or three quotes word for word with the post link, and what to do with it (a reel or carousel to make, an FAQ entry, an ad angle, a landing page line, an objection for sales, a request for the product team). Below it, the list of recurring phrases, and how many posts and comments the table rests on.

## Judgment

- Quote comments word for word, with the link. The value is the commenter's language, and a paraphrase loses it.
- Comments under a creator's post are partly about the creator. Keep the ones about the product or the problem.
- A theme found on one post belongs to that post. Report themes that recur across three or more posts, or several accounts, first.
- A comment with 500 likes outweighs fifty comments with none: likes show how many people agree.
- Instagram has no search across comments, and hashtag search sees only tagged posts, so the table covers only the posts picked. Say so, and pick more posts rather than more pages when a theme is thin.
- Commenters are private people. Quote them as evidence for the user; in public copy, paraphrase and never name the commenter. Never turn commenters into a lead list.
- Views and likes on the post measure attention, not intent to buy. A question asking for a recommendation is the strongest intent a comment carries.
- Never reply, post, DM or contact anyone. The deliverable is a table for the user. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back; `dry_run: true` prices any call for free.

## Related skills

- Problems ranked across Reddit and every platform, which uses this skill for Instagram: [find-pain-points](../find-pain-points/SKILL.md).
- The same job on other platforms: [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md), [mine-youtube-comments](../mine-youtube-comments/SKILL.md) and [mine-facebook-comments](../mine-facebook-comments/SKILL.md).
- The doubts that stop a purchase, from Google, Reddit and reviews: [find-objections](../find-objections/SKILL.md).
- How the account itself performs: [audit-instagram-account](../audit-instagram-account/SKILL.md). Content ideas from the questions found here: [find-content-ideas](../find-content-ideas/SKILL.md).
- Hooks for the phrases people repeat: [find-instagram-hooks](../find-instagram-hooks/SKILL.md).
