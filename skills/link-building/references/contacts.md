# Finding a contact at a site

How to find the person who can add a link, a list entry or a correction at any website, and prove the address works. This is the one copy of these steps: AI citations, digital PR, lists, creators and every link job here use it. Rules on personal data and email verification in general live in [outbound](../../outbound/SKILL.md). Checked October 2026.

## Who to look for

- **The key is the site's domain, not a job title.** Use `leads_get_domain_emails`, never `leads_search_people`, which finds people for sales by role and returns no email.
- **The writer beats the inbox.** When the page names its author (a byline, an "about the author" box) and the host can read the page, look that person up first: the writer of the page adds a link faster than anyone else. On a publication, editors usually sit under `communication`; on a small site, the owner sits under `executive`.
- **Small sites often have only a generic address** (`hello@`, `editor@`, `press@`). It usually reaches the owner and is fine to use.

## Steps

1. **Domain search, shortlist only.** `leads_get_domain_emails` with up to 20 domains per call, `department: ["marketing", "communication"]` and `type: "all"`. Cost: 1 credit a domain plus 30 for each domain that returns addresses, whatever the `limit`: 31 a domain with addresses. Choose in this order: a named editor or content lead with `verification_status: valid`; any named person in those departments; a generic address. If a domain has nothing in those departments, call it once more with no `department`. A domain that answers `NoData` (1 credit) gets no further call: keep the row as "no contact found; use the site's contact page".
2. **A named person the search missed.** `leads_get_email` with `first_name`, `last_name` and `domain`: 7 credits on a hit, 1 on `NoData`. Never retry a `NoData`. `domains[].pattern` from step 1 shows the address format, but an address built from it is unproven until step 3.
3. **Verify the one address per site you would send to**, never the whole list: `leads_get_email_status`, 1 credit each. Drop `invalid`. Keep `accept_all` and mark it unproven (the server accepts anything, so nothing there can be proven). Keep `unknown` with a note: the mail server refused the check.

A typical shortlist of 20 sites costs about 620 credits for contacts and 20 for verification, more than all the research before it. Find contacts last.

## Outreach evidence

There is no current, independent benchmark for link outreach replies. What exists:

- **8.5%** of 12M outreach emails got a reply (Backlinko with Pitchbox, 2019). Roundups in 2025 and 2026 still quote it, often credited to Authority Hacker; it is seven years old.
- **4.1%** average reply rate across 11M cold emails sent in 2024 (Hunter, State of Cold Email 2025). Campaigns to 50 or fewer recipients averaged 5.8%, campaigns to 1,000+ averaged 2.1%; campaigns with follow-ups 4.9% against 3% without; unsubscribes climb after the third follow-up. General cold email, not link outreach, but the shape holds: small, targeted lists and one or two follow-ups.
- In BuzzStream's 2026 digital PR survey (150+ practitioners), 55% follow up once and 33% twice, and 61% say relevant contacts are harder to find than a year ago. Muck Rack's 2025 journalist survey: 48% of journalists seldom or never get a relevant pitch.
- **Warm asks convert far better than cold ones.** An unlinked mention, a lost link or a correction is a one-line ask to someone who already chose to name the brand; no study measures the rate, but practitioners treat these as the highest-yield outreach there is.

Set volume from the team's hours, not the list's length: 20 well-matched sites with a reason each beat 200 scraped ones.

## The ask

- **Write copy only when the user asks**, then one short email per row (under about 120 words) that names the page, the exact place a link fits and why it helps that page's readers.
- **Never offer payment for an undisclosed link or mention**, and never agree to a fee on the user's behalf; flag the request and let the user decide (see [spam and paid links](spam-and-paid-links.md)).
- **Never send, post or fill in a form.** The deliverable is a table: site, page, domain rank, contact name, role, email, verification status, the ask in one line. If the host has an email or sequencer tool, offer to hand the table over; do not send.
- **The server keeps no state.** To track replies or rerun next month, the host keeps the table with the date and the credits spent.

## Without the connector

By hand: the site's contact or "write for us" page, the author's bio and social profiles, the masthead or staff page for a publication. Say plainly that an address guessed from a name pattern is unproven, and suggest the user verify it with whatever verifier they have before sending.
