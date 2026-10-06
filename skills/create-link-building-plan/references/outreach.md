# Outreach

What every skill that ends in a list of sites with a person at each shares: the floors a site must clear, the steps that find and verify a contact, the credits and the handoff. Skills link here instead of repeating them.

## Floors

- **Domain rank.** Keep a site only when its `domain_rank` is above about 20 on DataForSEO's 0 to 1000 scale; below that a link moves nothing. For backlink targets, also keep it only when it is above the user's own domain rank from `seo_get_domain_overview`. Lists, publications and partners qualify by where they rank and who reads them, not by that second test.
- **Spam.** Skip any referring domain with a `spam_score` above 30, and skip directories, link farms, scraper sites and aggregators whatever their rank.
- **Relevance beats rank.** A site that covers the user's topic at domain rank 150 beats an unrelated site at 600. Give one line of evidence (a page title, a URL) for why each site fits.

## Contact steps

These are the only copy of the contact mechanics. Other skills link here instead of repeating them.

1. **Find a contact.** Call `leads_get_domain_emails` with up to 20 kept domains per call, `department: ["marketing", "communication"]` (unless the skill names other departments) and `type: "all"`. It costs 2 credits per domain plus 6 per 10 addresses returned: about 20 credits a domain at the default 30 addresses, or 8 with `limit: 10`, which is enough when you need one contact. Choose, in this order: a named editor or content lead with `verification_status: valid`; any named person in those departments; a generic address (`editor@`, `press@`, `hello@`). Small sites often have only the generic one, and it usually works. If a domain has nothing in those departments, call it once more with no `department` (on a one-person site the owner sits under `executive`). A domain that returns `NoData` (1 credit) gets no other call: keep the row as "no contact found" for the user to reach through the site's contact page.
2. **Named person, no address.** When you know the person (a byline, a LinkedIn author) and the domain search does not have them, call `leads_get_email` with `first_name`, `last_name` and `domain`: 6 credits on a hit, 1 on `NoData`. Do not retry a `NoData`. `domains[].pattern` from the domain search shows the address format, but a built address is unproven until step 3.
3. **Verify the shortlist.** Call `leads_get_email_status` on the one address per site you would actually send to, never on the whole list (1 credit each). Drop `invalid`. Keep `accept_all` and mark it unproven. `unknown` means the mail server refused the check: keep it with that note.

## Credits

- Say the estimate before the first paid call; each skill gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Rate in bulk and early: `seo_get_domain_ratings` takes up to 1,000 domains for 1 credit plus 2.5 per 100. Never rate domains one at a time with `seo_get_domain_overview`.
- Find contacts last, and only for the shortlist. Contacts cost more than everything before them.
- A result this account already paid for is free while it is cached (7 days for backlink data, 30 days for domain emails), so a second, wider pass costs little.

## Handoff

- Never send, post, buy links or fill in forms. The deliverable is a table. If the host has an email or sequencer tool, offer to pass the table to it; do not send.
- Write outreach copy only when the user asks, and then one short pitch per row that names the page that fits.
- The server keeps no state. If the user wants to track replies or run the job again next month, the host keeps the table.
