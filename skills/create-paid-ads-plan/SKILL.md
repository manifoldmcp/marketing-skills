---
name: create-paid-ads-plan
description: When the user wants a plan for paid ads. Baselines where the user and two or three competitors advertise in the Meta, TikTok, LinkedIn and Google ad libraries, who buys search clicks and what a click costs in the category, reads the long-running ads for the angles everyone uses and the ones nobody does, picks the platform and the first tests, and ends in a 30-60-90 day test plan. Also use when the user mentions a paid ads strategy, should we run Meta or Google ads, where to spend an ad budget, plan our first ad spend, a paid social plan, paid acquisition, how to start with PPC, or whether LinkedIn ads are worth it. A keyword list goes to find-google-ads-keywords, one competitor's ads or the category's to research-meta-ads, research-tiktok-ads, research-linkedin-ads or research-google-ads, a brief for the ads to write-ad-brief, and no channel chosen yet to create-growth-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Paid ads strategy

A paid ads strategy answers three questions before any money is spent: which platform the buyers can be reached on for the budget, what competitors already pay to keep running there, and which angles to test first. It ends in a 90-day test plan, not a campaign build. The libraries show what advertisers pay to keep running, never how it performs; the [ad libraries notes](references/ad-libraries.md) say how every paid ads skill reads them.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `ads_get_advertiser_ads` and `seo_get_traffic_estimates` (hosts often add a prefix, for example `mcp__manifold__ads_get_advertiser_ads`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `ads_*` tools are not, the ads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, run the search steps on the `seo_*` tools, and mark the library columns "not checked". If the `seo_*` group is off, skip the paid search steps and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the goal, the stage, the ICP, the offer, the competitors and their advertiser names and domains, the team and the budget) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: sales, signups, leads, app installs or awareness, and the cost per result the business can afford. Default: signups or leads. With no ceiling given, a rule of thumb: allowable cost per customer = first-year revenue per customer × gross margin ÷ 2 (about a six-month payback); the user confirms it after the first 30 days.
- **Stage**: pre-launch, early or established, and whether the user has run ads before and what happened. Default: early, no ads run yet.
- **ICP**: who buys, and whether they search for a solution or have to be shown one. B2B with a named job title points to LinkedIn and Google; a visual consumer product points to Meta and TikTok.
- **Budget**: monthly ad spend, and credits for the research (this skill costs about 90). Default: $3,000 a month and 500 credits.
- **Team**: who makes the creative (the founder with a phone, a designer, a video editor, UGC creators) and who runs the ad accounts. Default: the founder does both.
- **Competitors**: two or three. Default: run the category sweep from step 3 first (`ads_search_ads` on Meta, 2 pages) and take the advertisers that show up most.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 78 credits for the site and three competitors.
   - Where each brand advertises: for the user and each competitor, `ads_get_advertiser_ads` on `platform: "facebook"`, `"tiktok"` and `"linkedin"` (1 credit each for the first page), and on Google with the brand's domain (1 credit; the advertiser search in [the libraries](references/ad-libraries.md#the-libraries) when the domain finds nothing). That is 4 credits a brand, 16 for four. Per library, count the ads, the ones still running, and the ones that pass the [winner rules](references/ad-libraries.md#winners).
   - Paid search: `seo_get_traffic_estimates` with the four domains in one call (52 credits). `paid_traffic` shows who buys search clicks and roughly how many; `organic_traffic` sets the scale.
   - The price of a click: `seo_search_keywords` with the category as `seed` (10 credits). Read `cpc`, `competition` and `volume` on the commercial and transactional terms. Real volume on buying terms means demand exists to capture with search.
