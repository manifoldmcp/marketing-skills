# Lead data

The rules for Manifold's `leads_*` tools, and for any list of real people. Every outbound job runs the same funnel: count, search, cut, reveal, verify. Searches and records are cheap and the reveal is not, so the list is cut on the search rows and only the shortlist is revealed. Checked against the tool code and live calls, October 2026.

## Where the data comes from

Companies and people come from Icypeas. One person's email comes from a waterfall that stops at the first hit: Findymail, then Icypeas, with Hunter only when named (`provider: "hunter"`). A domain's addresses come from Hunter. Verification of an address the user holds runs on Icypeas. No single finder clears about 71% coverage on B2B decision makers (Anymail Finder's June 2026 test of 5,000 contacts: Findymail 70.9%, Hunter 57.6%, Icypeas 49.0% at 99.1% accuracy), which is why the reveal asks several and charges only on a hit.

The index is strongest on companies with a website and a LinkedIn presence, and thin on small local businesses and on markets outside English-speaking tech. Say so whenever a count or a list is read as the market.

## Searches and counts

- `leads_search_companies` and `leads_search_people` cost 12 credits per 100 rows asked for in `limit`, rounded up: the default 100 is 12, 25 is 3, 1 is 1, and each further page by `cursor` is priced the same way. `rows_available` is the provider's total for the filters: read it to count instead of fetching (`limit: 1` is enough). Pass `cursor` from `meta.cursor` for the next page; no `cursor` back means the last page.
- `industries` are LinkedIn industry names ("Software Development", not "SaaS"); copy the string from a record. Some records carry the name in another language ("Développement de logiciels" in our run), so a strict industry filter misses them; a `keywords` search catches them.
- `keywords[]` on companies takes the category words an industry name misses. A row is kept only when a keyword appears in its name, industry, description or specialties, so a page can return fewer rows than `limit` while the charge counts the rows fetched, and `rows_available` is the total before that check. The match is loose: "SaaS" in our run returned HR services, insurance and hospitality firms beside software companies (15 of 40 rows were "Software Development"). Read the first 20 rows; if fewer than about 15 fit, change the filters before going on.
- `titles` and `seniority` combine into phrases: `titles: ["Marketing"]` with `seniority: ["executive"]` searches "VP Marketing", "Head of Marketing" and the like. Pass full titles ("head of growth") with no `seniority`, or bare functions with it; a full title plus a seniority searches phrases nobody holds. `keywords` on people is one string: a job-title word with no `titles` is matched against the current title, other words against the whole profile.
- Accounts before people. A people search across a whole industry returns thousands of loose title matches; a search inside 100 accounts that fit returns buyers. In our run, 40 US software-ish companies of 51-200 staff yielded 18 marketing leaders at 15 of them: expect a buyer at about a third to a half of small accounts, not all.

## What the records hold

- **Person row:** real name, `headline`, `title`, `title_start` (when the current role began), `company`, `company_domain`, the company's headcount and industry, `location` and `linkedin_url`; its `id` is the LinkedIn profile URL, or null when the person has no profile (look them up by `first_name`, `last_name` and `domain`).
- **Company row:** `industry`, `employees`, `headcount_growth[]`, `location`, `description`, `keywords[]` (LinkedIn specialties), `revenue_min` and `revenue_max` (an estimated range) and `phone`. `headcount_growth[]` was filled for 6 of 40 rows in our run: absence is not "no growth".
- `leads_get_company` (1 credit, cached 30 days) adds `revenue` where disclosed. `leads_get_person` (9 credits when matched by profile URL, 3 by name and domain, 1 on `NoData`) adds `employment_history[]`, `education[]`, `skills[]` and `summary` when looked up by profile URL; by name and domain the history is the current role only. Any `id` that is not a full https://www.linkedin.com/in/<public-id> URL is `InvalidTarget` at no charge.
- **Empty though the fields exist:** a company's `total_funding`, `funding_stage` and `technologies[]`, and a person's `seniority` and `has_email`. Build no filter or step on them. Funding and new leaders come from posts and news ([signals](signals.md)); the tech stack is not visible to any tool here.
- **Records lag.** A `headline` can name the previous employer for months after `title` and `title_start` show the new one (one of 18 rows in our run). Trust `title_start` over the headline, and the live LinkedIn profile over both when they disagree.
- **A person's `location` is not the company's.** Six of 18 marketing leaders at US-headquartered companies lived in Canada, the UK, Israel, India or Hong Kong. The legal regime follows the person ([legal](legal.md)), so filter or flag on the person's `location`.
- Junk rows exist (a profile with the headline "test"). Drop rows with a one-letter name or no headline.

## Emails

- **Reveal last, only for people worth contacting.** `leads_get_email` costs 7 credits on a hit by name and domain, 31 with `provider: "hunter"`, and 1 on `NoData`, cached 90 days. Misses are not cached, so asking again pays again for the same answer: never retry a `NoData`. A source that does not answer in time is skipped; when none answers the call is `ProviderUnavailable` at no charge, so retry that later.
- Pass the row's `first_name`, `last_name` and `company_domain` as `domain`, with or without its `id`. With an `id` alone, the reveal first looks the person up in Icypeas to get the name and domain (9 credits more, 16 on a hit), so `meta.providers_tried` starting with icypeas is that lookup, not a failed finder.
- Reveal the first 10 and count the hits before buying more. Under 5 hits means the titles are too junior or the companies too small for the sources' coverage; stop and say so.
- Person records never carry an email. With no name (anyone at a small firm), `leads_get_domain_emails` lists a domain's known addresses (1 credit a domain plus 30 for each domain that returns addresses, whatever the `limit`, cached 30 days) with `position`, `department`, `type` (personal or generic) and `confidence`; for a founder, pass `department: ["executive"]` and `limit: 10`. Its addresses come back mostly unverified. Contacts at publishers for links or coverage are a different job: [link-building](../../link-building/SKILL.md).

## Verification

- **Two verdicts, two sources.** Findymail returns only addresses it has verified, so a Findymail hit comes back `valid` on its own word; a hit with an `unknown` status gets a second check from the Icypeas verifier before it is returned. `leads_get_email_status` (1 credit, cached 30 days) is the Icypeas verifier. In our run it agreed on 8 of 10 Findymail `valid` reveals, called one `accept_all` and could not answer one. Before the first sends from a new domain, re-check reveals for 1 credit each.
- **Read the status:**
  - `valid`: send.
  - `accept_all`: the server takes any address, so this one is unproven. About a third of B2B contacts sit on such domains. Keep it, marked, and send it after the verified rows, in small batches, stopping if it bounces.
  - `unknown`: the check got no answer. When `mx_records`, `score` and `mail_provider` are all null too, the verifier heard nothing back at all, not a refusal. It does not improve with a quick retry (cached 30 days); treat it like `accept_all`.
  - `invalid`, or `mx_records: false`: drop. An address that went invalid usually means the person left; finding them again is a [job-change](signals.md#job-changes) question.
- `mail_provider` says who runs the domain's mail: google, microsoft, other, or a security gateway such as mimecast. Gateways (Proofpoint, Mimecast, Barracuda) filter cold mail harder; batch those domains apart and watch them.
- The verifier does not report `disposable` or `webmail` (always null). Flag by domain: webmail (gmail.com, outlook.com, hotmail.com, yahoo.com, icloud.com) is a personal address and stays off a B2B cold list; disposable domains are dropped. Flag role addresses (info@, sales@, hello@, support@) too: they reach a shared inbox, reply less and draw more complaints.
- Re-check a revealed address that came back `cached: true` and is more than a month old, and re-verify any list that waits more than about a month before its send: people change jobs.

## Personal data

- These are records of real people. Collect only the work data the user has a reason to use for this outreach: name, title, company, work email, LinkedIn URL. No personal addresses, phone numbers or private life in a list, a line or a brief.
- Keep a source column in every table (the search, the signal, `found_by`, the date), so the user can answer "where did you get my address", which UK and EU recipients are entitled to ask.
- Flag lists with people in the EU, the UK or Germany and point to [legal](legal.md). The user decides the legal basis; this is not legal advice.

## Credits

- Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop at `BudgetExceeded`. `dry_run: true` prices any call for free.
- Funnel prices: count or search 12 per 100 rows asked for (1 at `limit: 1`), company record 1, person record 3 by name and domain or 9 by profile URL (1 on a miss), reveal 7 (1 on a miss), verify 1, domain search 1 plus 30 when it returns addresses. A 50-contact list costs about 408: two company searches, two people searches, 50 reveals and ten re-checks.
- A result this account already paid for is free while cached: people searches 7 days; company searches, company and person records, domain searches and verifications 30 days; reveals 90 days. Another account's cached result costs a quarter of the price.
- The leads tools take one company or person per call. Run a few at a time; on `ConcurrencyLimit`, wait `retry_after_s` and send fewer.

## Handoff

- Never send, sequence, connect on LinkedIn or write to a CRM. The deliverable is a table with plain headers that pastes into a sequencer or CRM import (first_name, last_name, title, company, company_domain, email, verification_status, linkedin, location, source, notes). If the host has a sequencer or CRM tool, offer to pass the table; do not send.
- The user's data is the record. When filling or checking their sheet, never overwrite a cell they typed: new values and statuses go in new columns.
- The server keeps no state. The host keeps the list, the exclusions (customers, open deals, people already contacted, opt-outs) and the date of each reveal.
