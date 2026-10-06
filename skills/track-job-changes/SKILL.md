---
name: track-job-changes
description: When the user wants to know which past champions, buyers or power users moved to a new company. Checks each known person's record by LinkedIn URL against the last known company, sizes the companies the movers joined, dates each move from LinkedIn posts and reveals a verified email for the movers worth contacting. Also use when the user mentions job changes, champion tracking, which of our champions changed jobs, track job changes of our customers' contacts, former users who moved companies, or where did our old buyers go. New buyers who just started at target accounts go to find-buying-signals, filling the empty columns of a spreadsheet to enrich-lead-list, a fresh list of people by title to build-lead-list.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Job changes

A champion, buyer or power user who moves to a new company is the warmest outbound there is: they know the product and they are choosing tools in a new seat. This skill checks people the user already knows for a move, one LinkedIn URL at a time, sizes the companies they moved to and reveals an address for the movers worth contacting. It ends in a table of movers with the reason to write to each.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_get_person` and `leads_get_email` (hosts often add a prefix, for example `mcp__manifold__leads_get_person`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it this skill cannot find or verify a person.
- This skill also reads `linkedin_*` to date a move. If that group is switched off, skip that step and say the moves are undated.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the ICP the new company has to fit, the countries) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **People**: the user's champions, buyers and power users, from the CRM or the product: name, the company they were at, and the LinkedIn URL. Default: the contacts on closed-won deals and the most active users. Without a LinkedIn URL the check can only say whether someone is still at the old company.
- **Last known**: the company domain and title the user has on file for each person, which the host keeps between runs.
- **ICP**: whether the new company has to fit (industry, size, region) before the user reaches out. Default: the ICP from the product context.
- **Budget**: a default run of 200 people with 20 movers costs about 200 x 3 + 20 x 1 + 20 x 1 + 20 x 6 = 760 credits: a check per person (1 instead of 3 for anyone not found), then a company record, a post search and a reveal per mover. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Prepare the list.** The host reads the people, dedupes them by LinkedIn URL and leaves out anyone marked do not contact.
2. **Check each person.** `leads_get_person` with `linkedin_url` as the full profile URL, https://www.linkedin.com/in/<public-id> (3 credits when matched, 1 on `NoData`; any other form is `InvalidTarget` at no charge). Compare its `company_domain` and `company` with the last known company:
   - The same: still there. A new `title` is a promotion, also a reason to write.
   - Different: a mover. Another domain of the same `company` (a country site, a rebrand) is not a move.
   - `NoData`: not in the index. Mark it "not found" and do not retry.
   For a person with no LinkedIn URL, use `first_name`, `last_name` and the old `domain`: a match means still there; `NoData` means unknown, so list them for the user to find the profile. Run a few checks at a time; on `ConcurrencyLimit`, wait `retry_after_s`.
3. **Size the new companies.** `leads_get_company` once per new `company_domain` (1 credit): `industry`, `employees`, `location`, `description`. Mark whether it fits the ICP. A new company that is already a customer is an expansion lead for its account owner, not a new account.
4. **Date the move**, for the movers that fit. `linkedin_search_posts` with the person's full name as `query` and `since: "month"` (1 credit), keeping rows whose `author` is their profile handle: a "starting a new position" post dates it. Without one, say the move happened since the last check.
5. **Reveal.** For the movers at companies that fit, `leads_get_email` with `id` set to their LinkedIn URL (6 credits on a hit, 1 on `NoData`), and verify by the [email rules](../create-outbound-plan/references/lead-data.md#emails).
6. **Deliver** a table: name, LinkedIn URL, relationship (champion, buyer, user), old company and title, new company, domain and title, fits the ICP, move dated (post link, or "since last check"), email, verification_status, and the reason to write ("ran X at Old Co for two years"). Below it, the people still in place with a new title, the customer accounts that lost their champion, and the people not found. Hand the new companies and titles back for the host to store as the last known.

## Judgment

- Write as someone they know. The shared history is the reason for the email; a mover dropped into a cold sequence wastes it.
- Timing matters. As a rule of thumb, people bring in tools in their first three months in a seat; after about six months the move is background.
- The other side of a move is a risk: a customer account whose champion left needs a new champion. Flag it to the user as churn risk.
- Records lag a move by weeks, so "still there" can be wrong. Re-check the list every quarter; a person record is cached 30 days, so a re-check inside a month is free and no newer.
- A former champion is still a person whose data needs a reason; lists covering the EU or the UK follow the [personal data](../create-outbound-plan/references/lead-data.md#personal-data) rules.
- Never send, sequence or write to a CRM; see the [handoff rules](../create-outbound-plan/references/lead-data.md#handoff).
- Checking the same people every month runs on the host's schedule: the host runs this skill again and keeps the last known company, since the server keeps no state.

## Related skills

- New leaders who just started in the buyer's seat at target accounts, and other reasons to buy now: [find-buying-signals](../find-buying-signals/SKILL.md).
- An opening line for each mover: [write-first-lines](../write-first-lines/SKILL.md). Prep for a call at the new company: [research-account](../research-account/SKILL.md).
- The user's whole CRM export filled in or verified: [enrich-lead-list](../enrich-lead-list/SKILL.md) and [clean-email-list](../clean-email-list/SKILL.md).
- Where job changes fit in an outbound plan: [create-outbound-plan](../create-outbound-plan/SKILL.md).
