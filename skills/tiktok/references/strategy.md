# A TikTok plan

When the user wants a plan rather than a check: where the account stands, what wins in its niche and for its rivals, and which work closes the gap for the hours the team has, over 90 days. It ends in a 30-60-90 day plan with KPIs the tools can measure again, not a list of video ideas.

## Inputs

Settle in one message and mark defaults. If the user has not chosen TikTok yet, settle that first with [growth-strategy](../../growth-strategy/SKILL.md).

- **Goal:** awareness (median views per video, the default), followers, traffic and signups, or sales.
- **Stage:** no account, new (under about 1,000 followers), or established.
- **Niche terms:** two or three phrases people would search ("meal prep", "budget app"), in the user's words.
- **Hours a week** for filming and editing (default 4), **who can be on camera** (default the founder, on a phone), and any money for creators or ads.
- **Rivals:** two or three handles; default the three brand accounts that recur in the niche search.
- **Markets:** where the user sells, for the audience check and the language filter.

## Baseline and gaps

About 40 credits, 66 with the audience check.

- **Baseline (about 16):** the [audit](audit.md) table for the user and three rivals, two listing pages each plus one popular page. `tiktok_get_audience` on the user's own account (26) when they sell in a few countries.
- **The niche (about 6):** `tiktok_search_videos` per term, `since: "month"`, `sort: "popular"`, two pages. Who wins (creators, brands, the user), which formats, lengths and topics recur, and how thin the term is. Topics the niche rewards that the user never covers are the first gap.
- **Openings (about 12):** medians for the six niche authors with the most views, then transcripts of the six videos with the highest lift ([hooks and trends](hooks-and-trends.md)).
- **Questions (about 3):** one page of comments on the three most commented niche videos; count the questions ([comments](comments.md)).
- **Rivals' ads (1 each):** `ads_get_advertiser_ads` with `platform: "tiktok"`; read rows with a recent `last_shown`.

## Choosing the work

Pick two or three, and say which number made each fit:

- **Trends** when several niche authors repeat a format in the last month: the cheapest views while the account is small. Check the audio is open to a brand.
- **Hooks** when the user's median trails rivals on the same topics: the opening is usually what differs.
- **Search-led videos** when the niche is how-to or product review: answer the exact phrases people search, said out loud and in the caption ([how it ranks](how-it-ranks.md#tiktok-search)).
- **Comment answers** when the niche's comments are full of questions: each is a video, and a "reply to comment" video is a format in itself.
- **Longer formats** when the niche's top results run over a minute and the team has a subject that holds: test against the usual length ([content](content.md#length-and-watch-time)).
- **Creators** when nobody can be on camera, the category's brand accounts barely get views (common in B2B), or the goal needs reach faster than an account can grow: [creators](../../creators/SKILL.md).
- **Paid** when there is budget and an organic winner to push as a Spark Ad: [paid-ads](../../paid-ads/SKILL.md).

## The 90 days

| Period | Typical work | KPI the tools re-measure |
|---|---|---|
| Days 1 to 30 | A cadence the team can hold (default three a week, the efficient range is two to five), three formats and three hook patterns from the gaps, every sound checked for a brand licence | Videos a week and share of weeks posted; median views of each format |
| Days 31 to 60 | Keep the format with the best median, drop the rest; answer the top comment questions; one longer-format test | Median views and engagement over the last 20 videos; outliers (3 times the median) in the window |
| Days 61 to 90 | Double down on the series that produced outliers; a first creator partnership or a paid test if there is budget | Outlier count; presence in the top results for the niche terms; followers and `posts_count` |

Give each period an owner and three first actions for this week.

## Setting targets honestly

- **Promise inputs and medians, not virality.** The median video barely moves with frequency; what grows is the chance of a hit (Buffer, 2025). Target outlier counts and median views, and say a single hit is luck.
- **Small accounts start slow.** Average views for 1K to 5K accounts fell from 860 to 350 a post from 2024 to 2025 (Socialinsider). Set the 30-day median target from the account's own first 20 videos, not from a benchmark.
- **The tools cannot see watch time, completion, traffic sources, profile visits or link clicks.** At each review the user reads them in TikTok Studio; for signups or sales the KPI is the user's own analytics on the bio link. TikTok Shop and LIVE are outside the tools.
- **US accounts:** reach shifts in 2026 may come from the joint venture's retraining, which is undocumented. Re-check against rivals over the same weeks before changing the plan ([how it ranks](how-it-ranks.md#the-us-joint-venture)).
- **Weekly or monthly re-measures** are the host's schedule; the server keeps no history, so the host stores each run's table.
