# TikTok competitor accounts

What rival brands do on TikTok and what works for them: how often they post, in which formats, which videos beat their own median, how engaged their audience is, and which posts are paid. It ends in a side-by-side table and a line per competitor on what the user should take from it.

## Inputs to settle first

- **Competitors**: two to five TikTok handles. If the user names brands but not handles, find the handle in the author field of a `tiktok_search_videos` on the brand name (1 credit). If they do not know their competitors, the [competitors](../../competitors/SKILL.md) group finds them first.
- **User's account**: the user's handle, to put their own numbers in the same table. Optional.
- **Window**: the last 90 days by default.
- **Budget**: a default run costs about 4 x (1 + 3 + 1 + 3) = 32 credits for three competitors and the user. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Size.** `tiktok_get_profile` on each handle (1 credit): followers, `posts_count`, likes, bio, website. A handle that returns `NoData` is wrong; fix it before paying for the rest.
2. **Recent output.** `tiktok_get_videos` with `sort: "latest"` on each account, paging until the window is covered (1 credit a page, usually three pages for 90 days). Order the rows by `created_at` yourself: a pinned video can sit first whatever its age. For each account: videos in the window, videos a week, median views, median engagement rate, median `duration_s`, the share of photo posts (`media: "image"`), and the days and hours it posts. Read the numbers as the [router](../SKILL.md#reading-the-numbers) says.
3. **Top videos.** Mark the outliers in the window (3 times the account's median views or more). Then `tiktok_get_videos` with `sort: "popular"`, one page (1 credit), for the all-time top videos, and note how old they are.
4. **What the top videos are.** `tiktok_get_transcript` on the three strongest recent outliers per account (1 credit each), with the first line of each caption: the topic, the format, the hook and the call to action. Note any series or format the account repeats.
5. **Paid posts.** Count the rows with `is_ad: true`: ads and paid partnerships in the account's own feed, and what they promote. For the ads the competitor runs in TikTok's ad library, open the paid-ads group's [competitor ads](../../paid-ads/references/competitor-ads.md); do not pull them here.
6. **Deliver** a table, one row per account: handle and link, followers, videos in the window, videos a week, median views, median engagement rate, median length, main formats, paid posts, the top three recent videos (link, views, multiple of the median, what each is in a few words). Below it, one line per competitor: what the user should take and what to avoid.

## Judgment

- Followers say little on TikTok, where most views come from people who do not follow. Compare accounts on median views.
- The count of outliers in 90 days says more than the single biggest video. An account with six outliers has a repeatable format; an account with one got lucky.
- Formats a brand keeps posting are usually the ones that work for it. A format it tried once and dropped probably did not.
- A competitor whose cadence fell in the last month may have cut its TikTok effort. Say so; it is an opening.
- The tools see what is public. Follower growth over time, ad spend and the account's own analytics are not there. For growth over time, the host re-runs this and keeps the table: the [monitoring](../../monitoring/references/competitor-watch.md) group's competitor watch.
- For everything about a competitor beyond TikTok, use the [competitors](../../competitors/references/teardown.md) group's teardown.
