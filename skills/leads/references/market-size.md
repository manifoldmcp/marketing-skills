# Market size

How many companies fit the ICP, and how many buyers sit in them, counted from the provider's totals rather than fetched row by row. It ends in a table of segments with accounts, buyers and a value, which tells the user whether outbound has enough room and where to start.

## Inputs to settle first

- **ICP**: what the companies do (keywords, industries), where they are (locations) and how big they are (employee bands). Ask for the words customers use about themselves, not the user's category name.
- **Buyer titles**: two to five titles of the person who buys, and the seniority. Default: the titles on the user's last few deals.
- **Splits**: how to cut the market. Default: three employee bands by two regions, six segments.
- **Deal value**: annual contract value per account, for a value column. Optional.
- **Budget**: a default run of six segments costs about 6 x 10 + 6 x 1 = 66 credits, plus 20 if step 1 reads two customers' records. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Turn the ICP into filters.** `keywords[]`, `industries[]`, `locations[]` and `employee_ranges[]` (as "min,max", for example `employee_ranges: ["51,200"]`) on `leads_search_companies`. If the user has customers, `leads_get_company` on two of them (10 credits each) shows the provider's own `keywords[]` and `industries[]` for companies like theirs; those words match its search better than the user's.
2. **Count accounts.** `leads_search_companies` once per segment (10 credits a page, whatever the `limit`). Read `rows_available`; do not page. The first page is a free sample: read the first 20 names and count how many truly fit. That share is the precision of the filter.
3. **Count buyers.** People search has no size filter, so count per account. `leads_search_people` with the buyer `titles` and `company_domains` set to the up to 100 domains from each segment's first page (1 credit each). `rows_available` divided by the number of accounts searched is buyers per account; the share of rows with `has_email: true` is how many are reachable by email.
4. **Put numbers together.** For each segment: accounts that fit = `rows_available` x precision; buyers = accounts that fit x buyers per account; value = accounts that fit x deal value. The serviceable market is the segments the user can sell to now (language, time zone, product fit), not the sum of all.
5. **Deliver** a table: segment, filters, accounts (`rows_available`), precision, accounts that fit, buyers per account, buyers, reachable by email (share), value at the deal size, and three example companies. Add one line on which segment to start with and why.

## Judgment

- Count, do not fetch. A segment of 40,000 accounts costs 10 credits to count; revealing one buyer at each would cost 240,000. Never page through a search to size it.
- The provider's index is not the whole market. It is strongest on companies with a website and a LinkedIn page, and undercounts small local businesses, sole traders and markets outside English-speaking tech. Call the result a floor for those segments and say so.
- Keyword filters are loose. A precision under about 50 percent means the filter is measuring something else: tighten the keywords and count again (10 credits) rather than report the big number.
- Title matching is loose too. "Head of growth" returns growth marketers and growth-stage investors; read the sample titles before trusting buyers per account.
- Under about 500 accounts that fit, outbound is account-based: each account deserves research. Say so, and point to the [strategy](strategy.md).
- A public statistic (a census count, an analyst figure) is a useful cross-check. If it is ten times the count, the filter or the index is missing most of the market; say which is more likely.
- Search demand for the problem is a different measure: the `customers` group's [demand check](../../customers/references/demand-check.md). The players in the market are its [market map](../../customers/references/market-map.md).
