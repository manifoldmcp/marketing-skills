---
name: leads
description: Leads and outbound prospecting with the manifold tools. Finds companies and people who fit an ideal customer profile, reveals and verifies work emails, and hands back a list for a sequencer or CRM. Also plans an outbound or ABM strategy, sizes a market in accounts and buyers, finds lookalikes of the best customers, spots accounts with buying signals (funding, tech stack, ads, posts about the problem), briefs an account before a sales call, writes personalized first lines, cleans an email list and enriches a spreadsheet. Use when the user asks for leads, a lead list, prospects, prospecting, outbound, cold outreach targets, ABM or target accounts, an ICP list, decision makers by job title, TAM or how many companies fit, companies like our customers, intent data or in-market accounts, call prep or account research, icebreakers, email verification or bounces, or enriching a CSV. A contact at a site about a backlink is link building. Sending, sequencing and CRM writes are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Leads

Every job here runs the same funnel: count, search, fill, reveal, verify. Search rows are cheap stubs and each step after them costs more, so each step cuts the list before the next one.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_search_people` and `leads_get_email` (hosts often add a prefix, for example `mcp__manifold__leads_search_people`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it no playbook here can find or verify a person; account brief and buying intent can still run their LinkedIn, ads and search steps.
- Buying intent, account brief and first lines also read `linkedin_*`, `ads_*` and `seo_*`. If one of those groups is switched off, skip the steps that need it and say which signal is missing from the result.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only says "leads", open lead list.

| Job | The user says | Open |
|---|---|---|
| Outbound strategy: who to target, which signals, and a 90-day plan | "outbound strategy", "plan our cold outreach", "ABM plan", "how should we do outbound", "start an outbound motion" | [references/strategy.md](references/strategy.md) |
| Market size: how many companies and buyers fit the ICP, counted not fetched | "how many companies fit our ICP", "TAM in accounts", "how many heads of HR in German logistics", "is the market big enough for outbound" | [references/market-size.md](references/market-size.md) |
| Lookalike companies: accounts that resemble the best customers | "companies like our best customers", "lookalike accounts", "more companies like Acme", "similar companies to our top accounts" | [references/lookalike-companies.md](references/lookalike-companies.md) |
| Buying intent: accounts showing a reason to buy now | "intent data", "in-market accounts", "companies that just raised", "accounts using a competitor", "who is showing buying signals" | [references/buying-intent.md](references/buying-intent.md) |
| Lead list: people by title at ICP companies, with verified emails, CSV-ready | "build a lead list", "find me leads", "prospect list", "VPs of sales at fintechs with emails", "decision makers at these accounts" | [references/lead-list.md](references/lead-list.md) |
| Account brief: one company and its buying committee before a call | "prep me for a call with", "research this account", "account brief", "who is on the buying committee at", "brief me before the demo" | [references/account-brief.md](references/account-brief.md) |
| First lines: one personalized opening line per lead, with its source | "personalized first lines", "icebreakers", "personalize these cold emails", "openers from their LinkedIn" | [references/first-lines.md](references/first-lines.md) |
| List cleaning: verify, flag and dedupe an email list the user has | "verify these emails", "clean my list", "check for bounces", "remove invalid emails", "catch-all check" | [references/list-cleaning.md](references/list-cleaning.md) |
| Enrichment: fill the empty columns of the user's spreadsheet | "enrich this CSV", "fill in titles and emails", "add company size to this sheet", "data enrichment", "complete my CRM export" | [references/enrichment.md](references/enrichment.md) |

## Shared rules

### Stubs and counts

- Search rows are stubs. `leads_search_people` masks `last_name` and leaves seniority, company domain, location and LinkedIn URL null; `leads_search_companies` leaves industry, employees and location null. The filters still applied: a null `industry` on a row from an industry search is not a miss.
- `leads_get_company` (10 credits) and `leads_get_person` (10 credits, 1 on `NoData`) fill one stub each. Decide on the stub (title, company, `has_email`) and fill only what a decision or the deliverable needs.
- `rows_available` on the first page is the provider's total for the filters. Read it instead of paging: counting accounts costs 10 credits a query and counting people 1.
- For a size and industry check on many companies, `linkedin_get_company` with the stub's LinkedIn URL (1 credit) gives LinkedIn's `employees`, `industry` and `location` at a tenth of `leads_get_company`. Keep `leads_get_company` for the fields only it has: `keywords[]`, `technologies[]`, `funding_stage`, `total_funding`, `revenue`.
- The provider's index is not the whole market. It is strongest on companies with a website and a LinkedIn presence, and thinner on small local businesses and on markets outside English-speaking tech. Say so whenever a count or a list is read as the market.

### Emails

- Reveal last, and only for people worth contacting. `leads_get_email` is the paid reveal: 6 credits on a hit, 1 on `NoData`. Do not retry a `NoData`: misses are not cached, so asking again pays again for the same answer.
- Pass the stub's `id` to `leads_get_email`: it fills the real name and domain, so there is no need to buy `leads_get_person` first just to unmask a last name.
- `has_email: true` on a stub means the provider already holds an address, so put those people first. `false` only means that provider has none; the waterfall behind `leads_get_email` may still find one.
- Person records never carry an email. With no name, only a company (anyone at a small firm), the address comes from a domain search: run the [contact steps](../link-building/SKILL.md#contact-steps) with the departments the buyer sits in.
- Verify before any send. `leads_get_email` verifies as it finds, so read its `verification_status`. Every other address (the user's file, a pattern guess, a domain search) gets `leads_get_email_status` (1 credit). Re-check a revealed address that came back `cached: true`: a reveal is cached 90 days and people change jobs. Read the status as the contact steps do: drop `invalid`, keep `accept_all` marked unproven, keep `unknown` with a note.

### Personal data

- These are records of real people. Collect only work data the user has a reason to use for this outreach: name, title, company, work email, LinkedIn URL. No personal addresses, phone numbers or private life in a list or a line.
- For people in the EU and the UK, cold B2B email rests on a legitimate interest the user can state, an opt-out in every message and a record of where the data came from. Some countries, Germany among them, want consent before any marketing email, B2B included. Say this once when a list covers those markets; the user decides the legal basis, and this is not legal advice.
- Keep a source column in every table (the search, the signal, `found_by`), so the user can answer "where did you get my address".
- A `webmail: true` address is a personal one. Flag it; do not add it to a cold list.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Spend in funnel order: count (1 or 10), search (1 or 10 a page), fill (10), reveal (6), verify (1). Never reveal an email for a row a cheaper step could have dropped.
- A result this account already paid for is free while cached: people searches 7 days, company searches, company and person records and verifications 30 days, reveals 90 days. A second pass over the same accounts costs little.
- The leads tools take one company or person per call. For a long list, run a few calls at a time; on `ConcurrencyLimit`, wait `retry_after_s` and send fewer at once.

### Handoff

- Never send, sequence, connect on LinkedIn or write to a CRM. The deliverable is a table with plain column headers that pastes into a sequencer or a CRM import. If the host has a sequencer or CRM tool, offer to pass the table to it; do not send.
- The server keeps no state. The host keeps the list, the exclusions (customers, open deals, people already contacted) and the date of each reveal.
- Write outreach copy only when the user asks. First lines is the one playbook that writes, and it writes one line per lead.

## Other groups

- A contact at a website about a backlink, a guest post or press: the [link-building](../link-building/SKILL.md) group, whose contact steps this group uses for any address found by domain.
- People posting about the problem on LinkedIn, as a list of posts rather than accounts: [LinkedIn problem posts](../linkedin/references/problem-posts.md). Posts to comment on for social selling: the [linkedin](../linkedin/SKILL.md) group.
- Reddit threads where people ask for a tool like the user's: [Reddit threads to reply to](../reddit/references/threads-to-reply.md).
- Who the buyer is and what they need, before any list: [personas](../customers/references/personas.md). The players and segments of a market: [market map](../customers/references/market-map.md). Search demand for the problem: [demand check](../customers/references/demand-check.md).
- Who the competitors are and how they sell and price: the [competitors](../competitors/SKILL.md) group. Their ads in depth: [competitor ads](../paid-ads/references/competitor-ads.md).
- Research on a prospect client for an agency pitch: [agency prospect audit](../agency/references/prospect-audit.md).
- First customers with no channel chosen yet: [first 100 customers](../growth-plan/references/first-100-customers.md).
- Watching accounts for new signals every week: the [monitoring](../monitoring/SKILL.md) group.
