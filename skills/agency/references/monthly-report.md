# Monthly report

A monthly report answers three questions for the client: what moved, why, and what happens next month. It re-measures the tracked set from onboarding with the same calls and params, sets it against last month and the baseline, and hands the tables to the host to lay out and send.

## Inputs to settle first

- **Client and baseline**: the stored record from [client onboarding](client-onboarding.md) and the months since, from the host. Without a baseline there is nothing to compare: run onboarding now, and this month becomes the baseline.
- **Period**: the month reported. Default: the last full calendar month, measured now.
- **Work done**: what the agency shipped in the month (pages, fixes, links, campaigns). The tools see results, not work; the report needs both.
- **Channels**: the ones in the retainer. Measure nothing else.
- **Budget**: a default report costs about 4 x 5 + 20 x 6 + 10 + 12 + 5 x 18 + 2 + 4 = 258 credits per client per month, for 20 keywords, 5 prompts and 3 competitors. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Load the tracked set.** Read the domain, competitors, keywords, prompts and locale from the stored record, and use them exactly as stored: a changed `location`, device or prompt wording breaks the comparison.
2. **Re-measure.** The same calls as the baseline, with the same params:
   - Search: `seo_get_domain_overview` on the client and each competitor (5 credits each), without history; the stored months are the history.
   - Rankings: `seo_get_position` for each tracked keyword (6 credits each). Tracking them weekly between reports is [rank tracking](../../monitoring/references/rank-tracking.md).
   - Links: `seo_get_backlink_summary` on the client (10 credits), and `seo_get_referring_domains` on it (12 credits) for the rows with `first_seen` in the month (won) and with `lost: true` (lost). Count only won domains that clear the floors in the [link-building router](../../link-building/SKILL.md).
   - AI answers: `aeo_run_ai_answers` with the tracked prompts and brands (18 credits a prompt), then `get_task`. Tracking them weekly is [AI visibility tracking](../../monitoring/references/ai-visibility-tracking.md).
   - Paid and social, when in the retainer: `ads_get_advertiser_ads` (1 credit each) and the platform profile tools (1 credit each), as at onboarding.
3. **Compare.** For each metric: baseline, last month, this month, change. Call a move real only when it clears the noise: 3 or more positions for a keyword, 10 percent or more in estimated traffic, 2 or more cells in AI mentions. Anything smaller is "flat".
4. **Explain.** Tie each real move to the work log or to an outside cause: a competitor's jump (its overview), a keyword whose ranking page changed (`url` from `seo_get_position` differs from last month's), a seasonal dip (the baseline's 12-month line). Where the tools cannot say why, say so.
5. **Store.** Hand this month's numbers to the host to append to the client's record, so next month compares with them. To produce the report every month without being asked, the host schedules it and keeps the record, the way [weekly report](../../monitoring/references/weekly-report.md) sets up a recurring run.
6. **Deliver** the report as tables for the host to lay out:
   - Summary: three wins, one problem, and next month's three actions.
   - KPIs: metric, baseline, last month, this month, change, note.
   - Rankings: keyword, volume, baseline rank, last month's rank, rank now, ranking URL.
   - AI answers: prompt, engines that mention the client at baseline and now, competitors mentioned.
   - Links won and lost: domain, domain rank, first seen or lost.

   For each problem, name the playbook that fixes it, for example [quick wins](../../seo/references/quick-wins.md) for keywords stuck at 4 to 10, [content refresh](../../seo/references/content-refresh.md) for a page losing rank, or [incorrect answers](../../ai-search/references/incorrect-answers.md) when an engine gets the client wrong. Never send the report; if the host has a document or email tool, offer to pass it on.

## Judgment

- Report the same set every month. When the client wants a new keyword tracked, add it as a new row with its own start month; never replace an old one.
- Show losses beside wins. The client finds a drop on their own, and a report that hid it loses their trust.
- Estimated traffic is a model, and a 10 percent move can be the model. Rank and AI-mention changes on the tracked set are harder evidence.
- A keyword that moved 1 or 2 positions moved within the daily noise: `seo_get_position` is one day's reading.
- AI answers vary run to run. Report the count over prompts times engines and its trend over three months, not this month's wording.
- The backlink index lags the live web by weeks: links built late in the month may show next month. Say so.
- `seo_get_referring_domains` returns the 100 strongest domains by default. On a site with more, pass `limit: 1000` (25 credits) or the new small ones are missed.
- Leads, sales and revenue sit in the client's analytics and CRM, which these tools do not read. Leave rows for the client's own numbers rather than estimating them.
