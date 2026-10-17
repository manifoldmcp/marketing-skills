# A growth plan

The answer to "how do we grow" when the goal is known and the channel is not: one goal, a cheap reading of every channel the tools can see, the gaps where buyers and competitors are and the user is not, two or three channels, and a 30-60-90 day plan with KPIs. The work inside each channel belongs to that channel's topic skill; this plan chooses and orders them.

## Settle first

Ask in one message; use the default and say so for anything unanswered.

- **Goal**: one of traffic, signups, leads, awareness, first customers. Default signups. Ask for the deciding KPI's current number, or day 90 has nothing to compare with.
- **Stage**: pre-launch, under 100 customers, or growing. Default under 100.
- **ICP**: who buys, B2B or B2C, and the problem in their words.
- **Price**: it sets the motion ([channel selection](channel-selection.md#price-sets-the-motion)).
- **Budget and team**: credits, money for ads or creators, hours a week, and who can write, film, sell or design. Default one founder who writes, 5 hours a week, no ad budget.
- **Competitors**: two or three. Default the brands the AI answers in the baseline name.

## Goal and KPI

One primary goal per plan. The goal sets the KPI, and the KPI decides which channels count.

| Goal | The KPI that decides | Leading signals the tools re-measure |
|---|---|---|
| Traffic | Organic visits (user's analytics) | Clicks and impressions (`console_get_search_analytics`, free), estimated organic traffic (`seo_get_domain_overview`), target keyword ranks (`seo_get_position`, 6 each) |
| Signups | Signups a week (user's analytics) | The traffic signals, plus mentions and referring domains |
| Leads | Qualified leads or meetings (user's CRM) | Reachable market (`leads_search_companies`), posts about the problem (`linkedin_search_posts`) |
| Awareness | Branded search (`seo_get_keyword_metrics` on the brand, or Search Console queries containing it) | AI mention rate (`aeo_run_ai_answers`), mentions by others ([brand-monitoring](../../brand-monitoring/SKILL.md)) |
| First customers | Customers (user's count) | Conversations started a week, from a tally the plan sets up |

The tools see public signals, not conversion. Name the deciding KPI from the user's own data and use the tool signals as leading indicators only.

## Baseline

About 130 credits. Say so before the first paid call.

- **Search:** `seo_get_domain_overview` on the site and two competitors (15 each), and `seo_search_keywords` on the category (up to 10). Strip competitors' brand terms before judging demand.
- **AI answers:** `aeo_run_ai_answers` with two buyer prompts and `brands` set to the user and competitors (27 a prompt on the default engines), read with `get_task`.
- **Communities:** `reddit_search_subreddits` on the problem (1), then `reddit_get_new_posts` on the top three to five with `since: "7d"`, `pages: 3` and `match` set to the problem words (up to 3 a subreddit). If `coverage[]` shows the window cut, say how many days it covered. No community, or no post in the window, is a 0 marked "no signal this week".
- **Social:** `tiktok_search_videos`, `youtube_search_videos` and `linkedin_search_posts` on the category with `since: "month"` (1 each).
- **Paid:** `ads_get_advertiser_ads` per competitor with `platform: "google"` and its domain, plus `"linkedin"` for B2B or `"facebook"` for consumer (1 each).
- **B2B only:** `leads_search_companies` with the ICP's industries, headcount and locations (1 at `limit: 1`).

One AI run and one week of Reddit are samples: they point the plan; the channel's own skill measures properly.

## Gaps and tactics

Set the user against the competitors and the [signals](channel-selection.md#signals-a-channel-is-worth-it), rank the gaps by fit to the goal and by what the team can make, and choose two or three:

- **Pre-launch or under 100 customers**, whatever the goal: direct conversations first, from communities ([reddit](../../reddit/SKILL.md)), the founder's network and founder-sent outreach ([outbound](../../outbound/SKILL.md)). Scalable channels come after.
- **New product or unsettled ICP:** a [go-to-market](gtm-plan.md) first; a date adds a [launch](launch.md).
- **Traffic, with search demand above the floor:** [seo](../../seo/SKILL.md); quick wins first when the site already ranks 4 to 20 for keywords with volume. Links when the site's domain rank trails: [link-building](../../link-building/SKILL.md).
- **Engines name competitors and not the user:** [ai-search](../../ai-search/SKILL.md).
- **Weekly problem threads asking for tools:** [reddit](../../reddit/SKILL.md).
- **B2B leads:** [outbound](../../outbound/SKILL.md) and founder posts on [linkedin](../../linkedin/SKILL.md).
- **Short video with creator views in the category:** [tiktok](../../tiktok/SKILL.md), [instagram](../../instagram/SKILL.md) or [youtube](../../youtube/SKILL.md); [creators](../../creators/SKILL.md) when the team cannot film.
- **Competitors' ads running 90+ days and a budget:** [paid-ads](../../paid-ads/SKILL.md).
- **Awareness, or branded search flat while competitors' grows:** [brand-monitoring](../../brand-monitoring/SKILL.md) to measure, [digital-pr](../../digital-pr/SKILL.md) to move it.
- **A new country or segment:** [market-research](../../market-research/SKILL.md).

## The 30-60-90 days

| Period | Typical work | Check |
|---|---|---|
| Days 1 to 30 | The first move in each chosen channel done; the measurement in place (UTMs per channel, a "how did you hear about us" field, the KPI's starting number) | Leading signals re-measured with the same calls |
| Days 31 to 60 | Most hours to the channel with the best early signal; the weaker one held to a minimum | The deciding KPI against its start |
| Days 61 to 90 | Re-measure everything with the same calls, competitors and queries; drop or keep the weakest channel on the evidence | KPI change against the noise ([measurement](measurement.md#is-it-real)) |

Give each period an owner and a KPI, and three first actions for this week.

## Rules

- Measure again with the same calls, competitors and queries at day 30, 60 and 90, or the comparison says nothing. The server keeps no state: the host keeps the plan, the baseline and the KPIs, and sets any recurring re-measure in its own scheduler.
- A channel with no signal is not ruled out for a new category; demand may not exist anywhere yet. Say so rather than calling the channel dead.
- The channels' own skills state their costs; add them up before running more than one. A result this account already paid for is free while cached, so running the first channel skill soon after the plan costs less.
- Deliver one document: the inputs with defaults marked, a baseline table (channel, the user, each competitor, the signal), the gaps in a line each, the chosen channels with the topic skill and why, the 30-60-90 table, three first actions. Running campaigns, posting, sending and spending stay with the user.
