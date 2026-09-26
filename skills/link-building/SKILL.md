---
name: link-building
description: Link building with the manifold tools. Finds sites worth a backlink, rates them, finds a contact at each and verifies the address. Also plans a link building or digital PR strategy, finds best-of lists and listicles that rank on Google, journalists to pitch, lost and broken links to reclaim, and affiliate partners. Use when the user asks for backlinks, link building, link prospects, an outreach list, off-page SEO, more domain authority or DR, guest posts, link insertions, unlinked mentions, lost or broken links, link reclamation, broken link building, a media list, journalists or reporters to pitch, a HARO alternative, press coverage, digital PR, getting into "best X" or "top 10" roundups, affiliate or partner sites, or who to email about a link. The result is a table for the user or their sequencer; sending, posting and paying for links are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Link building

Every job here ends the same way: a short list of sites, a person at each, and an address that works. The group's playbooks differ in how they choose the sites.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_referring_domains` and `leads_get_domain_emails` (hosts often add a prefix, for example `mcp__manifold__seo_get_referring_domains`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the `seo_*` tools are there but the `leads_*` tools are not, the leads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and deliver the site list without contacts.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only says "links", open backlink targets.

| Job | The user says | Open |
|---|---|---|
| Link building strategy: where links should come from, and a 90-day plan | "link building strategy", "plan our backlinks", "how do we get more links", "off-page SEO plan", "grow our domain authority" | [references/strategy.md](references/strategy.md) |
| Backlink targets: sites worth a link, with a verified contact at each | "find link prospects", "who should we email for backlinks", "sites that link to a competitor but not to us", "link outreach list", "guest post targets" | [references/backlink-targets.md](references/backlink-targets.md) |
| Best-of lists that rank: listicles on Google's first page to get into | "get us into best X lists", "listicles for our category", "roundup posts that rank", "top 10 tools articles", "alternatives pages we should be on" | [references/best-of-lists.md](references/best-of-lists.md) |
| Journalists: reporters who cover the topic, with an address each | "journalists to pitch", "build a media list", "reporters who write about X", "HARO alternative", "who covered our competitor" | [references/journalists.md](references/journalists.md) |
| Lost links: links the site lost, and its broken pages that still have links | "lost backlinks", "broken backlinks", "reclaim links", "404 pages with links", "link reclamation", "which links did we lose" | [references/lost-links.md](references/lost-links.md) |
| Affiliate partners: publishers who already promote competitors | "affiliate partners", "who promotes our competitor", "review sites to partner with", "recruit affiliates", "partner program prospects" | [references/affiliate-partners.md](references/affiliate-partners.md) |
| PR strategy: a digital PR plan that earns coverage and links | "digital PR strategy", "PR plan", "how do we get press coverage", "data-led PR", "newsjacking plan" | [references/pr-strategy.md](references/pr-strategy.md) |

## Shared rules

### Floors

- **Domain rank.** Keep a site only when its `domain_rank` is above about 20 on DataForSEO's 0 to 1000 scale; below that a link moves nothing. For backlink targets, also keep it only when it is above the user's own domain rank from `seo_get_domain_overview`. Lists, publications and partners qualify by where they rank and who reads them, not by that second test.
- **Spam.** Skip any referring domain with a `spam_score` above 30, and skip directories, link farms, scraper sites and aggregators whatever their rank.
- **Relevance beats rank.** A site that covers the user's topic at domain rank 150 beats an unrelated site at 600. Give one line of evidence (a page title, a URL) for why each site fits.

### Contact steps

These are the only copy of the contact mechanics. Other playbooks, in this group and in `ai-search`, link here instead of repeating them.

1. **Find a contact.** Call `leads_get_domain_emails` with up to 20 kept domains per call, `department: ["marketing", "communication"]` (unless the playbook names other departments) and `type: "all"`. It costs 2 credits per domain plus 6 per 10 addresses returned: about 20 credits a domain at the default 30 addresses, or 8 with `limit: 10`, which is enough when you need one contact. Choose, in this order: a named editor or content lead with `verification_status: valid`; any named person in those departments; a generic address (`editor@`, `press@`, `hello@`). Small sites often have only the generic one, and it usually works. If a domain has nothing in those departments, call it once more with no `department` (on a one-person site the owner sits under `executive`).
2. **Named person, no address.** When you know the person (a byline, a LinkedIn author) and the domain search does not have them, call `leads_get_email` with `first_name`, `last_name` and `domain`: 6 credits on a hit, 1 on `NoData`. Do not retry a `NoData`. `domains[].pattern` from the domain search shows the address format, but a built address is unproven until step 3.
3. **Verify the shortlist.** Call `leads_get_email_status` on the one address per site you would actually send to, never on the whole list (1 credit each). Drop `invalid`. Keep `accept_all` and mark it unproven. `unknown` means the mail server refused the check: keep it with that note.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Rate in bulk and early: `seo_get_domain_ratings` takes up to 1,000 domains for 1 credit plus 2.5 per 100. Never rate domains one at a time with `seo_get_domain_overview`.
- Find contacts last, and only for the shortlist. Contacts cost more than everything before them.
- A result this account already paid for is free while it is cached (7 days for backlink data, 30 days for domain emails), so a second, wider pass costs little.

### Handoff

- Never send, post, buy links or fill in forms. The deliverable is a table. If the host has an email or sequencer tool, offer to pass the table to it; do not send.
- Write outreach copy only when the user asks, and then one short pitch per row that names the page that fits.
- The server keeps no state. If the user wants to track replies or run the job again next month, the host keeps the table.

## Other groups

- Mentions and citations in AI answers (ChatGPT, Perplexity, Google AI Overviews): the [ai-search](../ai-search/references/citation-building.md) group's citation building, which uses the contact steps above; lists that AI engines cite rather than lists that rank: its [best-of lists](../ai-search/references/best-of-lists.md).
- Keywords, content, audits and comparison pages: the [seo](../seo/SKILL.md) group.
- Who the real competitors are, and everything about them beyond links: the [competitors](../competitors/SKILL.md) group.
- People by job title at target companies, for sales rather than links: the [leads](../leads/SKILL.md) group.
- Press for a launch: the [launch](../launch/references/press.md) group.
