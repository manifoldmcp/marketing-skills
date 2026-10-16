# A LinkedIn plan

When the user wants a plan rather than a check: whose voice carries the account, what the ICP already engages with, and which tactics earn the most reach and pipeline for the hours the team has, over 90 days. If the user has not chosen LinkedIn yet, settle that first with [growth-strategy](../../growth-strategy/SKILL.md).

## Inputs

Ask in one message; use the default for anything unanswered and say so.

- **Mode:** founder-led (a person's profile), the company page, or both. Default: founder-led, with the page resharing (see [profiles and pages](profiles-and-pages.md)).
- **Goal:** pipeline, hiring, awareness or investors. Default: pipeline, measured by conversations the posts start.
- **Stage:** pre-launch, early customers or established; it decides whether the story is building in public or results.
- **ICP:** the titles and companies that buy, so topics are theirs and not the founder's peers'.
- **Time and credits:** default 2 to 4 posts a week, 20 minutes a day of comments, about 60 credits for the research.
- **Team:** who posts, who writes, whether employees will post too. Default: the founder writes and posts, no ghostwriter.
- **Competitors:** two or three companies, and two or three people whose audience overlaps.

## Baseline and gaps

About 60 credits.

- **Founder:** `linkedin_get_profile` and `linkedin_get_post` on 10 recent posts: followers, median likes and comments, per 1,000 followers, posts a week, topic spread (one topic area or many?), substance (a concrete detail and lived experience in each?). Ask for the user's own analytics: impressions per post and the in-network and out-of-network split.
- **Page and rivals:** the audit in [profiles and pages](profiles-and-pages.md#auditing-a-company-page), short form.
- **Topic gap:** four ICP topics, two pages each with `since: "month"`: which angles earn the most per 1,000 followers, and whether the founder or page posts on any of them.
- **Voice gap:** the authors behind the top posts (from the rows, no extra call): their followers and per-1,000 engagement against the founder's. These are the people to learn from and comment on.

Set expectations from [the feed](feed.md#reach-how-far-it-fell): reach for active posters is roughly half its 2024 level, so a plan that promises 2023 numbers fails on arrival.

## Tactics

Choose two or three that fit the numbers, and say why:

- **Commenting** ([commenting](commenting.md)) when the founder has under a few thousand followers: it reaches the audience before their posts can.
- **Formats** ([formats](formats.md)) when the founder posts but per-follower engagement sits below the niche median.
- **Topic leaders** ([topic-leaders](topic-leaders.md)) when the founder does not know who owns the conversation.
- **Buyer posts** ([buyer-posts](buyer-posts.md)) when the goal is pipeline.
- **Page audit** ([profiles and pages](profiles-and-pages.md#auditing-a-company-page)) in company mode, or when a rival's page clearly outperforms.
- **Articles or a newsletter** ([AI answers](ai-answers.md)) when the user's buyer questions already cite LinkedIn in AI answers.
- **Outbound to the same buyers** belongs to [outbound](../../outbound/SKILL.md); paying to boost the founder's best posts to [paid-ads](../../paid-ads/SKILL.md).

## The 30-60-90 days

| Period | Typical work | KPI the tools re-measure |
|---|---|---|
| Days 1 to 30 | A cadence the founder keeps (2 to 4 a week) on the one or two topics with the most per-follower engagement; every post with a concrete detail or lived experience; a daily list of 10 posts to comment on | Posts a week; median likes and comments on the last 10 posts |
| Days 31 to 60 | Double the formats that worked; one recurring series; buyer-post list weekly if the goal is pipeline | Median per 1,000 followers against the day-1 baseline; founder followers |
| Days 61 to 90 | A second topic, or a newsletter if the first holds; in company mode, the page resharing the founder plus one post of its own a week; employees posting | How often the founder appears among the top authors for the topic searches; page posts a week and their median engagement |

Give each period an owner and three first actions for this week.

- **KPIs the tools can read again:** founder followers (`linkedin_get_profile`), median likes and comments on the last 10 posts (`linkedin_get_post` on each URL; the host keeps the URLs, since `recent_posts` reaches back only five or six), page posts a week (`linkedin_get_company_posts`), and the founder's share of top authors in the topic searches (`linkedin_search_posts`). Impressions, reach and leads are not visible to the tools; the user reads them from their own analytics and CRM.
- **Set targets above the noise.** On 10 posts one viral post moves the median little and the mean a lot; report medians and call a change real only when it holds for a month.
- **Consistency beats volume**, but a missed week costs nothing (a first post after a break ran 19% above average). Three posts a week for 90 days beat daily posts for three weeks and then silence.
- **Weight topics by what the ICP engages with**, not peers and other founders; ask the user who wrote to them after a post.
- **Never post, schedule or comment for the user,** and never suggest pods, engagement groups or comment tools: LinkedIn detects and limits them (see [commenting](commenting.md)).
- A weekly re-measure is the host's schedule; the server keeps no state.
