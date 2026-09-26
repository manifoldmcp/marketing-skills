# Growth plan

The answer to "where do I start". A growth plan settles one goal, takes a cheap reading of every channel the tools can see, finds where buyers and competitors already are and the user is not, and picks two or three channels. The work in each channel is another group's playbook; this plan chooses them, orders them in a 30-60-90 plan and sets the KPIs.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: one of traffic, signups, leads, awareness, first customers. Default: signups. The KPI for each is in the [router](../SKILL.md#goal-first); ask for its current number.
- **Stage**: pre-launch, fewer than 100 customers, or growing. Default: fewer than 100 customers.
- **ICP**: who buys, B2B or B2C, and the problem in their words.
- **Budget**: credits for the research (this playbook costs about 80), money for ads or creators, and hours a week. Default: 1,000 credits, no ad budget, 5 hours a week.
- **Team**: who does the work: writing, video, sales calls, design. Default: one founder who can write.
- **Competitors**: two or three. Default: the top real businesses from `seo_get_serp_competitors` (10 credits, only when the user names none), or the brands the AI answers name in step 2.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 80 credits. One cheap reading per channel:
   - Search: `seo_get_domain_overview` on the site and two competitors (5 credits each), and `seo_search_keywords` with the category as `seed` (10 credits) for demand.
   - AI answers: `aeo_run_ai_answers` with two buyer questions and `brands` set to the user and the competitors (18 credits a prompt), then `get_task`.
   - Communities: `reddit_search_subreddits` with the problem (1 credit), then `reddit_get_new_posts` on the top five with `since: "7d"` and `match` set to the problem words (5 credits): every post about the problem there in the last week.
   - Social: `tiktok_search_videos`, `youtube_search_videos` and `linkedin_search_posts` with the category and `since: "month"` (1 credit each).
   - Paid: `ads_get_advertiser_ads` with `platform: "facebook"` on each competitor (1 credit each).
   - B2B only: `leads_search_companies` with the ICP's industry, headcount and location (10 credits); `meta.rows_available` sizes the market.
3. **Gaps.** For each channel, set the user against the competitors and against the [channel signals](../SKILL.md#channel-signals): where buyers are active and competitors show up but the user does not. Rank the gaps by fit to the goal and by what the team can do.
4. **Tactics.** Choose two or three, and say why each fits the goal and the gaps:
   - [First 100 customers](first-100-customers.md) when the stage is pre-launch or under 100 customers, whatever the goal: channels that scale come after.
   - [Go-to-market](go-to-market.md) when the product is new or the ICP is not settled; a launch date adds the [launch plan](../../launch/references/launch-plan.md).
   - [Brand awareness](brand-awareness.md) when the goal is awareness, or the user's branded search is flat while the competitors' grows.
   - [Market entry](market-entry.md) when the growth is in a new country or segment.
   - Traffic: the [SEO strategy](../../seo/references/strategy.md) when search demand clears the signal; [quick wins](../../seo/references/quick-wins.md) first when the site already ranks 4 to 20 for keywords with volume.
   - AI answers: the [AI search strategy](../../ai-search/references/strategy.md) when the engines name competitors and not the user.
   - Communities: the [Reddit strategy](../../reddit/references/strategy.md) when the problem comes up every week and threads ask for a tool like this.
   - Leads, B2B: the [leads strategy](../../leads/references/strategy.md) for outbound, and the [LinkedIn strategy](../../linkedin/references/strategy.md) for founder-led posts.
   - Short video: the [TikTok](../../tiktok/references/strategy.md), [Instagram](../../instagram/references/strategy.md) or [YouTube](../../youtube/references/strategy.md) strategy, or the [influencers strategy](../../influencers/references/strategy.md) when the team cannot make video itself.
   - Paid: the [paid ads strategy](../../paid-ads/references/strategy.md) when competitors' ads run long and there is a budget.
   - Links and press: the [link building strategy](../../link-building/references/strategy.md) when search is chosen and the site's domain rank trails the competitors'.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week. By day 30, the first playbook of each chosen channel has run and its first actions are done; by day 60, the channel with the best early signal gets most of the hours; by day 90, the KPIs are measured again and the weakest channel is dropped or kept on the evidence.
   - KPIs the tools can measure again later: estimated organic traffic (`seo_get_domain_overview`), ranks for target keywords (`seo_get_position`, 6 credits each), AI mention rate on the same prompts (`aeo_run_ai_answers`), branded search (`seo_get_keyword_metrics` on the brand, `trend[12]`), and weekly posts about the problem in the same subreddits (`reddit_get_new_posts`). Signups, leads and revenue come from the user's analytics and CRM: the plan names the deciding KPI from the [router](../SKILL.md#goal-first) and uses the tool KPIs as leading signals.
   - **Deliver** one document: the inputs with defaults marked, a baseline table (channel, the user, each competitor, the signal), the gaps in a line each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and the three first actions.

## Judgment

- Two or three channels for a small team, never all of them. The plan's value is in what it leaves out, with the reason.
- Before about 100 customers, the channels that scale (search, paid) are slow or costly tests of an offer that is still changing. Conversations come first.
- Choose channels where buyers already are, not where the founder is comfortable. When the two are the same, better still.
- A channel with no signal is not ruled out for a new category: demand may not exist yet anywhere. Say so rather than calling the channel dead.
- One AI run and one week of Reddit are samples. They point the plan; the channel playbook measures properly.
- Measure again with the same calls, competitors and queries at day 30, 60 and 90, or the comparison says nothing.
