# Launch plan

A launch plan decides where the launch's attention comes from (communities, creators, press, the user's own audience), what has to happen before the date for each, and how the result is measured after it. It runs like a strategy (intake, baseline, gaps, tactics) but ends in the launch timeline from the [router](../SKILL.md#timeline) instead of a 90-day plan.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Launch**: what launches (a new product, a major feature, a relaunch) and the date. Default date: 30 days from today, so every step has time.
- **Goal**: signups, paying customers, waitlist, or coverage. Default: signups in the first week.
- **ICP**: who should hear about it, and the problem in their words.
- **Audience**: what the user already has: an email list or waitlist, followers per platform, community accounts with a history. Default: none.
- **Budget**: credits for the research (this playbook costs about 30) and money for creators or ads. Default: 500 credits and no paid budget.
- **Team**: who posts, who answers comments on the day, who pitches press. Default: the founder alone.
- **Competitors**: two or three whose launches the plan learns from.
- **Venues**: the ones the user has in mind (Product Hunt, Hacker News, a subreddit, a Slack group).
- **Horizon**: T-30 to T+30.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 30 credits.
   - Demand: `seo_search_keywords` with the category as `seed` (10 credits): whether people already search for this kind of product. `seo_get_keyword_metrics` with the brand and the competitors' names as `keywords` (about 6 credits): the branded search to compare with at T+30.
   - Where the ICP talks: `reddit_search_subreddits` with the problem the product solves (1 credit), and `tiktok_search_videos`, `youtube_search_videos` and `linkedin_search_posts` with the category and `since: "month"` (1 credit each). Note the platforms with recent posts that draw views and comments.
   - How competitors launched: `reddit_search_posts` with "<competitor> launch" at the default relevance sort (1 credit each), keeping the rows on topic as the [reddit router](../../reddit/SKILL.md) says, and `seo_get_serp` for "<competitor> launch" (1 credit each): which threads and articles covered them, and when.
   - The user's own reach: the profile tool of each platform they post on (`linkedin_get_profile`, `tiktok_get_profile`, `instagram_get_profile`, `youtube_get_channel`, `twitter_get_profile`; 1 credit each).
3. **Gaps.** Against the competitors and the market: platforms where the ICP talks and the user has no presence; communities that want weeks of participation before a launch post; no press relationships; no creators; branded search near zero. Each gap becomes a line of the timeline, or a reason to move the date.
4. **Tactics.** Choose from this group, and say why each fits the baseline:
   - [Communities](communities.md) when step 2 found subreddits or groups where the ICP asks about the problem. Almost every launch needs it.
   - [Creators](creators.md) when step 2 found category videos with views on TikTok or YouTube, and there is product to send or money to pay.
   - [Press](press.md) when there is news beyond "we launched" (a new category, data, a known founder, funding) and competitors' launches got coverage in step 2.
   - [Reaction report](reaction-report.md) always, at T+1, T+7 and T+30.
   - Beyond this group: the launch content across channels in the [content calendar](../../content/references/calendar.md), and launch ads from a [creative brief](../../paid-ads/references/creative-brief.md) when there is a paid budget.
5. **Timeline.** A table with one row per action: when (T-30, T-7, launch day, T+7, T+30), action, venue, owner, the playbook that does it, and the KPI.
   - T-30: venues chosen and their rules read, community accounts active, creators shortlisted, press list built.
   - T-7: embargoed briefings out, product with the creators, one post drafted per community, the email to the list written.
   - Launch day: posts go out staggered across the day, the founder answers every comment, the email goes, creators post.
   - T+7: the first full reaction report, replies to open questions, press follow-ups with early numbers.
   - T+30: the second reaction report, branded search measured again, and the watch handed to [brand mentions](../../monitoring/references/brand-mentions.md).
   - KPIs the tools can measure again later: mentions since launch (the searches in the reaction report), branded search (`seo_get_keyword_metrics` on the brand; its `trend[12]` is monthly, so read it at T+30, not before), and referring domains from coverage (`seo_get_backlink_summary`, 10 credits). Signups and sales come from the user's analytics.
   - **Deliver** one document: the inputs with defaults marked, the baseline in a short table, the gaps, the chosen tactics with the linked playbook and why, the timeline table, and three first actions for this week.

## Judgment

- A launch cannot build an audience in a day. Venues that want a history (most subreddits, many groups) need the account active from T-30; if the date does not allow it, move the date or drop the venue.
- Two or three venues done well beat eight done badly. A founder alone cannot answer comments in eight places on one day.
- Product Hunt and Hacker News can sit on the timeline, but no tool here reads them. The user checks their current rules and the best time to post there.
- Low search demand is normal for a new category. It means the launch has to create demand rather than capture it, so communities and creators matter more than search.
- Launch day is one day. Plan T+7 and T+30 (the post with results, an answer to every question, press follow-ups with numbers) with the same care; that is where a quiet launch recovers.
- Competitor launches from years ago are weak evidence for venue and timing today; prefer the last year's.
