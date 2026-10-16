# A paid ads plan

When the user wants a plan rather than a check: which platform the buyers can be reached on for the budget, what competitors already pay to keep running there, which angles to test first, and a 30-60-90 day test plan. Not a campaign build.

## Inputs

Ask in one message; use the default and say so for anything unanswered.

- **Goal** and the cost per result the business can afford. Default: signups or leads. With no ceiling given, allowable cost per customer = first-year revenue per customer x gross margin ÷ 2 (a rule of thumb, about a six-month payback); confirm it after 30 days.
- **Stage**, and whether ads have run before and what happened. Default: early, none yet.
- **Buyer**, and whether they search for a solution or have to be shown one.
- **Budget:** monthly ad spend, and credits for the research (about 90). Default: $3,000 a month.
- **Team:** who makes creative and who runs the accounts. Default: the founder does both.
- **Competitors:** two or three. Default: the advertisers that show up most in a Meta category sweep (`ads_search_ads`, two pages, `active_only: true`).

## Baseline (about 78 credits for the user and three competitors)

- **Where each brand advertises:** `ads_get_advertiser_ads` on Meta, TikTok (Europe only), LinkedIn and Google per brand, 1 credit each for the first page. Per library: ads running, long-runners ([winners](ad-libraries.md#winners-what-run-length-means)).
- **Who buys search clicks:** `seo_get_traffic_estimates` with the four domains in one call (52 credits). `paid_traffic` is a modelled estimate of search ad clicks, not spend: rank competitors with it, never set a budget from it.
- **What a click costs:** `seo_search_keywords` on the category (10 credits). Real volume on buying terms means demand to capture with search.
- **Gaps (about 13 more):** `ads_get_ad` on the ten longest-running competitor ads for the angles everyone uses and the ones nobody does; a category sweep on Meta and TikTok for ad rivals the user did not name.

## Choosing

- **Platform:** search first when the baseline found demand; Meta or TikTok first when the product must be shown; LinkedIn only when a deal carries clicks at several times Meta's price ([platforms](platforms.md#choosing-between-them)). One platform under about $5,000 a month.
- **Events:** weekly budget ÷ allowable cost per result under about 50 means optimising for an earlier event than the purchase on Meta.
- **The tactics that fit the numbers:** a keyword list when buying terms have volume ([Google search ads](google-search-ads.md)); a brand check when the user has branded volume ([brand bidding](brand-bidding.md)); competitor reads when they keep ads running 30 days or more ([competitor ads](competitor-ads.md)); always a brief before the first social ad ([ad briefs](ad-brief.md)); creators when the team cannot film ([creators](../../creators/SKILL.md)); ChatGPT or AI Mode only as a later test ([AI answer ads](ai-answer-ads.md)).
- A category with no advertisers in any library is a warning as often as an opening. Search demand with no ads is an opening; no demand and no ads is a hard market to buy.

## The 30-60-90 days

| Period | Typical work | KPI |
|---|---|---|
| Days 1 to 30 | Conversion tracking verified, three angles with two concepts each live on one platform; on search, the core ad groups on exact and phrase | Cost per result against the allowable, from the user's account |
| Days 31 to 60 | Losers cut, the winning angle given new concepts and hooks, search added if the keyword work found demand | Cost per result trend; the user's ads still running in the libraries |
| Days 61 to 90 | Winners scaled; a second platform, retargeting or AI Max only if the first pays back | Payback against the allowable; `paid_traffic` for the site |

Give each period an owner and three first actions for this week. Keep the plan to what the team can produce: three new concepts a week is a real workload, and a founder alone runs one platform.

## Measuring it

- CPA, ROAS and CTR live in the user's ad accounts, never in these tools. Ask for them at each checkpoint.
- What the tools can re-measure: the user's and competitors' ads and how long each keeps running (`ads_get_advertiser_ads`), `paid_traffic` (`seo_get_traffic_estimates`), and the category's `cpc` (`seo_get_keyword_metrics` on the kept keywords). The host stores the baseline; the server keeps no history.
- Platform-reported conversions overlap: Meta, Google and LinkedIn each claim the same sale. Judge the plan on the business's own count of customers and revenue against spend, and say which number each KPI comes from.

## Deliver

One document: the inputs with defaults marked, the baseline table (brand against libraries: ads running, long-runners, `paid_traffic`), the category's click prices with the bid range, the gaps in three lines each, the chosen tactics and why, the 30-60-90 table, and three first actions. Where paid fits among all channels belongs to [growth-strategy](../../growth-strategy/SKILL.md).
