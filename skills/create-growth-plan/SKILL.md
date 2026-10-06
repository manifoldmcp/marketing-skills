---
name: create-growth-plan
description: When the user wants to grow and knows the goal but not the channel. Settles one goal, takes a cheap baseline across Google search, AI answers, Reddit, TikTok, YouTube, LinkedIn, competitor ads and company counts, finds the gaps against competitors, and picks two or three channels in a 30-60-90 day plan with KPIs. Also use when the user mentions how do I grow my startup, a growth plan, growth strategy or marketing plan, a marketing plan for the next 90 days, more traffic, signups, users, leads or customers with no channel named, or traction. Which channels to be on goes to pick-channels, the first customers to find-first-customers, a new product to create-gtm-plan, a new country or segment to create-market-entry-plan, awareness to measure-brand-awareness, a dated launch to create-launch-plan. It plans; running campaigns, posting and sending stay with the user. Which skill to run first goes to manifold-get-started.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Growth plan

The answer to "where do I start". A growth plan settles one goal, takes a cheap reading of every channel the tools can see, finds where buyers and competitors already are and the user is not, and picks two or three channels. The work in each channel is another skill; this plan chooses them, orders them in a 30-60-90 plan and sets the KPIs.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `reddit_search_posts` (hosts often add a prefix, for example `mcp__manifold__seo_search_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The baseline reads `seo_*`, `aeo_*`, `reddit_*`, the social platforms, `ads_*` and `leads_*`. If some of these tool groups are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, carry on, and mark those channels "not measured" in the plan rather than calling them weak.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product, the ICP, the competitors, the goal, the conversion that counts and the budget) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: one of traffic, signups, leads, awareness, first customers. Default: signups. The KPI for each is in [Goal and KPI](#goal-and-kpi) below; ask for its current number.
- **Stage**: pre-launch, fewer than 100 customers, or growing. Default: fewer than 100 customers.
- **ICP**: who buys, B2B or B2C, and the problem in their words.
- **Budget**: credits for the research (this skill costs about 80), money for ads or creators, and hours a week. Default: 1,000 credits, no ad budget, 5 hours a week.
- **Team**: who does the work: writing, video, sales calls, design. Default: one founder who can write.
- **Competitors**: two or three. Default: the top real businesses from `seo_get_serp_competitors` (10 credits, only when the user names none), or the brands the AI answers name in step 2.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 80 credits. One cheap reading per channel:
   - Search: `seo_get_domain_overview` on the site and two competitors (5 credits each), and `seo_search_keywords` with the category as `seed` (10 credits) for demand.
   - AI answers: `aeo_run_ai_answers` with two buyer questions and `brands` set to the user and the competitors (18 credits a prompt), then `get_task`.
   - Communities: `reddit_search_subreddits` with the problem (1 credit), then `reddit_get_new_posts` on the top five with `since: "7d"` and `match` set to the problem words (5 credits): every post about the problem there in the last week. No community found, or no post in the week, is a 0 marked "no signal this week": do not fill the cell from search.
   - Social: `tiktok_search_videos`, `youtube_search_videos` and `linkedin_search_posts` with the category and `since: "month"` (1 credit each).
   - Paid: `ads_get_advertiser_ads` on each competitor with `platform: "google"` and its domain, plus `platform: "linkedin"` for a B2B buyer or `"facebook"` for a consumer (1 credit each). Count the ads that pass the paid signal in [channel signals](references/channel-signals.md): a Google text ad that has run for months is a competitor buying search demand.
   - B2B only: `leads_search_companies` with the ICP's `industries`, `employee_ranges` and `locations` (4 credits); `meta.rows_available` sizes the market.
3. **Gaps.** For each channel, set the user against the competitors and against the [channel signals](references/channel-signals.md): where buyers are active and competitors show up but the user does not. Rank the gaps by fit to the goal and by what the team can do.
4. **Tactics.** Choose two or three, and say why each fits the goal and the gaps:
   - [find-first-customers](../find-first-customers/SKILL.md) when the stage is pre-launch or under 100 customers, whatever the goal: channels that scale come after.
   - [create-gtm-plan](../create-gtm-plan/SKILL.md) when the product is new or the ICP is not settled; a launch date adds [create-launch-plan](../create-launch-plan/SKILL.md).
   - [measure-brand-awareness](../measure-brand-awareness/SKILL.md) when the goal is awareness, or the user's branded search is flat while the competitors' grows.
   - [create-market-entry-plan](../create-market-entry-plan/SKILL.md) when the growth is in a new country or segment.
   - Traffic: [create-seo-plan](../create-seo-plan/SKILL.md) when search demand clears the signal; [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md) first when the site already ranks 4 to 20 for keywords with volume.
   - AI answers: [create-ai-search-plan](../create-ai-search-plan/SKILL.md) when the engines name competitors and not the user.
   - Communities: [create-reddit-plan](../create-reddit-plan/SKILL.md) when the problem comes up every week and threads ask for a tool like this.
   - Leads, B2B: [create-outbound-plan](../create-outbound-plan/SKILL.md) for outbound, and [create-linkedin-plan](../create-linkedin-plan/SKILL.md) for founder-led posts.
   - Short video: [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md) or [create-youtube-plan](../create-youtube-plan/SKILL.md), or [create-influencer-plan](../create-influencer-plan/SKILL.md) when the team cannot make video itself.
   - Paid: [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md) when competitors' ads run long and there is a budget.
   - Links and press: [create-link-building-plan](../create-link-building-plan/SKILL.md) when search is chosen and the site's domain rank trails the competitors'.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week. By day 30, the first skill of each chosen channel has run and its first actions are done; by day 60, the channel with the best early signal gets most of the hours; by day 90, the KPIs are measured again and the weakest channel is dropped or kept on the evidence.
   - KPIs the tools can measure again later: clicks and impressions from Search Console when connected (`console_get_search_analytics`, free), estimated organic traffic (`seo_get_domain_overview`), ranks for target keywords (`seo_get_position`, 6 credits each), AI mention rate on the same prompts (`aeo_run_ai_answers`), branded search (`seo_get_keyword_metrics` on the brand, `trend[12]`), and weekly posts about the problem in the same subreddits (`reddit_get_new_posts`). Signups, leads and revenue come from the user's analytics and CRM: the plan names the deciding KPI from [Goal and KPI](#goal-and-kpi) and uses the tool KPIs as leading signals.
6. **Deliver** one document: the inputs with defaults marked, a baseline table (channel, the user, each competitor, the signal), the gaps in a line each, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions. Offer to run the first skill.

## Judgment

- Two or three channels for a small team, never all of them. The plan's value is in what it leaves out, with the reason.
- Before about 100 customers, the channels that scale (search, paid) are slow or costly tests of an offer that is still changing. Conversations come first.
- Choose channels where buyers already are, not where the founder is comfortable. When the two are the same, better still.
- A channel with no signal is not ruled out for a new category: demand may not exist yet anywhere. Say so rather than calling the channel dead.
- One AI run and one week of Reddit are samples. They point the plan; the channel's own skill measures properly.
- Measure again with the same calls, competitors and queries at day 30, 60 and 90, or the comparison says nothing. The server keeps no state: the host keeps the plan and its KPIs, and scheduling the re-measure is [write-weekly-report](../write-weekly-report/SKILL.md)'s job.
- Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. The chosen skills state their own cost; add them up before running more than one. The baseline calls are the ones they start from, and a result this account already paid for is free while cached (7 days for most search data), so running the first one soon after the plan costs less.
- The deliverable is a plan. Running campaigns, posting, sending and spending stay with the user.

## Goal and KPI

One primary goal per plan. The goal sets the KPI, and the KPI decides which channels count:

| Goal | The KPI that decides | Leading signals the tools re-measure |
|---|---|---|
| Traffic | organic visits (the user's analytics) | clicks and impressions from Search Console when connected (`console_get_search_analytics`, free), estimated organic traffic (`seo_get_domain_overview`), ranks of target keywords (`seo_get_position`) |
| Signups | signups a week (the user's analytics) | the traffic signals, plus mentions and referring domains |
| Leads | qualified leads or meetings (the user's CRM) | size of the reachable market (`leads_search_companies`), posts about the problem (`linkedin_search_posts`) |
| Awareness | branded search (`seo_get_keyword_metrics` on the brand) | AI mention rate (`aeo_run_ai_answers`), mentions by others, referring domains |
| First customers | customers (the user's count) | conversations started a week, from the tally the plan sets up |

The tools see public signals, not conversion. Ask for the current number of the deciding KPI at intake, so day 90 has something to compare with.

## Related skills

- Which channels to be on, ranked with the evidence: [pick-channels](../pick-channels/SKILL.md).
- A request that names the channel goes to that channel's skill: [create-seo-plan](../create-seo-plan/SKILL.md), [audit-technical-seo](../audit-technical-seo/SKILL.md), [create-ai-search-plan](../create-ai-search-plan/SKILL.md), [create-link-building-plan](../create-link-building-plan/SKILL.md), [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md), [create-influencer-plan](../create-influencer-plan/SKILL.md), [create-outbound-plan](../create-outbound-plan/SKILL.md), [create-reddit-plan](../create-reddit-plan/SKILL.md), [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-youtube-plan](../create-youtube-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md).
- A launch with a date: [create-launch-plan](../create-launch-plan/SKILL.md).
- Who the competitors are and how to beat them: [find-competitors](../find-competitors/SKILL.md) and [create-competitor-plan](../create-competitor-plan/SKILL.md). Who the customers are and what they need: [build-personas](../build-personas/SKILL.md) and [find-pain-points](../find-pain-points/SKILL.md).
- Content ideas and a calendar for the chosen channels: [find-content-ideas](../find-content-ideas/SKILL.md) and [create-content-calendar](../create-content-calendar/SKILL.md).
- Tracking the plan's KPIs every week: [write-weekly-report](../write-weekly-report/SKILL.md).
- A plan for an agency's client: [onboard-client](../onboard-client/SKILL.md).
- The product, the ICP and the goal written down once for every skill: [create-product-context](../create-product-context/SKILL.md).
