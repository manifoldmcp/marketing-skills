---
name: mine-tiktok-comments
description: When the user wants to know what people say in the comments under TikTok videos. Reads the comments under the niche's top TikTok videos, from search terms, a competitor's account or the user's own, and codes them into questions, objections, desires, comparisons and experiences, with counts, likes, verbatim quotes and the words people repeat. Also use when the user mentions mining TikTok comments, what people ask in TikTok comments, objections under a competitor's TikToks, or customer language from TikTok. Comments on Instagram go to mine-instagram-comments, on YouTube to mine-youtube-comments, on Facebook to mine-facebook-comments, pains ranked across Reddit and every platform to find-pain-points, doubts before buying across sources to find-objections, how a TikTok account performs to audit-tiktok-account, hooks to find-tiktok-hooks.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Mine TikTok comments

The questions, objections, desires and words people use in the comments under the TikTok videos that matter. Comments are where buyers say what they doubt and what they want, in their own language. It ends in a table of themes with counts, verbatim quotes and what to do with each, from a few hundred comments rather than a guess.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos` and `tiktok_get_comments` (hosts often add a prefix, for example `mcp__manifold__tiktok_get_comments`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `tiktok_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the category and the problem in the buyer's words, the competitors and their accounts, the user's own accounts) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. After delivering, offer to write the objections and the customer language into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Inputs to settle first

- **Source**: two or three search terms for the category and the problem, plus competitor brand names: reviews of rivals collect the sharpest objections. Or competitor handles, to read the comments on their own videos, or the user's own handle, or video URLs.
- **Question**: what the user wants to learn: objections before buying, questions, feature requests, what confuses people, or the language for copy. Default: all of them.
- **Window**: `since: "year"` by default, which on TikTok reaches back about six months; `since: "month"` for a new product or a news-driven topic.
- **Sample**: 15 videos with two pages of comments each by default.
- **Budget**: a default run costs about 3 x 2 + 15 x 2 = 36 credits. Say the figure before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the videos.** `tiktok_search_videos` for each term with `since: "year"` and `sort: "popular"`, two pages each (1 credit a page). For handles, `tiktok_get_videos` with `sort: "popular"`, one page each (1 credit). From URLs, use them as given. Keep the 15 videos with the most `comments`, from different authors, mixing creator reviews and brand videos. Skip giveaways and contests: their comments are entries, not opinions. Label any row with `is_ad: true`: comments on an ad are objections to the ad.
2. **Pull the comments.** `tiktok_get_comments` on each video, two pages (1 credit a page). Keep `text`, `likes` and `replies`. Replies themselves do not come back; a comment with many `replies` is a debate worth noting.
3. **Clean.** Drop spam, emoji-only comments and comments that only tag a friend (count those: they show people sharing the video). Keep the creator's own comments apart (the `author` is the video's author): they answer, they do not ask.
4. **Code each comment.** One type per comment: question, objection, desire (the outcome they want), comparison (names a competitor or an alternative), or experience (they used it, good or bad). Group them into themes, and weight each theme by the likes on its comments: likes show how many people agree.
5. **Pull the language.** List the exact phrases people repeat for the problem and the outcome across videos. These are the words for hooks, ads and landing pages.
6. **Deliver** a table: theme, type, comments, total likes, videos it appears on, two or three quotes word for word with the video link, and what to do with it (a video to make, an FAQ entry, an ad angle, a landing page line, an objection for sales, a request for the product team). Below it, the list of recurring phrases, and how many videos and comments the table rests on.

## Judgment

- Quote comments word for word, with the link. The value is the commenter's language, and a paraphrase loses it.
- Comments under a creator's video are partly about the creator. Keep the ones about the product or the problem.
- A theme found on one video belongs to that video. Report themes that recur across three or more videos, or several creators, first.
- A comment with 500 likes outweighs fifty comments with none: likes show how many people agree.
- TikTok has no search across comments, so the table covers only the videos picked. Say so, and pick more videos rather than more pages when a theme is thin.
- Commenters are private people. Quote them as evidence for the user; in public copy, paraphrase and never name the commenter. Never turn commenters into a lead list.
- Views and likes on the video measure attention, not intent to buy. A question asking for a recommendation is the strongest intent a comment carries.
- Never reply, post or contact anyone. The deliverable is a table for the user. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back; `dry_run: true` prices any call for free.

## Related skills

- Problems ranked across Reddit and every platform, which uses this skill for TikTok: [find-pain-points](../find-pain-points/SKILL.md).
- The same job on other platforms: [mine-instagram-comments](../mine-instagram-comments/SKILL.md), [mine-youtube-comments](../mine-youtube-comments/SKILL.md) and [mine-facebook-comments](../mine-facebook-comments/SKILL.md).
- The doubts that stop a purchase, from Google, Reddit and reviews: [find-objections](../find-objections/SKILL.md).
- How the account itself performs: [audit-tiktok-account](../audit-tiktok-account/SKILL.md). Content ideas from the questions found here: [find-content-ideas](../find-content-ideas/SKILL.md).
- Hooks for the phrases people repeat: [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md).
