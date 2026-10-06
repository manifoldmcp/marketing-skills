---
name: create-competitor-plan
description: When the user wants a plan to win against named competitors. Baselines the user and two or three rivals on search, active ads, AI answers and headcount, finds the gaps (keyword gaps, open claims, switching threads on Reddit, AI prompts where engines skip a rival), picks the competitor skills that fit, and ends in a 30-60-90 day plan with KPIs the tools can measure again. Also use when the user mentions competitive strategy, how do we beat X, a plan to take share from X, a funded rival just launched, or a 90-day plan against the incumbents. Who the competitors are goes to find-competitors, one rival profiled to tear-down-competitor, a sales card to write-battlecard, and a growth plan with no rival in focus to create-growth-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Competitive strategy

A competitive strategy answers three questions before anyone rewrites a page or buys an ad: where each competitor is strong, where it is exposed, and which competitor skills turn that into share for the hours and money the team has. It ends in a 90-day plan, not a report on the competitors.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_domain_overview`, `ads_get_advertiser_ads` and `aeo_run_ai_answers` (hosts often add a prefix, for example `mcp__manifold__seo_get_domain_overview`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- This skill reads several tool groups: `seo_*`, `aeo_*`, `ads_*`, `leads_*`, `linkedin_*` and `reddit_*`. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run without them, and mark their columns "not checked" rather than leaving them empty, so nobody reads a gap as a zero.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the goal, the stage, the ICP, the competitors and their domains, the differentiation and the team) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: win more deals against a named rival, take search or AI share, defend against a new entrant, or stand out in a crowded category. Default: take share from the two strongest direct competitors.
- **Stage**: pre-launch, early (under about 100 customers) or growing. Default: early.
- **ICP**: who buys. Default: the buyer the homepage `h1` and meta description name.
- **Budget**: credits for the research (this skill costs about 149) and money for ads or content. Default: 1,000 credits and no paid media.
- **Team**: who does marketing and sales, and whether anyone writes, designs or runs ads. Default: one founder who also sells.
- **Competitors**: two or three. Default: the direct competitors from [find-competitors](../find-competitors/SKILL.md) if it has run; otherwise the top real businesses from `seo_get_serp_competitors`, filtered per the [real competitors](#real-competitors) rule.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 71 credits for the site and three competitors.
   - Search: `seo_get_domain_overview` on the site and each competitor (5 credits each): `domain_rank`, `organic_traffic`, `organic_keywords`.
   - Paid: `ads_get_advertiser_ads` with `active_only: true` on `facebook` and `google` for each (1 credit a page): active ads, and the oldest still running. Only Facebook applies `active_only`; Google rows carry `active: null`, so keep those whose `last_shown` falls in the last 30 days.
   - AI answers: `aeo_run_ai_answers` with two category prompts and `brands` holding the site and the competitors, on the default engines (18 credits per prompt, 36); `get_task` (free) after `poll_after_s`. Count the cells that mention each brand.
   - Company: `leads_get_company` on each competitor (1 credit each) for `employees` and `founded_year`, and `linkedin_get_company` on all four (1 credit each) for LinkedIn's own `employees` and `followers`. The two headcounts often disagree; report both.
3. **Gaps.** About 78 credits more.
   - Search: `seo_get_keyword_gap` with the site as `target` and each competitor as `competitor` (10 credits each). Keep the commercial and transactional keywords: the searches where each rival meets buyers the site never sees.
   - Message: `seo_get_page` on each homepage (free). List the claims all of them make, and the ones none of them make.
   - Switching: `reddit_search_posts` for "<competitor> alternative" (1 credit each), with the default relevance sort per the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching), keeping the threads from the past year by `created_at`. Threads asking for a way out of a competitor show where it is exposed; the reasons in depth are in [find-competitor-complaints](../find-competitor-complaints/SKILL.md).
   - Prompts: `aeo_search_prompts` with the strongest competitor's domain as `domain` (45 credits for 50 rows), with `ai_search_volume` per prompt. Rows with `mentions_brand` true are its AI ground; prompts with volume where it is false are where engines skip it. Add the best two to the day-90 re-measure.
   - Absence: every channel in the baseline where a competitor shows zero (no active ads, no AI mentions, no ranking pages for a topic) is ground the site can take without a fight.
4. **Tactics.** Choose two or three and say why each fits the numbers:
   - [find-competitors](../find-competitors/SKILL.md) when the user is unsure of the field, or the AI answers in the baseline named brands nobody listed.
   - [tear-down-competitor](../tear-down-competitor/SKILL.md) on the one competitor that wins the most deals or grew fastest, for its full channel picture.
   - [write-battlecard](../write-battlecard/SKILL.md) when sales loses deals to one named competitor; it is the day-30 sales deliverable.
   - [compare-messaging](../compare-messaging/SKILL.md) when the claims converge (everyone says the same thing) or price is the objection the user hears.
   - [find-positioning](../find-positioning/SKILL.md) when the gaps show a segment or a claim no competitor owns. For an early company this is usually the core of the plan.
   - To act on the gaps: [plan-comparison-pages](../plan-comparison-pages/SKILL.md) for "X vs Y" and "X alternatives" keywords, [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md) before any paid push, and [create-ai-search-plan](../create-ai-search-plan/SKILL.md) when the rivals own the AI answers.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week. By day 30, the chosen positioning on the homepage and a battlecard for sales; by day 60, an attack on the strongest competitor's weakest channel (the gap keywords with weak pages, the platform it ignores, the prompts where engines skip it); by day 90, a re-measure against the baseline.
   - KPIs the tools can measure again later: AI mentions on the same prompts and engines (`aeo_run_ai_answers`), the site's rank on the gap keywords (`seo_get_position`, 6 credits each), `organic_traffic` and `organic_keywords` against each competitor (`seo_get_domain_overview`), and the competitors' active ads (`ads_get_advertiser_ads`).
6. **Deliver** one document: the inputs with defaults marked, a baseline table (site against each competitor: domain rank, organic traffic, active ads, AI mentions out of the cells run, employees from both sources), the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions.

## Judgment

- Do not attack a competitor where it is strongest. Pick the ground where it is weak and the user's buyers are.
- Headcount and active ads show what a rival can outspend; the tools do not see funding, so ask the user when it matters. A small team should not fight a paid war with a bigger competitor; it should take the segments and channels the rival ignores.
- One main competitor per plan. A plan against five rivals is a plan against none.
- Rank moves take months; AI mentions move faster but are noisy. Re-measure on the same prompts, engines and keywords, and compare shares, not single cells.
- Do not copy the leader's messaging. Sounding like the leader makes the user the cheaper copy.
- `organic_traffic` is an estimate modelled from rankings: good for comparing sites, not a visit count. Every claim about a competitor carries its source; mark an inference as an inference.
- Credits: say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. `aeo_run_ai_answers` is never cached, so put every competitor in `brands[]` on the first run and keep that result for the day-90 comparison.
- The server keeps no state. The host keeps the baseline table for the day-90 re-measure; a weekly watch of the same competitors is [monitor-competitors](../monitor-competitors/SKILL.md).
- Never contact anyone. After delivering, offer to write the findings into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Real competitors

- A business competitor sells to the same buyer for the same job. A SERP competitor only shares keywords. Many of the top rows of `seo_get_serp_competitors` are publishers, marketplaces, review sites and directories. Never count a domain from it as a business competitor until its homepage title or its company description (`seo_get_page`, `leads_get_company`) shows it sells the same thing.
- Compare the user with two or three competitors, not ten.

## Related skills

- The tactics in step 4: [find-competitors](../find-competitors/SKILL.md), [tear-down-competitor](../tear-down-competitor/SKILL.md), [write-battlecard](../write-battlecard/SKILL.md), [compare-messaging](../compare-messaging/SKILL.md) and [find-positioning](../find-positioning/SKILL.md).
- A plan by goal across every channel, with no rival in focus: [create-growth-plan](../create-growth-plan/SKILL.md).
- The same competitors watched every week: [monitor-competitors](../monitor-competitors/SKILL.md).
- Buyers, their pain points and the market by segment: [find-pain-points](../find-pain-points/SKILL.md) and [map-market](../map-market/SKILL.md).
