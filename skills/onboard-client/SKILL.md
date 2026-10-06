---
name: onboard-client
description: When an agency, freelancer or consultant has just signed a client and needs the starting point. Fixes the tracked set (domain, three competitors, 10 to 30 keywords, 3 to 10 AI prompts, one market), measures the baseline once on the channels in the retainer (12-month organic traffic, rankings, links, AI answer mentions, site health, ads, social reach), hands the record to the host to keep, and opens the strategy skill that fits the client's goal for a first 30-60-90 plan. Also use when the user mentions onboard a new client, baseline for a new retainer, kickoff benchmarks, where does our new client stand today, or a client's first 90 days. Winning the client first goes to prepare-client-pitch, the monthly report against this baseline to write-client-report, weekly tracking between reports to write-weekly-report.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Client onboarding

The first week with a client sets two things: the numbers every later report is measured against, and the first plan. This skill fixes the measurement set, measures it once, hands the record to the host to keep, and opens the strategy skill that fits the client's goal.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_domain_overview` and `aeo_run_ai_answers` (hosts often add a prefix, for example `mcp__manifold__seo_get_domain_overview`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The baseline reads the `seo_*`, `aeo_*` and `ads_*` tools, and the platform profile tools for social reach. If the `seo_*` tools are there and one of the others is not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and mark those rows "not measured" rather than dropping them: a client should see what the baseline did not cover.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the agency's services and its own voice for client-facing documents; for the client, their own context file if the work runs in their project: the domain, what they sell, the competitors, the keywords and prompts in their tracking set) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Client**: the domain, and what they sell to whom.
- **Goal**: the one outcome the retainer is judged on: organic traffic, AI answers, links, leads, paid, or social. Default: organic traffic.
- **Services**: what the agency does for them. Only those channels are measured.
- **Competitors**: three. Default: the ones from the pitch, or the top three real businesses from `seo_get_serp_competitors` (10 credits).
- **Tracked keywords**: 10 to 30 the client cares about. Default: 20 from `seo_get_ranked_keywords` on the site (10 credits): the top 5 by traffic, plus commercial keywords ranking 4 to 30.
- **Tracked prompts**: 3 to 10 buyer questions for AI answers. Default: 5 written with the client. If they have none, the prompt research in [check-ai-visibility](../check-ai-visibility/SKILL.md) finds them.
- **Market**: one `location`, `language` and device, kept for every report.
- **Budget**: a default onboarding costs about 10 + 10 + 61 + 3 x 5 + 10 + 20 x 6 + 5 x 18 + 15 + 2 + 4 = 337 credits (the first two are the default competitors and keywords), plus the strategy skill in step 4, which states its own. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Fix the measurement set.** Restate the inputs as the tracked set: domain, competitors, keywords, prompts, locale, service lines. Every monthly report re-measures exactly this set with the same calls and params.
2. **Measure the baseline.** Only for the channels in the retainer:
   - Search: `seo_get_domain_overview` with `history: true` on the client (61 credits) for the 12 months before the engagement, and without history on each competitor (5 credits each). `seo_get_backlink_summary` on the client (10 credits) for `referring_domains` and `domain_rank`.
   - Rankings: `seo_get_position` for each tracked keyword (6 credits each): `rank` and `url`. The method for one keyword is in [track-rankings](../track-rankings/SKILL.md).
   - AI answers: `aeo_run_ai_answers` with the tracked prompts and `brands` set to the client and the competitors (18 credits a prompt), then `get_task`. Record, per brand, the cells where it is mentioned and where it is cited. Add `aeo_get_site_readiness` (free) for AI crawler access.
   - Site health: `seo_run_technical_crawl` with `max_pages: 500` (15 credits), then `get_task`: `onpage_score`, `broken_links`, `non_indexable`.
   - Paid: `ads_get_advertiser_ads` with `platform: "facebook"` and `platform: "google"` for the client (1 credit each): running ads (`active: true` on Meta, `last_shown` within 7 days on Google) and their formats.
   - Social: the profile tool for each platform the agency runs (`instagram_get_profile`, `tiktok_get_profile`, `linkedin_get_company`, `youtube_get_channel`; 1 credit each): `followers` and `posts_count`.
