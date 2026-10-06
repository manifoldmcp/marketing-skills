---
name: size-market
description: When the user wants to know how many companies and buyers fit their ideal customer profile. Counts accounts and buyers per segment from the lead provider's totals rather than fetching them, and returns a segment table with accounts, precision, buyers per account and a value at the deal size. Also use when the user mentions TAM in accounts, market size, how many companies fit our ICP, how many heads of HR in German logistics, is the market big enough for outbound, or counting the buyers in a market. Search demand for the problem goes to check-demand, the players and segments of a market to map-market, the people themselves with emails to build-lead-list, a full outbound plan to create-outbound-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Market sizing

How many companies fit the ICP, and how many buyers sit in them, counted from the provider's totals rather than fetched row by row. It ends in a table of segments with accounts, buyers and a value, which tells the user whether outbound has enough room and where to start.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_search_companies` and `leads_search_people` (hosts often add a prefix, for example `mcp__manifold__leads_search_companies`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it this skill cannot count companies or buyers.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the ICP, the best customers, the personas and job titles, the countries, the price points) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **ICP**: what the companies do (industries), where they are (locations) and how big they are (employee bands). Ask for one or two customer domains: their records give the exact industry names the search matches.
- **Buyer titles**: two to five full titles of the person who buys. Default: the titles on the user's last few deals.
- **Splits**: how to cut the market. Default: three employee bands by two regions, six segments.
- **Deal value**: annual contract value per account, for a value column. Optional.
- **Budget**: a default run of six segments costs about 6 x 4 + 6 x 4 = 48 credits, plus 2 if step 1 reads two customers' records. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Turn the ICP into filters.** `industries[]`, `locations[]` and `employee_ranges[]` (as "min,max", for example `employee_ranges: ["51,200"]`) on `leads_search_companies`. Industries are LinkedIn's names, matched exactly: if the user has customers, `leads_get_company` on two of them (1 credit each) shows the `industry` string to copy. The user's category word ("SaaS", "fintech") is not one. The [search rules](../create-outbound-plan/references/lead-data.md#searches-and-counts) give the rest of the filters.
2. **Count accounts.** `leads_search_companies` once per segment (4 credits). Read `rows_available`; do not page with `cursor`, the count needs none. The rows are a free sample: read the first 20 names and count how many truly fit. That share is the precision of the filter.
3. **Count buyers.** People search has no size filter, so count per account. `leads_search_people` with the buyer `titles` and `company_domains` set to the up to 100 domains from each segment's sample (4 credits each). `rows_available` divided by the number of accounts searched is buyers per account.
4. **Put numbers together.** For each segment: accounts that fit = `rows_available` x precision; buyers = accounts that fit x buyers per account; value = accounts that fit x deal value. The serviceable market is the segments the user can sell to now (language, time zone, product fit), not the sum of all.
5. **Deliver** a table: segment, filters, accounts (`rows_available`), precision, accounts that fit, buyers per account, buyers, value at the deal size, and three example companies. Add one line on which segment to start with and why.

## Judgment

- Count, do not fetch. A segment of 40,000 accounts costs 4 credits to count; revealing one buyer at each would cost 240,000.
- The provider's index is not the whole market. It is strongest on companies with a website and a LinkedIn page, and undercounts small local businesses, sole traders and markets outside English-speaking tech. Call the result a floor for those segments and say so.
- LinkedIn industries are self-chosen and broad. A precision under about 50 percent means the filter is measuring something else: try a neighbouring industry name or a narrower band and count again (4 credits) rather than report the big number.
- Title matching is loose too. "Head of growth" returns growth marketers and growth-stage investors; read the sample titles before trusting buyers per account.
- Under about 500 accounts that fit, outbound is account-based: each account deserves research. Say so, and point to [create-outbound-plan](../create-outbound-plan/SKILL.md).
- A public statistic (a census count, an analyst figure) is a useful cross-check. If it is ten times the count, the filter or the index is missing most of the market; say which is more likely.
- Search demand for the problem is a different measure: [check-demand](../check-demand/SKILL.md). The players in the market are [map-market](../map-market/SKILL.md).

## Related skills

- Who to target and how, as a 90-day plan built on these counts: [create-outbound-plan](../create-outbound-plan/SKILL.md).
- The accounts in a segment that most resemble the best customers: [find-lookalike-companies](../find-lookalike-companies/SKILL.md). The buyers there with emails: [build-lead-list](../build-lead-list/SKILL.md).
- Search demand for the problem: [check-demand](../check-demand/SKILL.md). The players and segments of the market: [map-market](../map-market/SKILL.md).
- A new country or segment to enter: [create-market-entry-plan](../create-market-entry-plan/SKILL.md).
