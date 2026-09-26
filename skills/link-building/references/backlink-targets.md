# Backlink targets

Backlink outreach is "who at these sites could I email about a link", not "who fits an ICP". The key is a domain, so the contact tool is `leads_get_domain_emails`, never `leads_search_people`.

## Inputs to settle first

- **Target**: the user's site, or the page that wants links.
- **Prospect source**: a competitor (its referring domains), a competitor's page (the sites linking to it), a keyword (its page-one sites), or nothing (find competitors first).
- **Budget**: a default run from one competitor to 30 domains costs about 12 + 12 + 5 + 4 + 30 x 8 + 30 = 303 credits with `limit: 10` on the contact call, or about 660 at the default 30 addresses per domain. Say so before starting if the user gave no budget; if they did, pass `max_credits` on every call.

## Steps

1. **Prospect.** Use one source:
   - A competitor: `seo_get_referring_domains` on it (100 rows, 12 credits). Run it on two or three competitors and keep the domains that link to at least two of them: a site that links to several rivals is the most likely to link to the user.
   - A competitor's page: `seo_get_backlinks` on the page URL (100 rows, 12 credits), then take `domain_from`. Best when one competitor page (a guide, a free tool, a study) earns most of their links.
   - A keyword: `seo_get_serp` for it (top 10, 1 credit). Page-one sites already cover the topic.
   - Nothing: `seo_get_serp_competitors` on the target (10 credits), then the first source on the top two competitors that are real businesses, not publishers or marketplaces.

   Then drop the domains that already link to the target: `seo_get_referring_domains` on the target (12 credits), or the `lost` flag on rows that used to.
2. **Rate.** `seo_get_domain_ratings` with every candidate in one call (4 credits per 100). Read the target's own domain rank from `seo_get_domain_overview` (5 credits). Apply the floors in the [router](../SKILL.md#floors): above the target's own rank, above about 20, `spam_score` at most 30. Cut to the 30 best by relevance first, rank second.
3. **Find a contact and verify it.** Run the [contact steps](../SKILL.md#contact-steps) on the kept domains: `leads_get_domain_emails` in batches of 20, then `leads_get_email_status` on the one address per domain you would send to.
4. **Deliver** a table: domain, domain rank, contact name, role, email, verification status, and one line on why the site fits (what it links to, the page that fits). Do not write or send the email; the host's sequencer takes the list.

## Judgment

- Referring domains of a competitor include directories, aggregators and spam. Skip anything with `spam_score` above 30 or a domain rank under the floor.
- A SERP prospect list is small but precise: page-one sites for the keyword already cover the topic, so the pitch writes itself.
- `lost: true` on a competitor's referring domain means the link is gone. Those sites linked once and may again: keep them, and say so in the "why" column.
- Person records never carry an email. If the user wants a specific person at a company, that is `leads_get_email`, which runs a waterfall and costs 6 credits on a hit.
- Cache hits on results this account already paid for are free: re-running for the same competitor within 7 days costs nothing, so it is fine to widen the list in a second pass.
