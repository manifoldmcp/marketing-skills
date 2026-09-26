# Instagram competitor accounts

What rival brands do on Instagram and what works for them: how often they post, how much of it is reels, which posts beat their own median, how engaged their following is, and which posts are paid. It ends in a side-by-side table and a line per competitor on what the user should take from it.

## Inputs to settle first

- **Competitors**: two to five Instagram handles. If the user names brands but not handles, ask, or read the `author` of the posts under the brand's own hashtag from `instagram_search_posts` (1 credit). If they do not know their competitors, the [competitors](../../competitors/SKILL.md) group finds them first.
- **User's account**: the user's handle, to put their own numbers in the same table. Optional.
- **Window**: the last 90 days by default.
- **Budget**: a default run costs about 4 x (1 + 3 + 2 + 3) = 36 credits for three competitors and the user. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Size.** `instagram_get_profile` on each handle (1 credit): followers, `posts_count`, bio, website, and `kind` (company for a business account). A handle that returns `NoData` is wrong; fix it before paying for the rest.
2. **Recent output.** `instagram_get_posts` on each account, paging until the window is covered (1 credit a page, about three pages for 90 days at a normal cadence). Order the rows by `created_at` yourself: pinned posts can sit first whatever their age. For each account: posts in the window, posts a week, the share of reels (`media: "video"`), the median image-post engagement rate over followers, and the days it posts. Read the numbers as the [router](../SKILL.md#reading-the-numbers) says.
3. **Reels.** `instagram_get_reels`, two pages (1 credit a page): reels a week, median views, median reel engagement rate, median `duration_s`. Mark the outliers (3 times the account's median views or more). The listings run newest first, so rank the account's top posts yourself: reels by views, image posts by likes.
4. **What the top reels are.** `instagram_get_transcript` on the three strongest recent outlier reels per account (1 credit each), with the first line of each caption: the topic, the format, the hook and the call to action. Note any series or format the account repeats.
5. **Paid posts.** Count the rows with `is_ad: true`: ads and paid partnerships in the account's own feed, what they promote, and which creators appear in them. For the ads the competitor runs in Meta's ad library, which covers Instagram placements, open the paid-ads group's [competitor ads](../../paid-ads/references/competitor-ads.md); do not pull them here.
6. **Deliver** a table, one row per account: handle and link, followers, posts in the window, posts a week, reels a week, median reel views, median reel engagement rate, image-post engagement rate, paid posts, the top three recent posts (link, views or likes, multiple of the median, what each is in a few words). Below it, one line per competitor: what the user should take and what to avoid.

## Judgment

- Compare accounts on median reel views and engagement rates, not followers. Followers pile up over years; reel views show what Instagram gives the account now.
- The count of outlier reels in 90 days says more than the single biggest post. Six outliers mean a repeatable format; one means luck.
- Formats a brand keeps posting are usually the ones that work for it. A format it tried once and dropped probably did not.
- Creators in a competitor's paid partnerships have already sold something like the user's product. They are a head start for [find creators](find-creators.md).
- A competitor whose cadence fell in the last month may have cut its Instagram effort. Say so; it is an opening.
- The tools see what is public. Saves, shares, reach, stories and follower growth over time are not there. For growth over time, the host re-runs this and keeps the table: the [monitoring](../../monitoring/references/competitor-watch.md) group's competitor watch.
- For everything about a competitor beyond Instagram, use the [competitors](../../competitors/references/teardown.md) group's teardown.
