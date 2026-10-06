---
name: build-lead-list
description: When the user wants a lead list, people by job title at companies that fit the ideal customer profile, with verified work emails. Searches the accounts, then the buyers at them, reveals and verifies emails for the shortlist only, and hands back a CSV-ready table for a sequencer or CRM import. Also use when the user mentions leads, find me leads, build a lead list, prospect list, prospecting, an ICP list, decision makers by job title, VPs of sales at fintechs with emails, or decision makers at these accounts. Accounts that resemble the best customers go to find-lookalike-companies, accounts with a reason to buy now to find-buying-signals, filling the user's own sheet to enrich-lead-list, a contact at a website about a backlink to create-link-building-plan. Sending, sequencing and CRM writes are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Lead list

A lead list is accounts first, then the buyers at them, then an address for the few worth contacting. Searches are cheap and the reveal is not, so the list is cut on the search rows and only the shortlist is revealed and verified. It ends in a CSV-ready table for the user's sequencer or CRM.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_search_people` and `leads_get_email` (hosts often add a prefix, for example `mcp__manifold__leads_search_people`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it this skill cannot find or verify a person.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the ICP, the personas and job titles, the countries) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Accounts**: the ICP as company filters (industries, locations, employee bands), or a list of domains the user already has, or the output of [find-lookalike-companies](../find-lookalike-companies/SKILL.md) or [find-buying-signals](../find-buying-signals/SKILL.md).
- **Buyers**: three to six full titles of the person who buys ("head of growth", "VP marketing", "director of demand generation"), passed with no `seniority`; see the [search rules](../create-outbound-plan/references/lead-data.md#searches-and-counts). Default: the economic buyer plus one likely champion.
- **Size**: contacts wanted and the cap per account. Default: 50 contacts, at most two per account.
- **Exclusions**: customers, open deals and people already contacted, as domains and emails. The host holds them.
- **Budget**: a default run of 50 contacts costs about 2 x 4 + 2 x 4 + 50 x 6 + 10 x 1 = 326 credits: two company searches, two people searches, 50 reveals and a few re-checks. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Accounts.** `leads_search_companies` with the ICP filters, plus `keywords` for the category words an industry name misses (4 credits a page of up to 100 rows), or take the user's domains. Read the first 20 rows: if fewer than about 15 fit, change the `industries` or the employee band before going on. Remove the exclusions by domain. The rows carry `industry`, `employees` and `location`, so no company needs a record.
2. **People.** `leads_search_people` with the buyer `titles` and `company_domains` set to up to 100 account domains per call (4 credits a page of up to 100 rows). When `meta.cursor` comes back, pass it as `cursor` for the next page (4 credits a page) until the accounts are covered. The rows carry the full name, `title`, `company`, `company_domain`, `location` and `linkedin_url`.
3. **Shortlist.** At most two people per account: the title closest to the buyer first, then the champion. Drop titles that only match the words ("growth investor" for "growth"). Cut to the size the user asked for.
4. **Small accounts with no match.** Where the search finds no buyer at a small company, the founder or a generic address is the contact: run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) with `department: ["executive"]` and `limit: 10`.
5. **Reveal.** `leads_get_email` with the row's `id` (6 credits on a hit, 1 on `NoData`). It returns `email`, `verification_status` and `found_by`. Do not retry a `NoData`; mark the row "no email found". Reveal the first 10 and count the hits: under 5, stop and say that the titles are too junior or the companies too small for the provider's coverage, rather than buying more misses.
6. **Verify.** Follow the [email rules](../create-outbound-plan/references/lead-data.md#emails).
7. **Deliver** a CSV-ready table with plain headers: first_name, last_name, title, company, company_domain, email, verification_status, linkedin, location, source (the search or signal that put the row there), and notes (accept_all, no email found, second contact at the account). Give the counts: accounts searched, people found, revealed, verified, and credits spent. Do not send; the host's sequencer or CRM import takes the table.

## Judgment

- Accounts before people. A people search across a whole industry returns thousands of loosely matched titles; a search inside 100 accounts that fit returns buyers.
- Two people per account at most in one sequence. Five people at one company getting the same email in the same week reads as spam, and they compare notes.
- A reveal that comes back `accept_all` is common at large companies, whose mail servers accept anything. Keep those rows in their own batch, send it after the verified rows, and keep it out of the first sends from a new domain.
- Spend in funnel order and never reveal an email for a row a cheaper step could have dropped; the [credit rules](../create-outbound-plan/references/lead-data.md#credits) give the cache lifetimes and how to pace a long list.
- Never send, sequence or write to a CRM, and write no outreach copy unless asked; see the [handoff rules](../create-outbound-plan/references/lead-data.md#handoff).
- Lists covering the EU or the UK follow the [personal data](../create-outbound-plan/references/lead-data.md#personal-data) rules.
- A contact at a website about a link is not a lead: that is [create-link-building-plan](../create-link-building-plan/SKILL.md).

## Related skills

- Accounts to feed the list: [find-lookalike-companies](../find-lookalike-companies/SKILL.md) for companies like the best customers, [find-buying-signals](../find-buying-signals/SKILL.md) for accounts with a reason to buy now.
- An opening line for each lead: [write-first-lines](../write-first-lines/SKILL.md). Prep for a call with one account: [research-account](../research-account/SKILL.md).
- A list the user already has, to verify or fill in: [clean-email-list](../clean-email-list/SKILL.md) or [enrich-lead-list](../enrich-lead-list/SKILL.md).
- Who to target and how many, before any list: [create-outbound-plan](../create-outbound-plan/SKILL.md). Who the buyer is and what they need: [build-personas](../build-personas/SKILL.md).
- A contact at a website about a backlink, a guest post or press: [create-link-building-plan](../create-link-building-plan/SKILL.md).
