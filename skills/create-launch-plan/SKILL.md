---
name: create-launch-plan
description: When the user wants to launch a product, a feature or a relaunch on a date. Builds a launch plan from a month before to a month after, finds the communities to launch in (subreddits and their self-promotion rules, Facebook groups, others the user names), the creators to seed, and the press list with its embargo timing, then reports what people said after launch on Reddit, TikTok, YouTube and LinkedIn. Also use when the user mentions a launch plan, launch checklist or timeline, launch day, a Product Hunt or Hacker News launch, Show HN, a beta, waitlist or feature launch, where to post a launch, creators or press for a launch, an embargo, a post-launch report, or launch reactions. A go-to-market plan goes to create-gtm-plan, creators outside a launch to find-creators, press outside a launch to find-journalists or create-digital-pr-plan, mentions after the launch window to monitor-brand-mentions.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Launch

A launch is a date that concentrates attention. This skill decides where the launch's attention comes from (communities, creators, press, the user's own audience), what has to happen before the date for each, and how the result is measured after it. It runs like a strategy (intake, baseline, gaps, tactics) but ends in a launch timeline from T-30 to T+30 instead of a 90-day plan; communities, creators, press and the reaction report each have a reference.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_posts` and `tiktok_search_videos` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The steps read Reddit, the social platforms (`tiktok_*`, `youtube_*`, `linkedin_*`, `instagram_*`, `facebook_*`), `seo_*` and, for press contacts, `leads_*`. If some of these tools are there and others are not, the missing tool groups are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, carry on with the rest, and list the platforms that went unchecked.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product, the ICP and the problem in their words, the competitors, the goal and the conversion that counts, the social accounts) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Launch**: what launches (a new product, a major feature, a relaunch) and the date. Default date: 30 days from today, so every step has time.
- **Goal**: signups, paying customers, waitlist, or coverage. Default: signups in the first week.
- **ICP**: who should hear about it, and the problem in their words.
- **Audience**: what the user already has: an email list or waitlist, followers per platform, community accounts with a history. Default: none.
- **Budget**: credits for the research and money for creators or ads. Default: 500 credits and no paid budget. The plan costs about 30 credits; communities about 96 with the skills it runs; creators about 190; press about 370; a reaction report about 42 per pass at T+1 and T+7, and 64 at T+30. Launch-day pulse checks add about 1 credit per subreddit per check. Say the total for the parts chosen before the first paid call; pass `max_credits` if the user gave a budget.
- **Team**: who posts, who answers comments on the day, who pitches press. Default: the founder alone.
- **Competitors**: two or three whose launches the plan learns from.
- **Venues**: the ones the user has in mind (Product Hunt, Hacker News, a subreddit, a Slack group).
- **Horizon**: T-30 to T+30.

## Steps

A request for one part only opens its reference directly: where to post and what each community allows, [communities](references/communities.md); who to seed, [creators](references/creators.md); the press list and embargo, [press](references/press.md); what people said since launch day, [reaction report](references/reaction-report.md). Otherwise:

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 30 credits.
   - Demand: `seo_search_keywords` with the category as `seed` (10 credits): whether people already search for this kind of product. `seo_get_keyword_metrics` with the brand and the competitors' names as `keywords` (about 6 credits): the branded search to compare with at T+30.
   - Where the ICP talks: `reddit_search_subreddits` with the problem the product solves (1 credit), and `tiktok_search_videos`, `youtube_search_videos` and `linkedin_search_posts` with the category and `since: "month"` (1 credit each). Note the platforms with recent posts that draw views and comments.
   - How competitors launched: `reddit_search_posts` with "<competitor> launch" at the default relevance sort (1 credit each), keeping the rows on topic as the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md) says, and `seo_get_serp` for "<competitor> launch" (1 credit each): which threads and articles covered them, and when.
   - The user's own reach: the profile tool of each platform they post on (`linkedin_get_profile`, `tiktok_get_profile`, `instagram_get_profile`, `youtube_get_channel`, `twitter_get_profile`; 1 credit each).
3. **Gaps.** Against the competitors and the market: platforms where the ICP talks and the user has no presence; communities that want weeks of participation before a launch post; no press relationships; no creators; branded search near zero. Each gap becomes a line of the timeline, or a reason to move the date.
4. **Tactics.** Choose from the references, and say why each fits the baseline:
   - [Communities](references/communities.md) when step 2 found subreddits or groups where the ICP asks about the problem. Almost every launch needs it.
   - [Creators](references/creators.md) when step 2 found category videos with views on TikTok or YouTube, and there is product to send or money to pay.
   - [Press](references/press.md) when there is news beyond "we launched" (a new category, data, a known founder, funding) and competitors' launches got coverage in step 2.
   - [Reaction report](references/reaction-report.md) always, at T+1, T+7 and T+30.
   - Beyond this skill: the launch content across channels in [create-content-calendar](../create-content-calendar/SKILL.md), and launch ads from [write-ad-brief](../write-ad-brief/SKILL.md) when there is a paid budget.
5. **Timeline.** A table with one row per action: when (T-30, T-7, launch day, T+7, T+30), action, venue, owner, the reference or skill that does it, and the KPI.
   - T-30: venues chosen and their rules read, community accounts active, creators shortlisted, press list built.
   - T-7: embargoed briefings out, product with the creators, one post drafted per community, the email to the list written.
   - Launch day, in hours from go-live (H): at H-2 the founder tests signup, payment and analytics end to end, with one UTM link per venue; at H0 the primary venue goes live (Product Hunt's day starts at 12:01 a.m. Pacific; the user confirms each venue's timing); at H+1 the email to the list; at H+2 the founder's own posts; then one community post every 60 to 90 minutes, never two at once, with creators posting through the day. At about H+3, H+6 and H+10, a pulse check: `reddit_get_new_posts` on the launch subreddits with `since: "6h"` and `match` set to the names (1 credit per subreddit, never cached), and `linkedin_search_posts` with the name and `since: "day"` (1 credit). Every open question goes on the founder's reply list.
   - T+7: the first full reaction report, replies to open questions, press follow-ups with early numbers.
   - T+30: the second reaction report, branded search measured again, and the watch handed to [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
   - KPIs the tools can measure again later: mentions since launch (the searches in the reaction report), branded search (`seo_get_keyword_metrics` on the brand; its `trend[12]` is monthly, so read it at T+30, not before), and referring domains from coverage (`seo_get_backlink_summary`, 10 credits). With Search Console connected, `console_get_search_analytics` with `dimensions: ["date"]`, a `filters` entry on `query` with `operator: "contains"` and the brand, and `end_date` set to the latest day (free; Google's data lags about two days) shows branded impressions day by day from T-7: the first awareness read at T+3, not T+30. Signups and sales come from the user's analytics.
6. **Deliver** one document: the inputs with defaults marked, the baseline in a short table, the gaps, the chosen tactics with the linked reference and why, the timeline table, and three first actions for this week.

## Judgment

- A launch cannot build an audience in a day. Venues that want a history (most subreddits, many groups) need the account active from T-30; if the date does not allow it, move the date or drop the venue.
- Two or three venues done well beat eight done badly. A founder alone cannot answer comments in eight places on one day.
- Product Hunt, Hacker News, Indie Hackers, Slack and Discord communities and newsletters are common launch venues, and no manifold tool reads them. Name them in a plan with the user's own notes, and have the user read each venue's current rules and timing themselves. Never report votes, rankings, comments or any number from them; pasted comments go in the reaction report, marked as pasted.
- Low search demand is normal for a new category. It means the launch has to create demand rather than capture it, so communities and creators matter more than search.
- Launch day is one day. Plan T+7 and T+30 (the post with results, an answer to every question, press follow-ups with numbers) with the same care; that is where a quiet launch recovers.
- Competitor launches from years ago are weak evidence for venue and timing today; prefer the last year's.
- Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- The deliverable is a plan or a table. Posting, emailing, scheduling and submitting stay with the user or the host's own tools; offer to pass the table on. The server keeps no state: the host keeps the launch date, the plan and the launch post URLs for the reaction report.

## Timeline

- Every part dates its rows in days from launch day, so their outputs drop into one plan. The milestones are T-30 (a month out), T-7, launch day, T+7 and T+30; a part adds a day between them where its work needs one (T-14 for a press exclusive, T-21 to ship product to creators).
- The launch date comes first. A part run without one asks for it; with fewer than 30 days left, it says which T-30 work no longer fits.

## Related skills

- A go-to-market plan for a new product, of which the launch is one step: [create-gtm-plan](../create-gtm-plan/SKILL.md).
- Subreddits, their rules and threads outside a launch: [find-subreddits](../find-subreddits/SKILL.md) and [find-reddit-threads](../find-reddit-threads/SKILL.md). Facebook groups outside a launch: [mine-facebook-groups](../mine-facebook-groups/SKILL.md).
- Creators for an ongoing program rather than a launch date: [create-influencer-plan](../create-influencer-plan/SKILL.md) and [find-creators](../find-creators/SKILL.md).
- A media list for a story that is not a launch: [find-journalists](../find-journalists/SKILL.md). A PR plan beyond launch week: [create-digital-pr-plan](../create-digital-pr-plan/SKILL.md).
- Mentions after the launch window, every day or week: [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
- Launch ads: [write-ad-brief](../write-ad-brief/SKILL.md). The launch content calendar across channels: [create-content-calendar](../create-content-calendar/SKILL.md).
