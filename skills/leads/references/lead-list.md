# Lead list

A lead list is accounts first, then the buyers at them, then an address for the few worth contacting. Search is cheap and the reveal is not, so the list is cut on the stubs and only the shortlist is filled, revealed and verified. It ends in a CSV-ready table for the user's sequencer or CRM.

## Inputs to settle first

- **Accounts**: the ICP as company filters (keywords, industries, locations, employee bands), or a list of domains the user already has, or the output of [lookalike companies](lookalike-companies.md) or [buying intent](buying-intent.md).
- **Buyers**: three to six title variants of the person who buys ("head of growth", "VP marketing", "director of demand generation") and the seniority. Default: the economic buyer plus one likely champion.
- **Size**: contacts wanted and the cap per account. Default: 50 contacts, at most two per account.
- **Exclusions**: customers, open deals and people already contacted, as domains and emails. The host holds them.
- **Fields**: whether the user needs LinkedIn URLs and locations, which cost a person fill each. Default: no.
- **Budget**: a default run of 50 contacts costs about 2 x 10 + 2 x 1 + 50 x 6 + 10 x 1 = 332 credits: two company pages, two people searches, 50 reveals and a few re-checks. With LinkedIn URLs, add 50 x 10 = 500. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Accounts.** `leads_search_companies` with the ICP filters (10 credits a page of 100), or take the user's domains. Read the first 20 names: if fewer than about 15 fit, tighten the keywords before going on. Remove the exclusions by domain. When a field the search cannot filter decides fit (`funding_stage`, `technologies[]`, `revenue`), `leads_get_company` (10 credits) on the candidates settles it; otherwise do not fill companies at all.
2. **People.** `leads_search_people` with the buyer `titles`, `seniority` and `company_domains` set to up to 100 account domains per call (1 credit a page of 100). Page with `cursor` only while the rows keep fitting. Stubs show `title`, `company` and `has_email`; the last name is masked, which does not matter yet.
3. **Shortlist.** At most two people per account: the title closest to the buyer first, then the champion. Prefer `has_email: true`. Drop titles that only match the words ("growth investor" for "growth"). Cut to the size the user asked for.
4. **Small accounts with no match.** Where the search finds no buyer at a small company, the founder or a generic address is the contact: run the [contact steps](../../link-building/SKILL.md#contact-steps) with `department: ["executive"]` and `limit: 10`.
5. **Fill, only if asked.** `leads_get_person` with the stub's `id` (10 credits) when the user wants LinkedIn URLs, locations or a check of the current role. Skip it otherwise: the reveal in step 6 returns the real name.
6. **Reveal.** `leads_get_email` with the stub's `id` (6 credits on a hit, 1 on `NoData`). It fills the name and domain from the id and returns `email`, `verification_status` and `found_by`. Do not retry a `NoData`; mark the row "no email found".
7. **Verify.** Follow the [router's email rules](../SKILL.md#emails): drop `invalid`, keep `accept_all` marked unproven, keep `unknown` with a note, and re-check with `leads_get_email_status` (1 credit) any reveal that came back `cached: true`.
8. **Deliver** a CSV-ready table with plain headers: first_name, last_name, title, company, company_domain, email, verification_status, linkedin (the profile URL, if filled), location (if filled), source (the search or signal that put the row there), and notes (accept_all, no email found, second contact at the account). Give the counts: accounts searched, people found, revealed, verified, and credits spent. Do not send; the host's sequencer or CRM import takes the table.

## Judgment

- Accounts before people. A people search across a whole industry returns thousands of loosely matched titles; a search inside 100 accounts that fit returns buyers.
- Two people per account at most in one sequence. Five people at one company getting the same email in the same week reads as spam, and they compare notes.
- `has_email: false` is not a dead end. The waterfall behind `leads_get_email` checks other sources; expect a lower hit rate and a 1-credit miss.
- A reveal that comes back `accept_all` is common at large companies, whose mail servers accept anything. Keep those rows, send them last and in small batches.
- If the hit rate falls under about half, the titles are too junior or the companies too small for the provider's coverage. Say so rather than buying more misses.
- Lists covering the EU or the UK follow the router's [personal data](../SKILL.md#personal-data) rules.
- A contact at a website about a link is not a lead: that is the [link-building](../../link-building/SKILL.md) group.
