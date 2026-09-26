# Paid ads strategy

A paid ads strategy answers three questions before any money is spent: which platform the buyers can be reached on for the budget, what competitors already pay to keep running there, and which angles to test first. It ends in a 90-day test plan, not a campaign build.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: sales, signups, leads, app installs or awareness, and the cost per result the business can afford. Default: signups or leads, at a cost per acquisition the user confirms after the first 30 days.
- **Stage**: pre-launch, early or established, and whether the user has run ads before and what happened. Default: early, no ads run yet.
- **ICP**: who buys, and whether they search for a solution or have to be shown one. B2B with a named job title points to LinkedIn and Google; a visual consumer product points to Meta and TikTok.
- **Budget**: monthly ad spend, and credits for the research (this playbook costs about 90). Default: $3,000 a month and 500 credits.
- **Team**: who makes the creative (the founder with a phone, a designer, a video editor, UGC creators) and who runs the ad accounts. Default: the founder does both.
- **Competitors**: two or three. Default: the advertisers that show up most in `ads_search_ads` on Meta for the category keyword.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 78 credits for the site and three competitors.
   - Where each brand advertises: for the user and each competitor, `ads_get_advertiser_ads` on `platform: "facebook"`, `"tiktok"` and `"linkedin"` (1 credit each for the first page), and on Google with the brand's domain (1 credit; the advertiser search in the [router](../SKILL.md#the-libraries) when the domain finds nothing). That is 4 credits a brand, 16 for four. Per library, count the ads, the ones still running, and the ones that pass the winner test in the [router](../SKILL.md#winners).
   - Paid search: `seo_get_traffic_estimates` with the four domains in one call (52 credits). `paid_traffic` shows who buys search clicks and roughly how many; `organic_traffic` sets the scale.
   - The price of a click: `seo_search_keywords` with the category as `seed` (10 credits). Read `cpc`, `competition` and `volume` on the commercial and transactional terms. Real volume on buying terms means demand exists to capture with search.
3. **Gaps.** About 13 credits more.
   - Platforms: the libraries where competitors keep long-running ads and the user has none are proven ground; a library where nobody in the category advertises is either empty ground or a poor fit.
   - Angles: `ads_get_ad` on the 10 longest-running competitor ads (10 credits), read as in the router's [reading an ad](../SKILL.md#reading-an-ad). Note the angles and offers every competitor uses (table stakes) and the ones none uses (the opening).
   - The real ad rivals: `ads_search_ads` with the category keyword on Meta (2 pages) and TikTok (1 page), 3 credits. Advertisers here that the user did not name compete for the same attention.
4. **Tactics.** Choose from this group, and say why each fits the numbers:
   - [PPC keywords](ppc-keywords.md) when buying terms have volume and competitors show `paid_traffic`: capture demand that already exists before creating it.
   - [Competitor ads](competitor-ads.md) when competitors keep ads running 30 days or more: the fastest read of what works in the category.
   - [Swipe file](swipe-file.md) when competitors run few ads, or the user sells into a crowded consumer category: the angles across the whole market.
   - [Creative brief](creative-brief.md) before the first social ad goes live, always: on Meta and TikTok the creative does most of the targeting.
   - Creator content for the ads, when the team cannot film: the `influencers` group's UGC creators playbook.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - Platform choice: one platform while spend is under about $5,000 a month. Meta's delivery needs about 50 conversions a week per ad set to leave its learning phase, and a budget split across three platforms gives none of them enough. Search first when step 2 found demand; Meta or TikTok first when the product has to be shown; LinkedIn only when a deal is worth enough to carry clicks that cost several times Meta's.
   - By day 30: tracking in place, three angles with two formats each from the creative brief, live on one platform. By day 60: losers cut, the winning angle given new hooks, search added if PPC keywords found demand. By day 90: the winners scaled, and a second platform or retargeting only if the first pays back.
   - KPIs the tools can measure again later: the user's own ads in the libraries and how long each keeps running (`ads_get_advertiser_ads`), competitors' running and long-running ads, `paid_traffic` for the site (`seo_get_traffic_estimates`), and the category's `cpc` (`seo_get_keyword_metrics` on the kept keywords). CPA, ROAS and CTR come from the user's ad accounts, not from these tools; ask the user for them at each checkpoint.
   - **Deliver** one document: the inputs with defaults marked, a baseline table (brand against libraries: ads running, long-runners, `paid_traffic`), the category's click prices, the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and three first actions for this week.

## Judgment

- Creative moves results more than targeting on Meta and TikTok. A plan with one audience and many creatives beats one with many audiences and one creative.
- A competitor with hundreds of ads but none older than two weeks is testing, not winning. Weigh long-runners, not counts.
- `paid_traffic` is a modelled estimate of search ad clicks, not a spend figure. Use it to rank competitors, not to set a budget.
- A category with no advertisers in any library is a warning as often as an opening. Say which the evidence supports: search demand with no ads is an opening; no demand and no ads is a hard market to buy.
- Keep the plan to what the team can produce. Three new creatives a week is a real workload; a founder alone manages one platform.
- The libraries lag and miss some ads. A competitor's missing ads mean "not found", not "not running".
