---
name: enrich-lead-list
description: When the user wants a contact spreadsheet or CRM export they already have filled in. Fills the empty company cells from the domain and the empty person and work email cells from the name and domain, row by row, flags people who have moved company, and never overwrites anything the user typed, putting a disagreeing value in a new column. Also use when the user mentions enrich this CSV, fill in titles and emails, add company size or industry to this sheet, find LinkedIn URLs for these contacts, data enrichment, append emails, or complete my CRM or HubSpot export. Verifying the addresses already in a list goes to clean-email-list, a new list of people by job title to build-lead-list, and past contacts who changed jobs to track-job-changes. Sending and CRM writes are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Enrich lead list

The user's own spreadsheet, filled in row by row: company fields from the domain, person fields and a work email from the name and domain. The user's data is the record, so nothing they typed is ever overwritten. It ends in the same sheet with the empty cells filled, new columns appended, a note on every row the tools could not match, and a summary.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_get_company`, `leads_get_person` and `leads_get_email` (hosts often add a prefix, for example `mcp__manifold__leads_get_email`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it this skill cannot fill a cell; only the free column mapping can run.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the ICP, the countries the sheet covers) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **The sheet**: a file or a paste, and which columns hold the name, company, domain, email and LinkedIn URL.
- **Fields to fill**: which empty columns the user wants: company fields (industry, employees, location, description, LinkedIn page), person fields (title, location, LinkedIn URL, headline) or the work email. Default: only the columns the user names. Funding, revenue, tech stack and seniority are not in the data: say so before the run rather than charge for empty cells.
- **Rows**: all, or a filter (for example only rows with no email).
- **Budget**: company fields 1 credit per unique domain; person fields 3 per person on a hit (1 on a miss); an email 6 per person on a hit (1 on a miss). A 200-row sheet with 120 unique domains, filled in full, is about 120 x 1 + 200 x (3 + 6) = 1,920 credits. Say it and get a yes before the first paid call; pass `max_credits` if the user gave a budget, stopping when the running total reaches it.

## Steps

1. **Map the columns.** The host reads the sheet and maps each column to a field. Nothing paid happens yet. Where the domain is missing, take it from the website column or from the email (never from a webmail address like gmail.com). Split full names into first and last. A row with only an email and no name cannot be looked up: no tool finds a person from an address.
2. **Price it.** Count the unique domains and the people with an empty target cell, apply the per-row prices, and give the total. `dry_run: true` on one call of each tool confirms the price for free.
3. **Test on ten rows.** Run the steps below on the first ten rows and show the result. Check the matches (the right company, the right person) before spending on the rest.
4. **Companies.** `leads_get_company` once per unique domain (1 credit, hit or miss), not once per row. Fill only the empty company cells: `industry`, `employees`, `location`, `description` and the LinkedIn page (`linkedin_url`).
5. **People.** `leads_get_person` by `linkedin_url` when the sheet has a full profile URL (https://www.linkedin.com/in/<public-id>), otherwise `first_name`, `last_name` and `domain` (3 credits when matched, 1 on `NoData`). Any other LinkedIn form is `InvalidTarget` at no charge, so use the name and domain for it. Check the match: if the returned `company_domain` differs from the row's domain, the person has moved; flag the row "moved" and fill nothing from it without asking. Another domain of the same `company` (a country site, a rebrand) is a match, not a move. Skip this step when the user wants only emails.
6. **Emails.** `leads_get_email` with `first_name`, `last_name` and `domain` (or the person's `id` from step 5, its LinkedIn URL), only for rows with an empty email cell (6 credits on a hit, 1 on `NoData`). It returns `verification_status`; keep it in its own column. Do not retry a `NoData`. Rows that already have an email are not re-found; verifying them is [clean-email-list](../clean-email-list/SKILL.md).
7. **Work in batches.** Run 25 to 50 rows at a time, a few calls in parallel, and report progress and credits spent after each batch. On `ConcurrencyLimit`, wait `retry_after_s` and send fewer at once. Stop at the budget.
8. **Merge without overwriting.** Fill only empty cells. Where a tool disagrees with a value the user typed, put the tool's value in a new column (for example `title_found`) and flag the row; never change the user's cell. Append these columns: match (matched, moved, not found), source (`meta.provider` or `found_by`), enriched_on (the date).
9. **Deliver** the sheet with the original columns and order untouched and the new columns at the end, plus a summary: rows processed, cells filled per field, rows not found, rows flagged, and credits spent. If the host can write files, it saves a copy; the original stays as it was. Do not send.

## Judgment

- Dedupe the domains before any company call. Ten people at one company pay for the company once, and a company or person record this account paid for in the last 30 days comes back from the cache for free.
- A miss costs a credit and teaches something: many misses on one kind of row (small local firms, one country) mean the provider's index is thin there. Say so after the test batch rather than buying 200 misses.
- A name match at the wrong company is worse than no match. When the name is common and the domain does not match, leave the row empty and flag it.
- Old sheets decay: people move and addresses die. For a sheet more than a few months old, run the person step (1 credit) even when the user wants only emails, so the email is searched at the current company.
- Only the fields the user asked for. Filling every available field makes a heavier sheet and a larger personal-data footprint; see the [personal data](../create-outbound-plan/references/lead-data.md#personal-data) rules.
- A found address still needs a legal basis to receive cold email. Never send, sequence or write to a CRM; see the [handoff rules](../create-outbound-plan/references/lead-data.md#handoff).

## Related skills

- The addresses already in the sheet verified, with the bounces dropped: [clean-email-list](../clean-email-list/SKILL.md).
- A new list of people by job title at companies that fit: [build-lead-list](../build-lead-list/SKILL.md).
- Past champions and buyers in the CRM export who moved to a new company: [track-job-changes](../track-job-changes/SKILL.md).
- An opening line for each row once the sheet is filled: [write-first-lines](../write-first-lines/SKILL.md).
