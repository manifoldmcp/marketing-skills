# Lists

Building a lead list, finding companies like the best customers, and filling in a sheet the user already has. All three run on the [lead data](lead-data.md) rules; this page holds the judgement each job needs on top. List quality moves replies more than copy does: Hunter's 2025 data put 21-50 recipient sequences at 6.2% replies against 2.4% for 500+, and one or two people per company at 5.1% against 3.5% for three or more.

## A lead list

Accounts first, then the buyers at them, then an address for the few worth contacting.

- **Inputs:** the ICP as company filters (or the user's domains, or a lookalike or signal list); three to six full buyer titles (the economic buyer plus one likely champion); the size (default 50 contacts, at most two per account); the exclusions (customers, open deals, people contacted, opt-outs), which the host holds.
- **Two people per account at most** in one sequence. Five people at one company getting the same email in a week reads as spam, and they compare notes.
- **Cut on the rows, for free.** Drop titles that only match the words ("growth investor" for "growth"), junk rows, and people whose `location` puts them in a market the user has no basis to email. Then reveal the shortlist and check the hit rate on the first 10.
- **Small accounts with no buyer found:** the founder or a named person from `leads_get_domain_emails` with `department: ["executive"]`. A generic address is the last resort, not the default.
- **Batch by risk:** verified rows first, `accept_all` rows later and in small batches, and keep catch-alls and gateway domains out of the first sends from a new domain.
- **Deliver** the CSV-ready table from [lead data](lead-data.md#handoff) with counts: accounts searched, people found, revealed, verified, credits spent. About 408 credits for 50 contacts.

## Lookalike companies

The best predictor of the next customer is the last good one.

- **Three good customers make a pattern; one does not.** Best by fit (bought fast, stayed, paid well), not the biggest logo: a famous customer that took a year to close teaches the wrong pattern. With fewer than three, say the list is a guess.
- **Profile them** with `leads_get_company` (1 credit each): the `industry` strings they share, the employee band that covers them, the regions, and `keywords[]` specialties that appear on three or more records.
- **Separate on what sets them apart.** A trait every company in the market has (the country, "Software Development") separates nobody; drop it from the scoring. Specific specialties ("payment orchestration") are strong, generic ones ("SaaS") weak. Companies type specialties themselves and small ones often leave them empty: score the `description` instead.
- **A customer filed under an odd industry** gets its own search, not a place in the pattern. LinkedIn industries are self-chosen.
- **Score on the rows at no further cost:** a point each for shared specialties, industry, employee band, region, and a description naming the same kind of customer or problem. Drop anything that misses two of the first four.
- **Lookalikes repeat the current customers' bias.** A new segment is a market-sizing question first: [market-research](../../market-research/SKILL.md).
- A default run (five customer records, three searches) is about 41 credits. Deliver the best 20 with the traits matched and the customer each most resembles.

## Enriching the user's sheet

The user's spreadsheet or CRM export, filled in row by row. The user's data is the record.

- **Price it first.** Company fields 1 credit per unique domain (dedupe the domains first); person fields 3 per person on a hit (9 by LinkedIn URL); an email 7 per person on a hit. A 200-row sheet with 120 domains filled in full is about 2,120 credits. Get a yes before the first paid call, and test on ten rows before the rest.
- **Map before paying.** Take a missing domain from the website column or the email, never from a webmail address. A row with only an email and no name cannot be looked up: no tool finds a person from an address.
- **Only the fields the user asked for.** Every extra field is a heavier sheet and a larger personal-data footprint. Funding, tech stack and seniority are not in the data; say so before the run rather than charge for empty cells.
- **A name match at the wrong company is worse than no match.** When `leads_get_person` returns a `company_domain` that differs from the row's, the person has moved: flag "moved" and fill nothing from it without asking. Another domain of the same company (a country site, a rebrand) is a match. With a common name and no domain match, leave the row empty.
- **Old sheets decay.** For a sheet more than a few months old, run the person step even when the user wants only emails, so the email is searched at the current company.
- **Never overwrite.** Fill empty cells only. A disagreeing value goes in a new column (`title_found`, for example) and the row is flagged. Append match (matched, moved, not found), source and enriched_on.
- Many misses on one kind of row (small local firms, one country) mean the index is thin there. Say so after the test batch rather than buying 200 misses.

## Without the connector

Lists can still be planned: write the ICP as filters, the buyer titles, the exclusions and the sending capacity, and build accounts by hand from LinkedIn company search and the customers' own competitors. Never fabricate a contact or guess an address and present it as found.
