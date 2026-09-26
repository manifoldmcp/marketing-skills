# Instagram comment mining

The questions, objections and words people use in the comments under the niche's top Instagram posts and reels. Comments are where buyers say what they doubt and what they want, in their own language. It ends in a table of themes with counts, quotes and what to do with each.

## Inputs to settle first

- **Source**: two or three hashtags for the category and the problem, plus a competitor's brand tag: posts about rivals collect the sharpest objections. Or competitor handles, to read the comments on their own posts.
- **Window**: `since: "year"` by default; `since: "month"` for a new product or a news-driven topic.
- **Sample**: 15 posts with two pages of comments each by default, without replies.
- **Budget**: a default run costs about 3 x 2 + 15 x 2 = 36 credits. Each page with `include_replies: true` costs 15 instead of 1. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the posts.** `instagram_search_posts` for each hashtag with `since: "year"`, two pages each (1 credit a page). For competitor handles, `instagram_get_posts`, two pages each (1 credit a page). Keep the 15 posts with the most `comments`, from different authors. Skip giveaways ("tag three friends to win"): their comments are entries, not opinions. Label any row with `is_ad: true`: comments on an ad are objections to the ad.
2. **Pull the comments.** `instagram_get_comments` on each post, two pages (1 credit a page). Keep `text`, `likes` and `replies`. Turn on `include_replies: true` (15 credits a page, charged even when no reply comes back) only for a post whose top comments show many `replies` and where the thread matters, such as a brand answering objections.
3. **Clean.** Drop spam, emoji-only comments and comments that only tag a friend (count those: they show people sharing the post). Count, then drop, one-word keyword comments ("LINK", "GUIDE") that trigger an automated DM: they show demand for the offer, not an opinion. Keep the post author's own comments apart: they answer, they do not ask.
4. **Code each comment.** One type per comment: question, objection, desire (the outcome they want), comparison (names a competitor or an alternative), or experience (they used it, good or bad). Group them into themes, and weight each theme by the likes on its comments: likes show how many people agree.
5. **Pull the language.** List the exact phrases people repeat for the problem and the outcome across posts. These are the words for hooks, captions, ads and landing pages.
6. **Deliver** a table: theme, type, comments, total likes, posts it appears on, two or three quotes word for word with the post link, and what to do with it (a reel or carousel to make, an FAQ entry, an ad angle, a landing page line). Below it, the list of recurring phrases, and how many posts and comments the table rests on.

## Judgment

- Comments under a creator's post are partly about the creator. Keep the ones about the product or the problem.
- A theme found on one post belongs to that post. Report themes that recur across three or more posts first.
- Instagram has no search across comments, and hashtag search sees only tagged posts, so the table covers only the posts picked. Say so, and pick more posts rather than more pages when a theme is thin.
- A comment with 500 likes outweighs fifty comments with none.
- Quote commenters as evidence for the user. In public copy, paraphrase and never name the commenter.
- For pain points across Reddit, reviews and several platforms, the [customers](../../customers/references/pain-points.md) group's pain points playbook calls this one and merges the results.