3. **Gaps.** About 13 credits more.
   - Platforms: the libraries where competitors keep long-running ads and the user has none are proven ground; a library where nobody in the category advertises is either empty ground or a poor fit.
   - Angles: `ads_get_ad` on the 10 longest-running competitor ads (10 credits), read as in [reading an ad](references/ad-libraries.md#reading-an-ad). Note the angles and offers every competitor uses (table stakes) and the ones none uses (the opening).
   - The real ad rivals: `ads_search_ads` with the category keyword on Meta (2 pages) and TikTok (1 page), 3 credits. Advertisers here that the user did not name compete for the same attention.
4. **Tactics.** Choose and say why each fits the numbers:
   - [find-google-ads-keywords](../find-google-ads-keywords/SKILL.md) when buying terms have volume and competitors show `paid_traffic`: capture demand that already exists before creating it.
   - [check-google-brand-bidding](../check-google-brand-bidding/SKILL.md) when the user has branded search volume: a competitor on the user's own name takes the cheapest, warmest clicks there are.
   - [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md) on the competitors when they keep ads running 30 days or more: the fastest read of what works in the category.
   - The same skills on the whole category when competitors run few ads, or the user sells into a crowded consumer category: the angles across the whole market.
   - [write-ad-brief](../write-ad-brief/SKILL.md) before the first social ad goes live, always: on Meta and TikTok the creative does most of the targeting.
   - Creator content for the ads, when the team cannot film: the UGC search in [find-creators](../find-creators/SKILL.md).
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - Platform choice: one platform while spend is under about $5,000 a month (a rule of thumb). Meta's delivery needs about 50 optimization events a week per ad set to leave its learning phase, and a budget split across three platforms gives none of them enough. Weekly budget ÷ allowable cost per result under 50 means optimizing for an earlier event (lead, trial start, add to cart), not the purchase. Search first when step 2 found demand; Meta or TikTok first when the product has to be shown; LinkedIn only when a deal is worth enough to carry clicks that cost several times Meta's.
   - By day 30: tracking in place, three angles with two formats each from the creative brief, live on one platform. By day 60: losers cut, the winning angle given new hooks, search added if PPC keywords found demand. By day 90: the winners scaled, and a second platform or retargeting only if the first pays back.
   - KPIs the tools can measure again later: the user's own ads in the libraries and how long each keeps running (`ads_get_advertiser_ads`), competitors' running and long-running ads, `paid_traffic` for the site (`seo_get_traffic_estimates`), and the category's `cpc` (`seo_get_keyword_metrics` on the kept keywords). CPA, ROAS and CTR come from the user's ad accounts, not from these tools; ask the user for them at each checkpoint.
6. **Deliver** one document: the inputs with defaults marked, a baseline table (brand against libraries: ads running, long-runners, `paid_traffic`), the category's click prices, the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and three first actions for this week.

## Judgment

- Creative moves results more than targeting on Meta and TikTok. A plan with one audience and many creatives beats one with many audiences and one creative.
- A competitor with hundreds of ads but none older than two weeks is testing, not winning. Weigh long-runners, not counts.
- `paid_traffic` is a modelled estimate of search ad clicks, not a spend figure. Use it to rank competitors, not to set a budget.
- A category with no advertisers in any library is a warning as often as an opening. Say which the evidence supports: search demand with no ads is an opening; no demand and no ads is a hard market to buy.
- Keep the plan to what the team can produce. Three new creatives a week is a real workload; a founder alone manages one platform.
- The libraries lag and miss some ads. A competitor's missing ads mean "not found", not "not running".
- Credits and handoff follow the [ad libraries notes](references/ad-libraries.md#credits): say the estimate first, pass `max_credits` on a budget, never buy, launch or pause ads, and ask the user for their own results, which live in their ad accounts. The host keeps the baseline for the checkpoints.

## Related skills

- The tactics in step 4: [find-google-ads-keywords](../find-google-ads-keywords/SKILL.md), [check-google-brand-bidding](../check-google-brand-bidding/SKILL.md), [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md) and [write-ad-brief](../write-ad-brief/SKILL.md).
- No channel chosen yet, or "more signups" with no channel: [create-growth-plan](../create-growth-plan/SKILL.md), and which channels to use at all: [pick-channels](../pick-channels/SKILL.md).
- Creators to make the ads or post for the brand: [create-influencer-plan](../create-influencer-plan/SKILL.md).
- Organic posts on the same platforms: [create-facebook-plan](../create-facebook-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md) or [create-youtube-plan](../create-youtube-plan/SKILL.md).
- Ads research for an agency's prospect: [prepare-client-pitch](../prepare-client-pitch/SKILL.md).
