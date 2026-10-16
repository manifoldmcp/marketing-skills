# Sizing a market

How big a market is, counted so the number survives a sceptical reader: accounts and buyers for B2B, searches and households for consumer markets, and a value only where a price is known. Checked October 2026.

## Top-down and bottom-up

- **Top-down** takes a published market figure (an analyst report, a census total) and applies shares ("1% of the $80B CRM market"). It is quick and almost always too big: the category in the report is defined differently from the user's, the figure is often two or three years old and its method opaque, and the share is asserted, not measured. Use it only as a ceiling and a cross-check.
- **Bottom-up** counts the accounts or people that fit, times what each would pay. It is the number to lead with, and it misleads in its own ways: the count is only as good as the filter (precision, below), the index behind it is thin in places (local businesses, sole traders, non-English markets), and the price is a guess until real deals set it.
- **Run both and explain the gap.** If the top-down figure is ten times the bottom-up count, either the filter or the index is missing most of the market, or the report counts a wider category. Say which is more likely; do not average them.
- **SAM is what the user can sell to now** (the language, time zone, compliance and product fit they have today), not the sum of every segment. SOM is a forecast, not a count; give it only as a range tied to a stated conversion assumption, and say it is an assumption.
- **No public figure validates a TAM.** No study has back-tested analyst market sizes against outcomes; the case against top-down figures is practitioners' and investors' judgement, not data. Say that rather than citing a percentage.

## Counting accounts bottom-up

With the connector, count from the lead provider's totals rather than fetching rows. A segment of 40,000 accounts costs 4 credits to count; revealing a buyer at each would cost far more.

1. **Turn the ICP into filters** on `leads_search_companies`: `industries[]`, `locations[]`, `employee_ranges[]` as "min,max" and `keywords[]`. Industries are LinkedIn's names; `leads_get_company` (1 credit) on two of the user's customers shows the exact `industry` string to copy. The user's category word ("SaaS", "fintech") is a keyword, not an industry. The full filter rules belong to [outbound](../../outbound/SKILL.md).
2. **Read `rows_available`, then measure precision.** `rows_available` is the provider's total for the filters, before the keyword check. Read the rows and count how many truly fit; precision is fitting rows over rows fetched (the `limit`). With `keywords[]` set the provider drops rows the keyword check rejects, so a page can return fewer rows than it fetched: count the dropped ones as misfits, since `rows_available` still includes them. Accounts that fit = `rows_available` × precision.
3. **Under about 50% precision the filter is measuring something else.** Try a neighbouring industry name or a narrower band and count again (4 credits) rather than report the big number. In our test a dental filter in Germany returned 2,277 companies of which at most 3 in 25 were practices ([field notes](field-notes.md)).
4. **Count buyers per account.** People search has no company-size filter, so search the buyer `titles` with `company_domains` set to up to 100 domains from the segment's sample: `rows_available` over the accounts searched is buyers per account. Read the sample titles first; title matching is loose ("head of growth" returns investors too).
5. **Value** = accounts that fit × annual contract value, only when the user has a real deal size.

Deliver a table per segment: filters, `rows_available`, precision, accounts that fit, buyers per account, buyers, value, three example companies, and one line on where to start. Under about 500 accounts that fit, outbound is account-based: each account deserves research.

**Where the index fails.** It is strongest on companies with a website and a LinkedIn page. It undercounts small local businesses (clinics, trades, restaurants), sole traders, and markets outside English-speaking tech, and its coverage differs by country. For those, call the count a floor, or switch to public statistics: business registers and official counts by industry code (in the US the Census Bureau's County Business Patterns by NAICS code, in the EU Eurostat's structural business statistics, in the UK Companies House and the ONS business population figures), or the professional body's own membership count.

## Consumer markets

Count people, not companies. Search volume on commercial terms sizes active intent (see [demand signals](demand-signals.md) for its traps: buckets, 12-month means, grouped variants). Population, household and ownership statistics size the base. Never multiply monthly searches by twelve and call them buyers: one buyer searches many times, and most searchers do not buy.

## Without the connector

Do the same arithmetic by hand from public statistics, the user's own customer counts and published price points, and label each input with its source and date. The method matters more than the tool: show every multiplication so the reader can change an assumption.
