---
name: find-lookalike-companies
description: When the user wants more companies like their best customers. Reads the company records of the best customers, finds the LinkedIn industry, employee band, region and specialties they share, searches for companies with the same traits and ranks them by how many they match, as an account list. Also use when the user mentions lookalike accounts, lookalike companies, companies like our best customers, more companies like Acme, similar companies to our top accounts, or a lookalike account list. The people and emails at those accounts go to build-lead-list, how many companies fit the ICP to size-market, accounts with a reason to buy now to find-buying-signals, competitors to find-competitors.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Lookalike companies

The best predictor of the next customer is the last good one. This skill reads the records of the user's best customers, finds what they share, searches for companies with the same traits and ranks them by how many they match. It ends in an account list ready for [build-lead-list](../build-lead-list/SKILL.md).

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_get_company` and `leads_search_companies` (hosts often add a prefix, for example `mcp__manifold__leads_get_company`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it this skill cannot read or search companies.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the best customers, the ICP, the countries) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Best customers**: three to ten domains. The best by fit (bought fast, stayed, paid well), not the biggest logos: a famous customer that took a year to close teaches the wrong pattern.
- **Exclusions**: current customers, open deals and accounts already contacted, as domains. The host holds them; the server keeps no state.
- **Market**: the regions to search. Default: the regions the customers are in.
- **Size of the list**: default up to 300 candidates, the best 60 read in full, the best 20 delivered.
- **Budget**: a default run costs about 5 x 1 + 3 x 4 + 60 x 1 = 77 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Profile the customers.** `leads_get_company` on each best customer (1 credit each). Read `industry`, `keywords[]` (the LinkedIn specialties), `employees`, `location`, `founded_year` and `description`. Write down what most of them share: the `industry` strings, the employee band that covers them, the regions, and specialties that appear on three or more records.
2. **Search the pattern.** `leads_search_companies` with the shared `industries` (the exact strings from step 1), `employee_ranges` and `locations` (4 credits a page of up to 100 rows). Add `keywords` with a specific specialty three or more customers share; it keeps only rows that name it, so drop it if the page comes back thin. When the customers span two or three industries or regions, one call per combination, up to three. Remove the exclusions and the customers themselves by domain. The [search rules](../create-outbound-plan/references/lead-data.md#searches-and-counts) apply.
3. **Cut on the rows.** The rows carry `industry`, `employees` and `location`: drop the ones outside the band or the regions, for free. Keep the best 60 by how close they sit to the customers (same industry string first, then the band).
4. **Read the shortlist.** `leads_get_company` on the 60 (1 credit each) for `keywords[]` and `description`. Score each one against the customer profile, one point per match: two or more shared specialties, industry, employee band, region, and a description that names the same kind of customer or problem as the customers' do. Drop anything that misses on two of the first four.
5. **Deliver** a table of the best 20: company, domain, LinkedIn URL, industry, employees, location, the traits matched (for example "4 of 5: payment orchestration, Financial Services, 51 to 200, London"), the specialties it shares with the customers, the customer it most resembles, and the score. Offer to run [build-lead-list](../build-lead-list/SKILL.md) on it from its people step.

## Judgment

- Three good customers make a pattern; one does not. With fewer than three, say that the list is a guess and ask which traits the user thinks matter.
- A trait every company in the market has (the country, "Software Development") does not separate anyone. Drop it from the scoring and keep the ones that set the customers apart.
- Shared specialties are strong when specific ("payment orchestration") and weak when generic ("SaaS"). Companies type them on LinkedIn themselves, and small ones often leave them empty: score the `description` instead.
- LinkedIn industries are self-chosen. A customer filed under an odd industry gets its own search, not a place in the pattern.
- A lookalike of the current customers repeats their bias. If the user wants a new segment, that is a [size-market](../size-market/SKILL.md) question first.
- The provider's index is not the whole market: small firms with little web presence are underrepresented, so a thin result in a local or offline market does not mean there are no lookalikes.
- A result this account already paid for is free while cached (company searches and records 30 days); see the [credit rules](../create-outbound-plan/references/lead-data.md#credits).

## Related skills

- The buyers and their emails at the accounts: [build-lead-list](../build-lead-list/SKILL.md).
- Which of the accounts show a reason to buy now: [find-buying-signals](../find-buying-signals/SKILL.md).
- How many companies fit the ICP, by segment: [size-market](../size-market/SKILL.md).
- Who to target and how, as a 90-day plan: [create-outbound-plan](../create-outbound-plan/SKILL.md).
- The companies the user competes with, rather than sells to: [find-competitors](../find-competitors/SKILL.md).
