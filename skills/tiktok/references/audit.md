# Auditing a TikTok account

What an account does on TikTok and what works for it, the user's own or a rival's, side by side over the same window. The numbers are read as [reading the numbers](reading-the-numbers.md) says; this page is what to pull and what to conclude.

## What to pull, per account

| Question | Call | Credits |
|---|---|---|
| Size, bio link, `posts_count` | `tiktok_get_profile` | 1 |
| Cadence, medians, formats, lengths, paid share over 90 days | `tiktok_get_videos` with `sort: "latest"`, paged until the window is covered (10 a page; three pages for an account posting two or three a week) | 1 a page |
| All-time top videos and their age | `tiktok_get_videos` with `sort: "popular"`, one page | 1 |
| What the recent outliers say and how they open | `tiktok_get_transcript` on the three strongest recent outliers | 1 each |
| What the brand pays to push | `ads_get_advertiser_ads` with `platform: "tiktok"` and the brand name; read rows with a recent `last_shown` (`active` is null on TikTok) | 1 |

About 10 to 12 credits an account; 48 for the user and three rivals. If the user names brands but not handles, the handle is the `author` of a brand video in `tiktok_search_videos` on the brand name (1 credit). Wrong handles come back `NoData`: fix before paying for listings.

## The table

One row per account: handle and link, followers, videos in the window, videos a week, share of weeks with a video and the longest gap, median views, median engagement rate, median shares and saves over views, median `duration_s`, share of photo posts (`media: "image"`), paid share (`is_ad`), live ads and any that repeat an organic video, and the top three recent outliers (link, views, multiple of the median, what each is in a few words). Below it, one line per rival: what the user should take and what to avoid.

## Reading it

- **Compare on median views, not followers.** Most views come from people who do not follow; a rival with half the followers and twice the median is the stronger account.
- **Same window, same rules.** Compare accounts only over the same weeks, with paid rows out, pinned rows ordered by date, and videos under 7 days old dropped.
- **Count outliers, then read them.** The formats and topics of a rival's outliers show what works for a brand like the user's. A format a brand keeps posting usually works for it; one tried once and dropped probably did not. Six outliers in a series are a format; one is luck.
- **All-time tops date fast.** A rival's most viewed videos can be years old and made for a different TikTok (before 2025's squeeze on small accounts, before longer videos paid). Weight the last 90 days.
- **Paid is its own story.** An account whose rows are mostly `is_ad` buys its reach; its organic medians rest on few videos, so say how many. An organic video that reappears as an ad is a proven seller. A full read of the ads belongs to [paid-ads](../../paid-ads/SKILL.md).
- **Cadence tells effort.** Bursts and long gaps mean no cadence to copy. A rival whose cadence fell in the last month may have cut its TikTok effort: an opening.
- **Audience location.** For the user's own account, when they sell in a few countries, `tiktok_get_audience` (26 credits) shows the share of followers by country from a sample of a few hundred: views from outside the market are reach that cannot buy.
- **Brand accounts in B2B often underperform the creators in their niche.** In our "crm software" sample HubSpot's latest ten posts had a median of about 1,000 views and Salesforce's video in the top 30 had 1,014, against 335,000 for an explainer account's history of Salesforce. Say so when the user's category behaves like that: the reach sits with creators and explainers, which points to [creators](../../creators/SKILL.md).

## What the audit cannot see

Follower growth over time, watch time, completion, traffic sources, profile visits and link clicks. For the user's own account ask for TikTok Studio's numbers; for growth over time the host re-runs the same audit and keeps the table (the server keeps none). A monitoring schedule for rivals belongs to [competitor-intel](../../competitor-intel/SKILL.md).
