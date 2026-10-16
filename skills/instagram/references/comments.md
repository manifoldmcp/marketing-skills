# Mining Instagram comments

The questions, objections, desires and words people use under the posts and reels that matter, coded into themes with counts and verbatim quotes. Comments are where buyers say what they doubt and want in their own language. Cross-platform pain and objection research, with Reddit and reviews, belongs to [voice-of-customer](../../voice-of-customer/SKILL.md); this is the Instagram slice.

## Method

1. **Pick the posts.** From two or three category and problem tags plus a competitor's brand tag (posts about rivals collect the sharpest objections), `instagram_search_posts` with `since: "year"`, two pages each; from handles, two pages of `instagram_get_posts`; or the user's URLs. Keep the 15 posts with the most `comments`, from different authors. Skip giveaways and contests (their comments are entries) and note comment-for-DM posts (their comments are keyword triggers). Label `is_ad: true` rows: comments on an ad are objections to the ad.
2. **Pull.** Two pages of `instagram_get_comments` on each (15 comments a page, newest first). Search rows also carry up to 15 `top_comments`, free with the search: enough to choose which posts deserve a pull. Turn on `include_replies: true` (15 credits a page, charged even when empty) only where the replies matter, such as a brand answering objections. About 36 credits by default.
3. **Clean.** Drop spam, emoji-only comments and bare friend tags (count the tags: people sharing the post). Count, then drop, one-word keyword comments ("LINK", "RECIPE"): they measure demand for the offer, not opinion. Keep the post author's own replies apart.
4. **Code** each comment as one of: question, objection, desire (the outcome wanted), comparison (names a competitor or alternative), experience (used it, good or bad). Group into themes and weight each by the likes on its comments.
5. **Pull the language:** the exact phrases people repeat for the problem and the outcome, across posts. These are the words for hooks, captions, ads and landing pages.
6. **Deliver** a table: theme, type, comments, total likes, posts it appears on, two or three verbatim quotes with the post link, and what to do with it (a reel or carousel to make, an FAQ entry, an ad angle, a page line, an objection for sales, a product request). Below it, the recurring phrases and how many posts and comments it rests on.

## Judgment

- Quote word for word with the link: the commenter's language is the value.
- A theme on one post belongs to that post. Report themes that recur on three or more posts, or several accounts, first.
- Likes on a comment show agreement; one comment with 500 likes outweighs fifty with none. A first page sorted newest first can miss the most-liked comments on an old post: pull more pages on the posts that matter.
- Comments under a creator's post are partly about the creator; keep the ones about the product or the problem.
- A question asking for a recommendation is the strongest buying intent a comment carries. A question asked again and again is a post to make: an answer in a carousel earns the saves Instagram rewards.
- Instagram has no search across comments, and hashtag search sees only tagged posts: the table covers only the posts picked. Pick more posts rather than more pages when a theme is thin.
- Commenters are private people. Quote them as evidence for the user; in public copy, paraphrase and never name them. Never turn commenters into a lead list, and never reply, DM or contact anyone.
- Offer to write the objections and customer language into `.agents/product-marketing.md` with [manifold-get-started](../../manifold-get-started/SKILL.md).
