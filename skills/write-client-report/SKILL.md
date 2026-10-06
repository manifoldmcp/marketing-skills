---
name: write-client-report
description: When an agency, freelancer or consultant needs the monthly report for a client. Re-measures the tracked set from onboarding with the same calls (Search Console when granted, estimated organic traffic, keyword rankings, links won and lost, AI answer mentions in ChatGPT, Gemini, Claude, Perplexity and AI Overviews, ads and social), sets it against last month and the baseline, explains each real move, and hands the tables to the host to lay out and send. Also use when the user mentions a monthly client report, what moved for the client this month, a retainer report, a monthly SEO or marketing report for a client, or a white-label report. A client with no baseline yet goes to onboard-client, a weekly report on the user's own brand to write-weekly-report, a prospect not yet signed to prepare-client-pitch.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Client report

A monthly report answers three questions for the client: what moved, why, and what happens next month. It re-measures the tracked set from onboarding with the same calls and params, sets it against last month and the baseline, and hands the tables to the host to lay out and send.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_position` and `aeo_run_ai_answers` (hosts often add a prefix, for example `mcp__manifold__seo_get_position`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The report reads the `seo_*`, `aeo_*`, `ads_*` and `console_*` tools, and the platform profile tools for social reach. If the `seo_*` tools are there and one of the others is not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and mark those rows "not measured" rather than dropping them: a client should see what the report did not cover.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the agency's own voice for client-facing documents; for the client, their own context file if the work runs in their project) from it; ask only for what it lacks. The tracked set itself comes from the stored onboarding record, not from the file. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Client and baseline**: the stored record from [onboard-client](../onboard-client/SKILL.md) and the months since, from the host. Without a baseline there is nothing to compare: run onboarding now, and this month becomes the baseline.
- **Period**: the month reported. Default: the last full calendar month, measured now.
- **Work done**: what the agency shipped in the month (pages, fixes, links, campaigns). The tools see results, not work; the report needs both.
- **Channels**: the ones in the retainer. Measure nothing else.
- **Budget**: a default report costs about 4 x 5 + 20 x 6 + 10 + 12 + 5 x 18 + 2 + 4 = 258 credits per client per month, for 20 keywords, 5 prompts and 3 competitors; 271 when the client has over 100 referring domains (step 2). Each extra tracked keyword adds 6 credits a month, each extra prompt 18. Search Console and the readiness check are free; each SERP check in step 4 adds 2. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Load the tracked set.** Read the domain, competitors, keywords, prompts and locale from the stored record, and use them exactly as stored: a changed `location`, device or prompt wording breaks the comparison.
2. **Re-measure.** The same calls as the baseline, with the same params:
   - Search: when the client granted Search Console access, `console_get_search_analytics` for the month and for the month before (free): real clicks, impressions, CTR and position, which lead the KPI table. Then `seo_get_domain_overview` on the client and each competitor (5 credits each), without history, labelled as the estimate; the stored months are the history.
   - Rankings: `seo_get_position` for each tracked keyword (6 credits each). Tracking them weekly between reports is [track-rankings](../track-rankings/SKILL.md).
   - Links: `seo_get_backlink_summary` on the client (10 credits), and `seo_get_referring_domains` on it for the rows with `first_seen` in the month (won) and with `lost: true` (lost): 12 credits for the default 100 rows, which are the strongest domains, so pass `limit: 1000` (25 credits) when the summary's `referring_domains` is over 100, or the new small ones are missed. Count only won domains that clear the [floors](../create-link-building-plan/references/outreach.md#floors).
   - AI answers: `aeo_run_ai_answers` with the tracked prompts and brands (18 credits a prompt), then `get_task`. Tracking them between reports is [check-ai-visibility](../check-ai-visibility/SKILL.md) on a schedule.
   - Paid and social, when in the retainer: `ads_get_advertiser_ads` (1 credit each) and the platform profile tools (1 credit each), as at onboarding.
   - AI crawler access: `aeo_get_site_readiness` on the client's site (free). A check with `tier: "blocks_citations"` that now shows `ok: false` (often a CDN or firewall rule changed since last month) is the first line of the report.
3. **Compare.** For each metric: baseline, last month, this month, change. Call a move real only when it clears the noise: 3 or more positions for a keyword, 10 percent or more in estimated traffic, 3 or more cells of 25 in AI mentions (a 10-point move in mention share). Anything smaller is "flat".
4. **Explain.** Tie each real move to the work log or to an outside cause: a competitor's jump (its overview), a keyword whose ranking page changed (`url` from `seo_get_position` differs from last month's), a seasonal dip (the baseline's 12-month line). Two outside causes to rule out first: a Google core or spam update in the month, which no tool reads (the user or the host's browser checks Google's Search Status Dashboard, and the report names the update when the dates match); and clicks that fell while rank held, where `seo_get_serp` with `ai_overview: true` on the top keywords (2 credits each) shows whether an AI Overview now sits above the client and whether it cites them. Where the tools cannot say why, say so.
5. **Store.** Hand this month's numbers to the host to append to the client's record, so next month compares with them. If the host cannot keep files, give the user the record as a table to store and paste back. To produce the report every month without being asked, the host schedules it and keeps the record; the server stores nothing and schedules nothing.
6. **Deliver** the report as tables for the host to lay out:
   - Summary: three wins, one problem, and next month's three actions.
   - KPIs: metric, baseline, last month, this month, change, note.
   - Rankings: keyword, volume, baseline rank, last month's rank, rank now, ranking URL.
   - AI answers: prompt, engines that mention the client at baseline and now, competitors mentioned.
   - Links won and lost: domain, domain rank, first seen or lost.

   For each problem, name the skill that fixes it, for example [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md) for keywords stuck at 4 to 10, [refresh-content](../refresh-content/SKILL.md) for a page losing rank, or [fix-wrong-ai-answers](../fix-wrong-ai-answers/SKILL.md) when an engine gets the client wrong. Never send the report; if the host has a document or email tool, offer to pass it on.

## Judgment

- Report the same set every month. When the client wants a new keyword tracked, add it as a new row with its own start month; never replace an old one. A number measured another way is not a change; it is a different number.
- Show losses beside wins. The client finds a drop on their own, and a report that hid it loses their trust.
- Every number names its tool and its date (`meta.data_as_of` where the response gives it). Estimated traffic is a model of Google organic visits, not the client's analytics, and a 10 percent move can be the model. Rank and AI-mention changes on the tracked set are harder evidence.
- A keyword that moved 1 or 2 positions moved within the daily noise: `seo_get_position` is one day's reading.
- AI answers vary run to run. Report the count over prompts times engines and its trend over three months, not this month's wording.
- An ad library shows which ads run and since when, not what they cost. Spend is a published range on a few platforms and often null.
- The backlink index lags the live web by weeks: links built late in the month may show next month. Say so.
- Leads, sales and revenue sit in the client's analytics and CRM, which these tools do not read. Leave rows for the client's own numbers rather than estimating them.
- If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. AI answers are never cached.

## Related skills

- The baseline this report compares with: [onboard-client](../onboard-client/SKILL.md).
- Weekly tracking between reports, whose sections this report can reuse: [write-weekly-report](../write-weekly-report/SKILL.md), [track-rankings](../track-rankings/SKILL.md), [check-ai-visibility](../check-ai-visibility/SKILL.md).
- A drop the report found, diagnosed: [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md).
- Winning the next client: [prepare-client-pitch](../prepare-client-pitch/SKILL.md).
