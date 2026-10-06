---
name: prepare-client-pitch
description: When an agency, freelancer or consultant wants marketing data to win a prospect. Either a fast, cheap prospect audit that finds three findings the prospect can check (traffic against a competitor, quick-win keywords, site health, AI crawler access, AI answer mentions, ads), or the chart-ready tables for a pitch deck (12-month traffic, scoreboard, share of page one, keyword gap, AI share of voice, paid presence) against three competitors. Also use when the user mentions a prospect audit, a sales audit, a free SEO audit to win a client, a mini audit for cold outreach, a quick audit before a sales call, pitch data, numbers or charts for a pitch deck or proposal, or new business research. A client already signed goes to onboard-client, a full technical audit to audit-technical-seo, the person to send it to to research-account.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Agency pitch

Agencies win clients on evidence. This skill measures a prospect with a few cheap calls and sets the numbers against its competitors, in one of two shapes: a prospect audit that opens a sales conversation with three findings, or the tables for a pitch deck, one per slide. The depth behind any finding is paid work after the prospect signs, and goes to the skill that owns it.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_domain_overview` and `aeo_run_ai_answers` (hosts often add a prefix, for example `mcp__manifold__seo_get_domain_overview`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The steps read the `seo_*`, `aeo_*` and `ads_*` tools. If the `seo_*` tools are there and one of the others is not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and mark those rows "not measured" rather than dropping them: a prospect should see what the audit did not cover.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the agency's services and its own voice for client-facing documents) from it; ask only for what it lacks. The file describes the agency, not the prospect: the prospect's details come from the user. If it does not exist and the job needs more than two answers about the agency, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Deliverable**: a prospect audit (a quick look to open a sales call or go with a cold email) or pitch data (numbers for a deck or proposal). If the request does not say, ask one question.
- **Prospect**: the domain, and what the business sells and where (a local dentist, a national online store, a SaaS).
- **Service**: what the agency sells (SEO, AI search, paid, all of it). It decides which finding or slide leads.
- **Competitors**: one or two for an audit, three for a deck; the reference gives the default.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a prospect audit costs about 84 credits (under a dollar); pitch data about 438, or about 215 without the 12-month traffic history. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the reference.** A quick, cheap look that opens a conversation: [prospect audit](references/prospect-audit.md). Tables for a deck or a proposal: [pitch data](references/pitch-data.md). Pitch data the day after an audit reuses its calls.
2. **Settle the reference's own inputs** (competitors, buyer questions, category), with the defaults it gives.
3. **Run its steps**, saying the estimate before the first paid call.
4. **Deliver** what the reference names, every number with its tool and its date. The host or the user sends it; nothing is sent from here.

## Judgment

- Every number in a deliverable names its tool and its date (`meta.data_as_of` where the response gives it).
- Traffic from `seo_get_domain_overview` is a model of Google organic visits, not the prospect's analytics. Call it "estimated organic traffic", never quote it as their number, and compare like with like: every domain from the same source.
- AI answers are live and change between runs. Report a mention rate over prompts times engines, never one answer as a trend.
- An ad library shows which ads run and since when (`first_shown`), not what they cost. Spend is a published range on a few platforms and often null.
- If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. A result this account already paid for is free while cached (7 days for most search data), so pitch data the day after an audit, or onboarding the week after a pitch, pays again only for the calls that differ. AI answers are never cached.
- Never send the audit or the deck. The host's slide, document or email tools take the tables; offer to pass them on.

## Related skills

- The client signed: [onboard-client](../onboard-client/SKILL.md) turns the same set into the baseline.
- The full work behind a finding: [audit-technical-seo](../audit-technical-seo/SKILL.md), [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md), [check-ai-visibility](../check-ai-visibility/SKILL.md), [research-meta-ads](../research-meta-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md), [tear-down-competitor](../tear-down-competitor/SKILL.md).
- A list of companies to prospect: [build-lead-list](../build-lead-list/SKILL.md). The decision maker to send the audit to: [research-account](../research-account/SKILL.md).
- The agency's own growth, not a prospect's: [create-growth-plan](../create-growth-plan/SKILL.md).
