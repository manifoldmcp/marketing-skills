# Mining Facebook comments

What people say under a page's posts: questions, objections, complaints, praise, requests, competitors named, and the words they use. On the user's own page it finds questions to answer and objections for the FAQ; on a competitor's page, what its customers complain about and ask for. Facebook has no comment search, so the job reads the comments of chosen posts. Field traps are in [data](data.md).

## Picking the posts

- From a page: about 50 posts from `facebook_get_posts` (about 17 calls at 3 posts a call), ranked by `comments`; take the top 15. The `top_comments[]` on each row are a free preview of what a thread is about.
- Leave out giveaways and contests: their comments are entries and friend tags, not opinions.
- Leave out boosted posts or keep them apart. Comments on ads and boosted posts skew toward objections and trolling, and Facebook rows do not mark them; match against the page's ads ([page audit](page-audit.md#paid-apart)) and ask the user which posts were boosted.
- From URLs the user gives, or from group threads found in [groups](groups.md), use them as given.

## Reading

- `facebook_get_comments` returns 10 top-level comments a call. Replies are counted in `replies` but not returned, so a long back-and-forth under one comment is invisible. Read a second and third call on posts with more than about 20 comments; report how many were read against each post's `comments` count.
- Drop tag-only, emoji-only and spam comments, but count the tag-only ones: a friend tagged is a sign people share the post.
- Keep the page's own replies apart: they show how the brand answers, not what customers think.
- Tag each comment: question, objection (price, trust, fit, effort), complaint, praise, request, or competitor named. Cluster into themes by the underlying problem, count comments per theme and the posts each theme appears on, and weight by `likes` (a like on a comment is agreement).

## What a pattern means

- **Themes on three or more posts first.** A theme on one post belongs to that post.
- **A question asked under many posts** is a gap on the website or in onboarding, not only a comment to answer.
- **A comment asking for a recommendation** is the strongest buying signal a comment carries; reactions on a post measure attention, not intent.
- **Comments under a brand's own posts skew** to fans and support questions; under a competitor's they show what its customers want fixed.
- **A comment with 500 likes outweighs fifty with none.**
- One page of comments is not all of them, and the table covers only the posts picked. When a theme is thin, pick more posts rather than more calls on the same post.

## Deliver

A table: theme, type, comments (count), total likes, posts it appears on, two or three verbatim quotes with the post link, and the implication (an FAQ answer, an ad angle for [paid-ads](../../paid-ads/SKILL.md), a product fix, a post idea, an objection for sales). Below it the phrases people repeat, and how many posts and comments the table rests on. On the user's own page, the questions nobody from the page answered, with links, for the user to answer themselves.

Quote word for word; the commenter's language is the value. Leave commenters' names out. Combining these themes with Reddit, reviews and other platforms into ranked pains, objections or personas is [voice-of-customer](../../voice-of-customer/SKILL.md); offer to write the customer language into `.agents/product-marketing.md` with [manifold-get-started](../../manifold-get-started/SKILL.md).
