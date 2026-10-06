# Enrichment

The user's own spreadsheet, filled in row by row: company fields from the domain, person fields and a work email from the name and domain. The user's data is the record, so nothing they typed is ever overwritten. It ends in the same sheet with the empty cells filled, new columns appended and a note on every row the tools could not match.

## Inputs to settle first

- **The sheet**: a file or a paste, and which columns hold the name, company, domain, email and LinkedIn URL.
- **Fields to fill**: which empty columns the user wants: company fields (industry, employees, location, description, LinkedIn page), person fields (title, location, LinkedIn URL) or the work email. Default: only the columns the user names. Funding, revenue, tech stack, seniority and employment history come back empty: say so if the user asks for them.
- **Rows**: all, or a filter (for example only rows with no email).
- **Budget**: price it per row before starting. Company fields cost 1 credit per unique domain; person fields 3 per person (1 on a miss); an email 6 per person on a hit (1 on a miss). A 200-row sheet with 120 unique domains, filled in full, is about 120 x 1 + 200 x (3 + 6) = 1,920 credits. Say so and get a yes before the first paid call; pass `max_credits` if the user gave a budget.

## Steps

1. **Map the columns.** The host reads the sheet and maps each column to a field. Where the domain is missing, take it from the website column or from the email (never from a webmail address like gmail.com). Split full names into first and last. A row with only an email and no name cannot be looked up: no tool finds a person from an address.
2. **Price it.** Count the unique domains and the people with an empty target cell, apply the per-row prices above, and give the total. `dry_run: true` on one call of each tool confirms the price for free.
3. **Test on ten rows.** Run the steps below on the first ten rows and show the result. Check the matches (the right company, the right person) before spending on the rest.
4. **Companies.** `leads_get_company` once per unique domain (1 credit), not once per row. Fill only the empty company cells: `industry`, `employees`, `location`, `description` and the LinkedIn page URL.
5. **People.** `leads_get_person` by LinkedIn URL when the sheet has one (a full `https://www.linkedin.com/in/` profile URL; anything else is refused as `InvalidTarget`), otherwise `first_name`, `last_name` and `domain` (3 credits, 1 on `NoData`). Check the match: if the returned `company_domain` differs from the row's domain, the person has moved; flag the row "moved" and fill nothing from it without asking. Skip this step when the user wants only emails.
6. **Emails.** `leads_get_email` with `first_name`, `last_name` and `domain` (or the person's `id` from step 5), only for rows with an empty email cell (6 credits on a hit, 1 on `NoData`). It returns `verification_status`; keep it in its own column. Do not retry a `NoData`. Rows that already have an email are not re-found; verifying them is [list cleaning](list-cleaning.md).
7. **Work in batches.** Run 25 to 50 rows at a time, a few calls in parallel, and report progress and credits spent after each batch. Stop at the budget.
8. **Merge without overwriting.** Fill only empty cells. Where a tool disagrees with a value the user typed, put the tool's value in a new column (for example `title_found`) and flag the row; never change the user's cell. Append these columns: match (matched, moved, not found), source (`meta.provider` or `found_by`), enriched_on (the date).
9. **Deliver** the sheet with the original columns and order untouched and the new columns at the end, plus a summary: rows processed, cells filled per field, rows not found, rows flagged, and credits spent. If the host can write files, it saves a copy; the original stays as it was.

## Judgment

- Dedupe the domains before any company call. Ten people at one company pay for the company once, and a domain this account filled in the last 30 days is free.
- A miss costs a credit and teaches something: many misses on one kind of row (small local firms, one country) mean the provider's index is thin there. Say so after the test batch rather than buying 200 misses.
- A name match at the wrong company is worse than no match. When the name is common and the domain does not match, leave the row empty and flag it.
- Old sheets decay: people move and addresses die. For a sheet more than a few months old, run the person step even when the user wants only emails, so the email is searched at the current company.
- Only the fields the user asked for. Filling every available field makes a heavier sheet and a larger personal-data footprint; see the router's [personal data](../SKILL.md#personal-data) rules.
