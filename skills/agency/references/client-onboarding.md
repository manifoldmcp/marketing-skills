# Client onboarding

The first week with a client sets two things: the numbers every later report is measured against, and the first plan. This playbook fixes the measurement set, measures it once, hands the record to the host to keep, and opens the strategy playbook that fits the client's goal.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Client**: the domain, and what they sell to whom.
- **Goal**: the one outcome the retainer is judged on: organic traffic, AI answers, links, leads, paid, or social. Default: organic traffic.
- **Services**: what the agency does for them. Only those channels are measured.
- **Competitors**: three. Default: the ones from the pitch, or the top three real businesses from `seo_get_serp_competitors` (10 credits).
- **Tracked keywords**: 10 to 30 the client cares about. Default: 20 from `seo_get_ranked_keywords` on the site (10 credits): the top 5 by traffic, plus commercial keywords ranking 4 to 30.
- **Tracked prompts**: 3 to 10 buyer questions for AI answers. Default: 5 written with the client. If they have none, the prompt research in [visibility check](../../ai-search/references/visibility-check.md) finds them.
- **Market**: one `location`, `language` and device, kept for every report.
- **Budget**: a default onboarding costs about 10 + 61 + 3 x 5 + 10 + 20 x 6 + 5 x 18 + 15 + 2 + 4 = 327 credits, plus the strategy playbook in step 4, which states its own. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Fix the measurement set.** Restate the inputs as the tracked set in the [router](../SKILL.md#measurement-set): domain, competitors, keywords, prompts, locale, service lines. Every monthly report re-measures exactly this set.
2. **Measure the baseline.** Only for the channels in the retainer:
   - Search: `seo_get_domain_overview` with `history: true` on the client (61 credits) for the 12 months before the engagement, and without history on each competitor (5 credits each). `seo_get_backlink_summary` on the client (10 credits) for `referring_domains` and `domain_rank`.
   - Rankings: `seo_get_position` for each tracked keyword (6 credits each): `rank` and `url`. The method for one keyword is [rank check](../../seo/references/rank-check.md).
   - AI answers: `aeo_run_ai_answers` with the tracked prompts and `brands` set to the client and the competitors (18 credits a prompt), then `get_task`. Record, per brand, the cells where it is mentioned and where it is cited. Add `aeo_get_site_readiness` (free) for AI crawler access.
   - Site health: `seo_run_technical_crawl` with `max_pages: 500` (15 credits), then `get_task`: `onpage_score`, `broken_links`, `non_indexable`.
   - Paid: `ads_get_advertiser_ads` with `platform: "facebook"` and `platform: "google"` for the client (1 credit each): active ads and their formats.
   - Social: the profile tool for each platform the agency runs (`instagram_get_profile`, `tiktok_get_profile`, `linkedin_get_company`, `youtube_get_channel`; 1 credit each): `followers` and `posts_count`.
3. **Hand the baseline to the host.** One record: the date, the tracked set, and every number with the tool and params that produced it. Ask the host to keep it where the next report can read it (a file in the client's folder, a sheet, the project's memory). If the host cannot keep files, give the user the record as a table to store. Task results expire after 30 days, so the record keeps the mentions and citations themselves, not the `task_id`.
4. **Open the first plan.** Pick the strategy playbook by the goal, and run it with the intake already settled and the baseline already bought (any call it repeats with the same params within a week is cached and free):
   - Organic traffic: [SEO strategy](../../seo/references/strategy.md).
   - AI answers: [AI search strategy](../../ai-search/references/strategy.md).
   - Links and authority: [link building strategy](../../link-building/references/strategy.md).
   - Paid: [paid ads strategy](../../paid-ads/references/strategy.md).
   - Leads and outbound: [leads strategy](../../leads/references/strategy.md).
   - Social: the platform's own strategy, for example [LinkedIn](../../linkedin/references/strategy.md), [TikTok](../../tiktok/references/strategy.md), [Instagram](../../instagram/references/strategy.md), [YouTube](../../youtube/references/strategy.md) or [Reddit](../../reddit/references/strategy.md).
   - Several goals, or a client who does not know: [growth plan](../../growth-plan/references/strategy.md).
5. **Deliver** one onboarding document: the tracked set, a baseline table (metric, client value, each competitor's value, tool, date), the 12-month traffic line, the 30-60-90 plan from the strategy playbook, and the date of the first monthly report. If the client wants weekly tracking between reports, the [monitoring](../../monitoring/SKILL.md) group sets it up.

## Judgment

- Choose tracked keywords the client can win within the retainer: a few head terms for the story, and more that rank 4 to 30, where movement shows within months. Twenty keywords on page five show no progress for a year.
- Record the locale and device with every number. A report run from another location compares nothing.
- Keep the 12 months before the engagement. They show the client's seasonality; without them, the first seasonal dip reads as the agency's fault.
- The tools estimate. If the client shares analytics or Search Console, record their own numbers beside the estimates; the [seo](../../seo/SKILL.md) group reads Search Console where it is switched on.
- Set expectations in the document: rankings move over months, AI answers change from run to run, and new links reach the backlink index weeks late.
- Do not measure channels the agency does not run. A baseline full of numbers nobody owns turns into a report of excuses.
