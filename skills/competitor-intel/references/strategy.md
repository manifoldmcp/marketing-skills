# A competitive plan

When the user wants a plan to win against named competitors rather than a report on them: where each rival is strong, where it is exposed, and which work turns that into share for the hours and money the team has, over 90 days. Checked October 2026.

## Settle first

The goal (win deals against a named rival, take search or AI share, defend against a new entrant, stand out in a crowded category), the stage, the ICP, the budget in credits and money, the team, and two or three competitors. Default: take share from the two strongest direct competitors, early stage, one founder who also sells, no paid media. Restate the inputs with the defaults marked.

## Baseline (about 71 credits for the user and three rivals)

| Measure | Call | Credits |
|---|---|---|
| Search strength | `seo_get_domain_overview` on each | 5 each |
| Paid activity | `ads_get_advertiser_ads` on facebook and google for each; on Google keep rows whose `last_shown` is in the last 30 days | 1 a page |
| AI shortlist | `aeo_run_ai_answers`, two category prompts, `brands` holding all four | 36 |
| Company size | `leads_get_company` and `linkedin_get_company` on each | 2 each |

## Gaps (about 78 more)

- **Search:** `seo_get_keyword_gap` with the user as `target` and each rival as `competitor` (10 each). Keep commercial and transactional keywords: where each rival meets buyers the user never sees.
- **Message:** homepage headings for all of them (free): the claims everyone makes and the ones nobody makes ([messaging](messaging.md)).
- **Switching:** "<rival> alternative" on Reddit (1 each), past year: where a rival is exposed ([complaints](complaints.md)).
- **AI ground:** `aeo_search_prompts` on the strongest rival's domain (45 for 50 rows). With a domain as input, `mentions_brand` means the domain is cited; prompts with volume where it is false are where engines answer without citing that rival. `ai_search_volume` is a proxy, not query logs.
- **Absence:** every channel in the baseline where a rival shows nothing (no active ads, no AI mentions, no pages for a topic) is ground the user can take without a fight.

## Choosing the ground

- **One main competitor per plan.** A plan against five rivals is a plan against none.
- **Attack where it is weak and the user's buyers are**, never where it is strongest. A small team should not fight a paid war with a larger rival; headcount and active ads show what it can outspend, and funding the tools cannot see, so ask.
- **For an early company, positioning is usually the core:** a segment or a claim no rival owns ([voice-of-customer](../../voice-of-customer/SKILL.md)).
- **The AI shortlist is now part of the battlefield.** The vendor ranked first before contact wins most deals (6sense, July 2026), and engines build their lists from list pages, including rivals' own "alternatives" pages. Where rivals own the answers, the work is in [ai-search](../../ai-search/SKILL.md).
- **Comparison and alternatives keywords** with volume and weak pages go to [seo](../../seo/SKILL.md); a paid push gets its creative research from [paid-ads](../../paid-ads/SKILL.md).

## The 30-60-90 days

| Period | Typical work | KPI the tools can re-measure |
|---|---|---|
| Days 1 to 30 | Positioning chosen and on the homepage; a [battlecard](battlecards.md) for sales; the weekly [watch](monitoring.md) set up | Homepage headings changed; card in use |
| Days 31 to 60 | Attack the main rival's weakest channel: the gap keywords with weak pages, the platform it ignores, the prompts where engines skip it, the switching threads | Rank on the gap keywords (`seo_get_position`, 6 each); pages live |
| Days 61 to 90 | Re-measure against the baseline on the same prompts, engines, keywords and location | AI mentions on the same prompts; organic traffic and keywords against each rival; rivals' active ads |

Give each period an owner and three first actions for this week.

## Honest targets

- Rank moves take months; AI mentions move faster but are noisy. Compare shares, never single cells, and set targets larger than the noise ([ai-search](../../ai-search/SKILL.md) gives the margins).
- Traffic estimates move with the provider's model: the 1 November 2026 change breaks any baseline taken before it. Re-take the baseline after that date before setting a traffic KPI.
- Put every rival in `brands` on the first AI run and keep that result: it is never cached and the day-90 comparison needs it. The host keeps the baseline table; the server keeps nothing.
