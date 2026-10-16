# An Instagram plan

When the user wants a plan to grow on Instagram rather than one check: where the account stands, what wins in its niche and for its competitors, which tactics close the gap for the hours the team has, over 30-60-90 days. If the user has not decided Instagram is the right channel, settle that first with [growth-strategy](../../growth-strategy/SKILL.md).

## Inputs

Take from the product context file what it answers; ask for the rest in one message and mark defaults.

- **Goal:** awareness (default, measured in median reel views), followers, traffic and signups, or sales.
- **Stage:** no account, new (under about 1,000 followers), or established.
- **Niche tags:** two or three the niche really uses (default: the category tag, then tags recurring in its first results' captions).
- **Competitors:** two or three handles (default: the brand accounts that recur in the tag search).
- **Team and hours:** who can be on camera, who edits, who designs carousels (default: the founder on camera with a phone, no designer, 4 hours a week).
- **Budget:** about 45 credits of research, plus any money for creators or ads.

## Baseline and gaps (about 45 credits)

1. **Baseline** the user and three competitors as in [the audit](audit.md), lighter: profile, two pages of posts and two of reels each (about 20 credits). With no account yet, baseline the competitors only.
2. **The niche:** `instagram_search_posts` on each tag with `since: "month"`, two pages (6 credits). Who wins (creators, brands, the user), the share of reels, recurring topics. Topics the niche rewards that the user never covers are the first gap.
3. **How winners open:** one page of `instagram_get_reels` on the six niche authors with the most views, then `instagram_get_transcript` on the six reels with the highest multiple of their author's median, competitors' outliers included (12 credits). See [hooks](hooks.md).
4. **What the audience asks:** one page of `instagram_get_comments` on the three most-commented niche posts (3 credits). Count the questions; see [comments](comments.md).
5. **Originality check** on the user's own feed: if most recent posts are reposts or lightly edited clips, nothing else matters until that changes ([ranking](ranking.md#originality)).
6. **Competitors' paid push:** one `ads_get_advertiser_ads` call per competitor (see [the audit](audit.md)).

## Choosing tactics

Pick two or three and say why each fits the numbers:

- **Trends** ([trends](trends.md)) when several niche accounts repeat a format this month: the cheapest reach while the account is small.
- **Hooks** ([hooks](hooks.md)) when the user's median reel views trail competitors' on the same topics: the opening is usually what differs, and skip rate in Insights confirms it.
- **One reel's breakdown** ([viral reel](viral-reel.md)) when a competitor or the user has an outlier worth repeating.
- **Carousels for the questions** ([comments](comments.md)) when the niche's comments are full of how-to questions: answers people save.
- **Collabs and creators** ([creators](../../creators/SKILL.md)) when nobody on the team can be on camera, or the goal needs reach faster than an account grows. Creator-published Collabs outperformed brand-only posts in Emplifi's 2026 data.
- **Paid amplification** ([paid-ads](../../paid-ads/SKILL.md)) of organic posts that already beat the median, when there is budget.

## The 30-60-90 days

| Period | Typical work | KPI the tools re-measure |
|---|---|---|
| Days 1 to 30 | A cadence the team can hold (default three reels and one carousel a week; Buffer's data favours 3 to 5 posts a week over 1 to 2). Three reel formats and three hook patterns from the gaps, each first as a Trial Reel so a miss does not cost the grid. Fix any originality problem. | Posts a week; median reel views over the last 10 reels |
| Days 31 to 60 | Keep the format with the best median views and drop the rest; answer the top comment questions with carousels; first Collab with a creator or partner brand | Median reel views; outliers in the last 20 reels; image-post engagement over followers |
| Days 61 to 90 | Scale the winning format; a creator programme through [creators](../../creators/SKILL.md) or a paid test of the best organic posts through [paid-ads](../../paid-ads/SKILL.md) | Median reel views and engagement against the day-1 baseline and against competitors; presence in the top results for the niche tags |

Give each period an owner and three first actions for this week.

## Setting targets honestly

- Judge progress on median reel views and engagement, not followers: followers lag both by weeks, and 2025 follower growth halved across every tier on Socialinsider's data (see [benchmarks](benchmarks.md)).
- Two reels a week the team can hold beat daily reels that stop in week three.
- The tools cannot see sends, saves, reach, watch time, skip rate, profile visits, link clicks or stories: ask the user for Insights at each review. If the goal is signups or sales, the KPI is the user's own analytics on the bio link.
- Never promise virality. Instagram tests posts from small accounts on non-followers, so any account can break out, but no one can schedule it.
- Deliver one document: inputs with defaults marked, the baseline table, the gaps in three lines each, the tactics and why, the 30-60-90 table, the first actions. Never post, comment, follow, DM or schedule; write hooks or scripts only when asked, each on a pattern the evidence shows.
