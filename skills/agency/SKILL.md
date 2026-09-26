---
name: agency
description: Agency work with the manifold tools. Audits a prospect's site in minutes to open a sales conversation, pulls the numbers and chart-ready tables for a pitch deck against the prospect's competitors, sets a new client's baseline and first plan, and writes the monthly client report against that baseline. Use when an agency, freelancer or consultant asks for a prospect audit, a sales audit, a free SEO audit to win a client, a mini audit for cold outreach, pitch data, numbers or charts for a pitch deck or proposal, pitching a client tomorrow, new business research, client onboarding, a kickoff baseline, benchmarks for a new retainer, a client's first 90 days, a monthly client report, a monthly SEO or marketing report for a client, retainer reporting, or a white-label report. The playbooks run a cheap baseline and route the depth to the seo, ai-search, competitors, paid-ads and monitoring groups. Sending the audit, building the slides and scheduling the report stay with the host.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Agency

Agencies win clients on evidence and keep them on progress. Every job here measures a prospect or a client with a few cheap calls, sets the numbers against competitors or against a stored baseline, and hands the depth to the group that owns it.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_domain_overview` and `aeo_run_ai_answers` (hosts often add a prefix, for example `mcp__manifold__seo_get_domain_overview`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The playbooks read the `seo_*`, `aeo_*` and `ads_*` tools, and the platform profile tools for social reach. If the `seo_*` tools are there and one of the others is not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and mark those rows "not measured" rather than dropping them: a prospect or a client should see what the audit did not cover.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. A request about the agency's own marketing, not a client's, belongs to another group (see below).

| Job | The user says | Open |
|---|---|---|
| Prospect audit: a fast, cheap look at a prospect's site that opens a sales conversation | "audit this prospect's site", "quick audit before a sales call", "free audit to send a lead", "what's wrong with this potential client's SEO", "mini audit for cold outreach" | [references/prospect-audit.md](references/prospect-audit.md) |
| Pitch data: the numbers and chart-ready tables for a pitch deck, prospect against competitors | "pitching a client tomorrow", "numbers for the pitch deck", "data for our proposal", "how does the prospect compare with its competitors", "charts for the new business pitch" | [references/pitch-data.md](references/pitch-data.md) |
| Client onboarding: the baseline every later report compares with, and the first plan | "onboard a new client", "baseline for a new retainer", "kickoff benchmarks", "where does our new client stand today", "first 90 days for a new client" | [references/client-onboarding.md](references/client-onboarding.md) |
| Monthly report: the client's numbers this month against last month and the baseline | "monthly client report", "what moved for the client this month", "retainer report", "monthly SEO report for a client", "white-label report" | [references/monthly-report.md](references/monthly-report.md) |

## Shared rules

### Measurement set

- A client's tracked set is fixed at onboarding: the domain, up to three competitors, 10 to 30 keywords, 3 to 10 buyer prompts, and one `location`, `language` and device. Every report re-measures exactly that set with the same calls and params. A number measured another way is not a change; it is a different number.
- Add, never swap. A keyword or prompt the client adds later joins as a new row with its own start month.

### Sources and estimates

- Every number in a deliverable names its tool and its date (`meta.data_as_of` where the response gives it).
- Traffic from `seo_get_domain_overview` is a model of Google organic visits, not the client's analytics. Call it "estimated organic traffic", never quote it as their number, and compare like with like: every domain from the same source.
- AI answers are live and change between runs. Report a mention rate over prompts times engines, never one answer as a trend.
- An ad library shows which ads run and since when (`first_shown`), not what they cost. Spend is a published range on a few platforms and often null.

### Credits

- Per prospect: a prospect audit costs about 85 credits (under a dollar); pitch data about 440 credits, or about 215 without the 12-month traffic history.
- Per client: onboarding about 330 credits once, plus the strategy playbook it opens; the monthly report about 260 credits per client per month for 20 keywords, 5 prompts and 3 competitors. Each extra tracked keyword adds 6 credits a month, each extra prompt 18.
- Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- A result this account already paid for is free while cached (7 days for most search data), so pitch data the day after a prospect audit, or onboarding the week after a pitch, pays again only for the calls that differ. AI answers are never cached.

### Handoff

- The server stores nothing. The host keeps each client's baseline and monthly numbers (a file in the client folder, a sheet, the project's memory) and gives them back to the monthly report. If it cannot keep files, give the user the record as a table to store and paste back.
- Never send the audit, the deck or the report, and never schedule it. The host's slide, document or email tools take the tables; offer to pass them on. A recurring report is scheduled by the host, the way [weekly report](../monitoring/references/weekly-report.md) sets up its schedule.

## Other groups

- The full work behind a finding, once the client signs: [seo](../seo/SKILL.md) (for example its [audit](../seo/references/audit.md) and [quick wins](../seo/references/quick-wins.md)), [ai-search](../ai-search/SKILL.md), [paid-ads](../paid-ads/SKILL.md) and [competitors](../competitors/SKILL.md).
- A list of companies to prospect, or the decision maker to send the audit to: [leads](../leads/SKILL.md), its [lead list](../leads/references/lead-list.md) and [account brief](../leads/references/account-brief.md).
- Weekly tracking and alerts for a client between reports: [monitoring](../monitoring/SKILL.md).
- The agency's own growth, not a client's: [growth-plan](../growth-plan/SKILL.md).
- A client's product launch: [launch](../launch/SKILL.md).