3. **Hand the baseline to the host.** One record: the date, the tracked set, and every number with the tool and params that produced it. Ask the host to keep it where the next report can read it (a file in the client's folder, a sheet, the project's memory). If the host cannot keep files, give the user the record as a table to store and paste back. Task results expire after 30 days, so the record keeps the mentions and citations themselves, not the `task_id`.
4. **Open the first plan.** Pick the strategy skill by the goal, and run it with the intake already settled and the baseline already bought (any call it repeats with the same params within a week is cached and free):
   - Organic traffic: [create-seo-plan](../create-seo-plan/SKILL.md).
   - AI answers: [create-ai-search-plan](../create-ai-search-plan/SKILL.md).
   - Links and authority: [create-link-building-plan](../create-link-building-plan/SKILL.md).
   - Paid: [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
   - Leads and outbound: [create-outbound-plan](../create-outbound-plan/SKILL.md).
   - Social: the platform's own plan, [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-youtube-plan](../create-youtube-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md), [create-facebook-plan](../create-facebook-plan/SKILL.md) or [create-reddit-plan](../create-reddit-plan/SKILL.md).
   - Several goals, or a client who does not know: [create-growth-plan](../create-growth-plan/SKILL.md).
5. **Deliver** one onboarding document: the tracked set, a baseline table (metric, client value, each competitor's value, tool, date), the 12-month traffic line, the 30-60-90 plan from the strategy skill, and the date of the first monthly report. If the client wants weekly tracking between reports, [write-weekly-report](../write-weekly-report/SKILL.md) and [track-rankings](../track-rankings/SKILL.md) set it up.

## Judgment

- Choose tracked keywords the client can win within the retainer: a few head terms for the story, and more that rank 4 to 30, where movement shows within months. Twenty keywords on page five show no progress for a year.
- Add, never swap. A keyword or prompt the client adds later joins as a new row with its own start month. A number measured another way is not a change; it is a different number.
- Record the locale and device with every number. A report run from another location compares nothing.
- Keep the 12 months before the engagement. They show the client's seasonality; without them, the first seasonal dip reads as the agency's fault.
- Every number names its tool and its date (`meta.data_as_of` where the response gives it). Traffic from `seo_get_domain_overview` is "estimated organic traffic", never the client's number. If the client shares analytics or Search Console, record their own numbers beside the estimates; the [Google search notes](../create-seo-plan/references/platforms/google.md#search-console) say how to read Search Console where it is switched on.
- AI answers are live and change between runs: record the mention rate over prompts times engines. An ad library shows which ads run and since when, not what they cost.
- Set expectations in the document: rankings move over months, AI answers change from run to run, and new links reach the backlink index weeks late.
- Do not measure channels the agency does not run. A baseline full of numbers nobody owns turns into a report of excuses.
- If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. A pitch run the week before is cached: only the calls that differ are paid again. AI answers are never cached.
- Never send the document. The host's document or email tools take it; offer to pass it on.

## Related skills

- The pitch that won the client, whose set this reuses: [prepare-client-pitch](../prepare-client-pitch/SKILL.md).
- The monthly report against this baseline: [write-client-report](../write-client-report/SKILL.md).
- Weekly tracking and alerts between reports: [write-weekly-report](../write-weekly-report/SKILL.md).
- The full technical work behind a baseline finding: [audit-technical-seo](../audit-technical-seo/SKILL.md).
- A client's product launch: [create-launch-plan](../create-launch-plan/SKILL.md). A client's social accounts: [audit-tiktok-account](../audit-tiktok-account/SKILL.md), [audit-instagram-account](../audit-instagram-account/SKILL.md), [audit-youtube-channel](../audit-youtube-channel/SKILL.md), [audit-facebook-page](../audit-facebook-page/SKILL.md), [audit-linkedin-page](../audit-linkedin-page/SKILL.md) or [audit-x-account](../audit-x-account/SKILL.md).
