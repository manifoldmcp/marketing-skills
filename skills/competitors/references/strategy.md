# Competitive strategy

A competitive strategy answers three questions before anyone rewrites a page or buys an ad: where each competitor is strong, where it is exposed, and which of this group's playbooks turns that into share for the hours and money the team has. It ends in a 90-day plan, not a report on the competitors.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: win more deals against a named rival, take search or AI share, defend against a new entrant, or stand out in a crowded category. Default: take share from the two strongest direct competitors.
- **Stage**: pre-launch, early (under about 100 customers) or growing. Default: early.
- **ICP**: who buys. Default: the buyer the homepage `h1` and meta description name.
- **Budget**: credits for the research (this playbook costs about 131) and money for ads or content. Default: 1,000 credits and no paid media.
- **Team**: who does marketing and sales, and whether anyone writes, designs or runs ads. Default: one founder who also sells.
- **Competitors**: two or three. Default: the direct competitors from [find competitors](find-competitors.md) if it has run; otherwise the top real businesses from `seo_get_serp_competitors`, filtered per the [router](../SKILL.md#real-competitors).
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 98 credits for the site and three competitors.
   - Search: `seo_get_domain_overview` on the site and each competitor (5 credits each): `domain_rank`, `organic_traffic`, `organic_keywords`.
   - Paid: `ads_get_advertiser_ads` with `active_only: true` on `facebook` and `google` for each (1 credit a page): active ads, and the oldest still running. Only Facebook applies `active_only`; on the other libraries keep the rows whose `active` is true or whose `last_shown` is recent.
   - AI answers: `aeo_run_ai_answers` with two category prompts and `brands` holding the site and the competitors, on the default engines (18 credits per prompt, 36); `get_task` (free) after `poll_after_s`. Count the cells that mention each brand.
   - Company: `leads_get_company` on each competitor (10 credits each) for `employees`, `total_funding` and `funding_stage`, and `linkedin_get_company` on all four (1 credit each) for `followers`.
3. **Gaps.** About 33 credits more.
   - Search: `seo_get_keyword_gap` with the site as `target` and each competitor as `competitor` (10 credits each). Keep the commercial and transactional keywords: the searches where each rival meets buyers the site never sees.
   - Message: `seo_get_page` on each homepage (free). List the claims all of them make, and the ones none of them make.
   - Switching: `reddit_search_posts` for "<competitor> alternative" (1 credit each), with the default relevance sort per the [router](../SKILL.md#evidence), keeping the threads from the past year by `created_at`. Threads asking for a way out of a competitor show where it is exposed; the reasons in depth are in [reddit competitor complaints](../../reddit/references/competitor-complaints.md).
   - Absence: every channel in the baseline where a competitor shows zero (no active ads, no AI mentions, no ranking pages for a topic) is ground the site can take without a fight.
4. **Tactics.** Choose two or three from this group and say why each fits the numbers:
   - [Find competitors](find-competitors.md) when the user is unsure of the field, or the AI answers in the baseline named brands nobody listed.
   - [Teardown](teardown.md) on the one competitor that wins the most deals or grew fastest, for its full channel picture.
   - [Messaging and pricing](messaging-pricing.md) when the claims converge (everyone says the same thing) or price is the objection the user hears.
   - [Positioning](positioning.md) when the gaps show a segment or a claim no competitor owns. For an early company this is usually the core of the plan.
   - To act on the gaps, the other groups take over: [seo comparison pages](../../seo/references/comparison-pages.md) for "X vs Y" and "X alternatives" keywords, [paid-ads competitor ads](../../paid-ads/references/competitor-ads.md) before any paid push, and [ai-search](../../ai-search/SKILL.md) when the rivals own the AI answers.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week. By day 30, the chosen positioning on the homepage and a battlecard for sales; by day 60, an attack on the strongest competitor's weakest channel (the gap keywords with weak pages, the platform it ignores, the prompts where engines skip it); by day 90, a re-measure against the baseline.
   - KPIs the tools can measure again later: AI mentions on the same prompts and engines (`aeo_run_ai_answers`), the site's rank on the gap keywords (`seo_get_position`, 6 credits each), `organic_traffic` and `organic_keywords` against each competitor (`seo_get_domain_overview`), and the competitors' active ads (`ads_get_advertiser_ads`).
   - **Deliver** one document: the inputs with defaults marked, a baseline table (site against each competitor: domain rank, organic traffic, active ads, AI mentions out of the cells run, employees, funding), the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and the three first actions.

## Judgment

- Do not attack a competitor where it is strongest. Pick the ground where it is weak and the user's buyers are.
- Funding and headcount show what a rival can outspend. A small team should not fight a paid war with a funded competitor; it should take the segments and channels the rival ignores.
- One main competitor per plan. A plan against five rivals is a plan against none.
- Rank moves take months; AI mentions move faster but are noisy. Re-measure on the same prompts, engines and keywords, and compare shares, not single cells.
- Do not copy the leader's messaging. Sounding like the leader makes the user the cheaper copy.
- The server keeps no state. The host keeps the baseline table for the day-90 re-measure; a weekly watch of the same competitors is [monitoring competitor watch](../../monitoring/references/competitor-watch.md).
