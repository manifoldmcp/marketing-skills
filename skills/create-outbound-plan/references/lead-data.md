# Lead data

The rules every outbound skill shares for the `leads_*` tools. Every job runs the same funnel: count, search, cut, reveal, verify. Searches and records are cheap and the email reveal is not, so the list is cut on the search rows and only the shortlist is revealed.

## Searches and counts

- `leads_search_companies` and `leads_search_people` cost 4 credits a page of up to 100 rows. `rows_available` is the provider's total for the filters: read it to count instead of fetching. When more rows are needed, pass `cursor` from `meta.cursor` for the next page (4 credits a page); no `cursor` back means the last page.
- Filters: `industries` are LinkedIn industry names, matched exactly ("Software Development", not "SaaS"): copy the `industry` string from a record. Then `locations`, `employee_ranges` as "min,max", and for people `titles`, `seniority` and `company_domains`.
- Keywords: `keywords[]` on companies takes the category words an industry name misses ("payments", "SaaS"). A row is kept only when a keyword appears in its name, industry, description or specialties, so a page can return fewer rows than `limit` while the charge counts the rows fetched, and `rows_available` is the total before that check. `keywords` on people is one string: a job-title word ("marketing", "sales") with no `titles` is matched against the current title; other words against the whole profile.
- `titles` and `seniority` combine into phrases: `titles: ["Marketing"]` with `seniority: ["executive"]` searches "VP Marketing", "Head of Marketing" and the like. Pass full titles ("head of growth") with no `seniority`, or bare functions with it; a full title with a seniority searches phrases nobody holds.
- Rows are complete enough to decide on. A person row carries the real name, `title`, `company`, `company_domain`, `location` and `linkedin_url`, and its `id` is the LinkedIn profile URL. A company row carries `industry`, `employees`, `location` and `linkedin_url`. `leads_get_company` (1 credit) adds `description`, `keywords[]` (its LinkedIn specialties) and `phone`; `leads_get_person` (3 credits when matched, 1 on `NoData`) adds `headline`; its `id` is the full LinkedIn profile URL from the row (https://www.linkedin.com/in/<public-id>), and anything else is `InvalidTarget` at no charge. A row whose `id` is null has no profile: look it up by `first_name`, `last_name` and `domain`.
- Not in the data, though the fields exist: a company's `revenue`, `total_funding`, `funding_stage` and `technologies[]`, and a person's `seniority`, `employment_history[]` and `has_email`. They come back empty; build no step or filter on them. Funding rounds and new leaders come from posts and news in [buying signals](../../find-buying-signals/SKILL.md).
- The provider's index is not the whole market. It is strongest on companies with a website and a LinkedIn presence, and thinner on small local businesses and on markets outside English-speaking tech. Say so whenever a count or a list is read as the market.

## Emails

- Reveal last, and only for people worth contacting. `leads_get_email` is the paid reveal: 6 credits on a hit, 1 on `NoData`. Do not retry a `NoData`: misses are not cached, so asking again pays again for the same answer. A source that does not answer in time is skipped; `meta.providers_tried` lists the sources asked, and when none answers the call is `ProviderUnavailable` at no charge, so retry it later.
- Pass the row's `id` (its LinkedIn URL) to `leads_get_email`, or `first_name`, `last_name` and `domain`. Whether an address exists is unknown until the reveal.
- Person records never carry an email. With no name, only a company (anyone at a small firm), the address comes from a domain search: run the [contact steps](../../create-link-building-plan/references/outreach.md#contact-steps) with the departments the buyer sits in.
- Verify before any send. `leads_get_email` verifies as it finds, so read its `verification_status`. Every other address (the user's file, a pattern guess, a domain search) gets `leads_get_email_status` (1 credit). Re-check a revealed address that came back `cached: true`: a reveal is cached 90 days and people change jobs. Read the status as the contact steps do: drop `invalid`, keep `accept_all` marked unproven, keep `unknown` with a note.

## Personal data

- These are records of real people. Collect only work data the user has a reason to use for this outreach: name, title, company, work email, LinkedIn URL. No personal addresses, phone numbers or private life in a list or a line.
- For people in the EU and the UK, cold B2B email rests on a legitimate interest the user can state, an opt-out in every message and a record of where the data came from. Some countries, Germany among them, want consent before any marketing email, B2B included. Say this once when a list covers those markets; the user decides the legal basis, and this is not legal advice.
- Keep a source column in every table (the search, the signal, `found_by`), so the user can answer "where did you get my address".
- An address at a webmail domain (gmail.com, outlook.com, hotmail.com, yahoo.com, icloud.com and the like) is a personal one. Flag it by its domain, since the verifier does not report it, and do not add it to a cold list.

## Credits

- Say the estimate before the first paid call; each skill gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Spend in funnel order: count (4), search (4 a page), company record (1), person record (3, 1 on a miss), reveal (6, 1 on a miss), verify (1). Never reveal an email for a row a cheaper step could have dropped.
- A result this account already paid for is free while cached: people searches 7 days, company searches, company and person records and verifications 30 days, reveals 90 days. A second pass over the same accounts costs little.
- The leads tools take one company or person per call. For a long list, run a few calls at a time; on `ConcurrencyLimit`, wait `retry_after_s` and send fewer at once.

## Handoff

- Never send, sequence, connect on LinkedIn or write to a CRM. The deliverable is a table with plain column headers that pastes into a sequencer or a CRM import. If the host has a sequencer or CRM tool, offer to pass the table to it; do not send.
- The server keeps no state. The host keeps the list, the exclusions (customers, open deals, people already contacted) and the date of each reveal.
- Write outreach copy only when the user asks. [Cold email first lines](../../write-first-lines/SKILL.md) is the one skill that writes, and it writes one line per lead.
