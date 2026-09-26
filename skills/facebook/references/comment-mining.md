# Facebook comment mining

What people say under Facebook posts: questions, objections, complaints, praise and the words they use. On the user's own page it finds questions to answer and objections for the FAQ; on a competitor's page, what its customers complain about and ask for. Facebook has no comment search, so the job reads the comments of chosen posts. It ends in a table of themes with counts and quotes.

## Inputs to settle first

- **Posts**: post or reel URLs, or a page (name or URL) to pick posts from. Default: the 15 posts with the most comments among the page's last 50 or so.
- **Whose page**: the user's own, a competitor's, or a group's posts (from [group mining](group-mining.md)). It decides what to look for.
- **Depth**: one page of comments per post, and a second on posts with many comments.
- **Budget**: a default run costs about 5 + 15 x 2 = 35 credits (five pages of posts, two pages of comments on each of 15 posts). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the posts.** From a page: `facebook_get_posts` with `handle` or `url` (1 credit per page of results), paging with `meta.cursor` to about 50 posts, and rank them by `comments`. Take the top 15, leaving out giveaways and contests, whose comments are entries rather than opinions. From URLs, use them as given.
2. **Pull the comments.** `facebook_get_comments` with each post's `url` (1 credit per page of results). Take a second page with `meta.cursor` on posts with more than about 50 `comments`. Keep `text`, `likes`, `replies`, `depth` and `created_at`.
3. **Clean.** Drop comments that only tag a friend, emoji-only comments and spam. Keep the page's own replies apart: they show how the brand answers, not what customers think. Count the tag-only comments separately as a sign people share the post.
4. **Code and cluster.** Tag each comment: question, objection (price, trust, fit, effort), complaint, praise, request, or a competitor named. Cluster into themes, count comments per theme, and weight by `likes`: a like on a comment is agreement.
5. **Deliver** a table: theme, type, comments (count), total likes, two or three verbatim quotes with the post link, and the implication (an FAQ answer, an ad angle, a product fix, a content idea). On the user's own page, add the questions nobody from the page answered, with links, for the user to answer.

## Judgment

- One page of comments is not all of them. Report how many were read against each post's `comments` count.
- Comments on ads and boosted posts (`is_ad`) skew toward objections and trolling; keep them apart from organic posts.
- A question asked under many posts is a gap on the website or in the onboarding, not only a comment to answer.
- Commenters are private people. Quote without names, and never turn commenters into a lead list.
- The same job on TikTok, Instagram or YouTube sits in those platforms' groups, and pain points across all of them in the `customers` group's [pain points](../../customers/references/pain-points.md), which uses this playbook for Facebook.
