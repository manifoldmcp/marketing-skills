# Auditing an Instagram account

What an account does on Instagram and what works for it, the user's own or a competitor's, judged against its own baseline and set beside rivals over the same window. Read [the data notes](data.md) first: most audit mistakes are field traps.

## What to pull

For each account, over the last 90 days by default:

| Call | Cost | For |
|---|---|---|
| `instagram_get_profile` | 1 | Followers, bio, website, `industry` |
| `instagram_get_posts`, about three pages | 3 | Cadence, format mix, image-post engagement over followers, captions, collabs |
| `instagram_get_reels`, two pages or until 90 days are covered | 2+ | Reel cadence, median views, engagement over views, median `duration_s`, outliers |
| `instagram_get_transcript` on the three strongest recent outliers | 3 | Topic, format, hook, call to action |
| `ads_get_advertiser_ads` with `platform: "facebook"`, the brand's page name, `active_only: true` | 1 | Live Meta ads; keep rows whose `placements` include instagram |

About 10 credits an account, 40 for the user and three competitors. Say the estimate before the first call. If the user names brands but not handles, read the `author` of posts under the brand's own hashtag (1 credit) or ask; if they do not know their competitors, the [competitor-intel](../../competitor-intel/SKILL.md) skill finds them.

## What to read

- **Size against reach.** Compare accounts on median reel views and engagement, not followers. Followers pile up over years; reel views show what Instagram gives the account now. A brand whose median reel reaches 3% of followers is normal (see [field notes](field-notes.md)); one that reaches far less than its direct rivals has a content problem.
- **Repeatable or lucky.** Count outliers in the window. Six outliers on one format mean a format that works for that brand; one means luck. A format a brand keeps posting usually works for it; one it tried once and dropped probably did not.
- **Format mix against the evidence.** Reels for reach to non-followers, carousels for saves and depth (see [benchmarks](benchmarks.md)). An account posting only single images is leaving reach on the table; one posting only reels may be missing the saves carousels earn on how-to topics.
- **Originality risk.** A feed that is mostly reposted clips, memes or customer videos without real changes is ineligible for recommendations since April 2026 (see [ranking](ranking.md#originality)). Flag it; it explains flat reach better than anything else.
- **Collabs and paid posts.** Rows by another `author` are collabs; `is_ad: true` reels and #ad captions are paid partnerships. Keep both out of the organic medians and report them in their own columns, with the creators named: creators a competitor already paid have sold something like the user's product, a head start for the [creators](../../creators/SKILL.md) skill.
- **Organic posts the brand paid to push.** An ad whose `body` repeats an organic caption is a post the brand put money behind, the strongest public sign it sells; an ad live for months (`first_shown`) is one that pays. A full read of a rival's ads belongs to [paid-ads](../../paid-ads/SKILL.md).
- **Momentum.** A rival whose cadence fell in the last month may have cut its effort; say so, it is an opening.
- **For the user's own account**, ask for the Insights the tools cannot see: views from non-followers, skip rate, average watch time, sends and likes per reach (the signals Instagram names), saves, and profile visits. Reels with a high skip rate fail in the first three seconds; reels with low non-follower views are not being recommended, and the originality check above is the first suspect.

## The deliverable

A table, one row per account: handle and link, followers, posts in the window, posts a week, reels a week, median reel views, median reel engagement over views, image-post engagement over followers, outliers, collabs, paid posts, live ads and those repeating an organic post, and the top three recent posts (link, views or likes, multiple of the median, what each is in a few words). Below it, one line per competitor: what the user should take and what to avoid. Compare accounts only within Instagram; a brand's TikTok or Facebook numbers go in their own table ([tiktok](../../tiktok/SKILL.md), [facebook](../../facebook/SKILL.md)).

The tools see what is public now. Follower growth over time needs the host to re-run the audit and keep each table; the server keeps no history. Never post, comment, follow or message.
