---
name: mine-youtube-comments
description: When the user wants to know what viewers say in the comments under YouTube videos. Reads the comments under the videos that matter, from a topic (reviews, tutorials, comparisons), a competitor's channel or the user's own, and codes them into questions, objections, feature requests, comparisons and use cases, with counts, likes, verbatim quotes and the words people repeat. Also use when the user mentions mining YouTube comments, questions under YouTube tutorials, objections in the comments of YouTube reviews, or customer language from YouTube. Comments on TikTok go to mine-tiktok-comments, on Instagram to mine-instagram-comments, on Facebook to mine-facebook-comments, pains ranked across Reddit and every platform to find-pain-points, doubts before buying across sources to find-objections, how a YouTube channel performs to audit-youtube-channel, video ideas to find-youtube-video-ideas.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Mine YouTube comments

What viewers say under the YouTube videos that matter: their questions, objections, complaints, the products they compare, and the words they use. Comments are where buyers say what they doubt and what they want, in their own language. It ends in a table of themes with counts, verbatim quotes and what to do with each, from a few hundred comments rather than a guess.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `youtube_search_videos`, `youtube_get_video` and `youtube_get_comments` (hosts often add a prefix, for example `mcp__manifold__youtube_get_comments`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `youtube_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the category and the problem in the buyer's words, the competitors and their channels, the user's own channel) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. After delivering, offer to write the objections and the customer language into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Inputs to settle first

- **Source**: which videos. The user's own channel, a competitor's channel, a topic (reviews and tutorials of the category or of a competitor), or video URLs. Reviews of rivals collect the sharpest objections.
- **Question**: what the user wants to learn: objections before buying, feature requests, what confuses people, or the language for copy. Default: all of them, coded in step 3.
- **Window**: `since: "year"` by default for a topic search.
- **Size**: default 10 videos and two pages of comments each.
- **Budget**: a default run costs about 4 + 15 + 10 x 2 + 2 = 41 credits. Say the figure before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the videos.** For a topic, `youtube_search_videos` with `since: "year"` (1 credit each) for four queries such as "<competitor> review", "<category> tutorial", "<competitor> vs <competitor>" and "is <competitor> worth it". For a channel, `youtube_get_videos` with `sort: "popular"` (1 credit). Then `youtube_get_video` on the 15 most viewed candidates (1 credit each), since only it carries the `comments` count, and keep the 10 with the most comments, from different channels where the source is a topic. Skip giveaways and contests: their comments are entries, not opinions. Reviews and comparisons hold objections; tutorials hold confusion and feature requests.
2. **Pull the comments.** `youtube_get_comments` on each video, two pages (1 credit a page). They come most liked first, so the first page is what the audience agrees with most. Keep `text`, `likes`, `replies` and `depth`.
3. **Clean and code.** Drop spam, timestamps, jokes, emoji-only comments and "great video". Keep the channel's own replies apart: they answer, they do not ask. Put each remaining comment in one code: question, objection or complaint, feature request, comparison (names another product), praise, or use case ("I use it to..."). Group codes into themes and count each theme, weighting by `likes`: a comment with 300 likes speaks for many viewers.
4. **Check a disputed claim.** When comments argue with something the video says, `youtube_get_transcript` on that video (1 credit) shows exactly what was said.
5. **Deliver** a table: theme, code, comments, total likes, videos it appears on, two or three verbatim quotes with the video URL, and what to do with it: a video to make, a question for the FAQ or the sales page, a phrase for copy, an objection for sales to answer, a feature request for the product team. Below it, the phrases people repeat, and how many videos and comments the table rests on.

## Judgment

- Quote comments word for word, with the link. The value is the commenter's language, and a paraphrase loses it.
- A creator's fans defend the creator, and a review's comments argue with the reviewer. Separate complaints about the product from reactions to the video.
- Replies (`depth` above 0) often hold the answer to a question, and sometimes a competitor's support team. Read them before calling a question unanswered.
- `created_at` is approximate and the order is by likes, not time. For what people said last month, pick recent videos rather than sorting comments.
- A theme found on one video belongs to that video. Viewers are not all buyers: a theme that recurs across several channels' videos is stronger than one busy thread, so report those first.
- A comment with 500 likes outweighs fifty comments with none: likes show how many people agree.
- YouTube has no search across comments, so the table covers only the videos picked. Say so, and pick more videos rather than more pages when a theme is thin.
- Commenters are private people. Quote them as evidence for the user; in public copy, paraphrase and never name the commenter. Never turn commenters into a lead list.
- Views and likes on the video measure attention, not intent to buy. A question asking for a recommendation is the strongest intent a comment carries.
- Never reply, post or contact anyone. The deliverable is a table for the user. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back; `dry_run: true` prices any call for free.

## Related skills

- Problems ranked across Reddit and every platform, which uses this skill for YouTube: [find-pain-points](../find-pain-points/SKILL.md).
- The same job on other platforms: [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md), [mine-instagram-comments](../mine-instagram-comments/SKILL.md) and [mine-facebook-comments](../mine-facebook-comments/SKILL.md).
- The doubts that stop a purchase, from Google, Reddit and reviews: [find-objections](../find-objections/SKILL.md).
- How the channel itself performs: [audit-youtube-channel](../audit-youtube-channel/SKILL.md). Video ideas from the questions found here: [find-youtube-video-ideas](../find-youtube-video-ideas/SKILL.md).
