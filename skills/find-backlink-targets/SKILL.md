---
name: find-backlink-targets
description: When the user wants a list of sites to ask for a backlink, with someone to email at each. Finds sites that link to competitors, to a competitor's page or that rank on page one for a keyword, drops the ones that already link, rates the rest by domain rank and spam score, and finds and verifies a contact at each. Also use when the user mentions link prospects, who should we email for backlinks, sites that link to a competitor but not to us, a link outreach list, guest post targets, link insertions, or backlinks to a specific page. A link building plan goes to create-link-building-plan, best X lists to find-best-of-lists, pages that name the brand without a link to find-unlinked-mentions, lost or broken links to reclaim-lost-links, and people at companies for sales to build-lead-list.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Backlink targets

Sites worth a link, with a verified contact at each. Backlink outreach is "who at these sites could I email about a link", not "who fits an ICP". The key is a domain, so the contact tool is `leads_get_domain_emails`, never `leads_search_people`. It hands back a table for the user or their sequencer.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_referring_domains` and `leads_get_domain_emails` (hosts often add a prefix, for example `mcp__manifold__seo_get_referring_domains`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the `seo_*` tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and deliver the site list without contacts.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the competitors and their domains, the pages that need links) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Target**: the user's site, or the page that wants links.
- **Prospect source**: a competitor (its referring domains), a competitor's page (the sites linking to it), a keyword (its page-one sites), or nothing (find competitors first).
- **Budget**: a default run from one competitor to 30 domains costs about 15 + 25 + 5 + 9 + 30 x 8 + 30 = 324 credits with `limit: 10` on the contact call (15 more for each extra competitor), or about 684 at the default 30 addresses per domain. Say so before starting if the user gave no budget; if they did, pass `max_credits` on every call.

## Steps

1. **Prospect.** Use one source:
   - A competitor: `seo_get_referring_domains` on it with `limit: 300` (15 credits). Run it on two or three competitors and keep the domains that link to at least two of them: a site that links to several rivals is the most likely to link to the user.
   - A competitor's page: `seo_get_backlinks` on the page URL (100 rows, 12 credits), then take `domain_from`. Best when one competitor page (a guide, a free tool, a study) earns most of their links.
   - A keyword: `seo_get_serp` for it (top 10, 1 credit). Page-one sites already cover the topic.
   - Nothing: `seo_get_serp_competitors` on the target (10 credits), then the first source on the top two competitors that are real businesses, not publishers or marketplaces.

   Then drop the domains that already link to the target: `seo_get_referring_domains` on the target with `limit: 1000` (25 credits; the default 100 rows holds only its strongest linkers), or the `lost` flag on rows that used to.
2. **Rate.** `seo_get_domain_ratings` with every candidate in one call (1 credit plus 2.5 per 100: 9 for 300). Read the target's own domain rank from `seo_get_domain_overview` (5 credits). Apply the [floors](../create-link-building-plan/references/outreach.md#floors): above the target's own rank, above about 20, `spam_score` at most 30. Cut to the 30 best by relevance first, rank second.
3. **Find a contact and verify it.** Run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on the kept domains: `leads_get_domain_emails` in batches of 20, then `leads_get_email_status` on the one address per domain you would send to.
4. **Deliver** a table: domain, domain rank, contact name, role, email, verification status, and one line on why the site fits (what it links to, the page that fits). Do not write or send the email; the host's sequencer takes the list.

## Judgment

- Referring domains of a competitor include directories, aggregators and spam. Skip anything with `spam_score` above 30 or a domain rank under the floor.
- A SERP prospect list is small but precise: page-one sites for the keyword already cover the topic, so the pitch writes itself.
- `lost: true` on a competitor's referring domain means the link is gone. Those sites linked once and may again: keep them, and say so in the "why" column.
- Person records never carry an email. If the user wants a specific person at a company, that is `leads_get_email`, which runs a waterfall and costs 6 credits on a hit.
- Cache hits on results this account already paid for are free: re-running for the same competitor within 7 days costs nothing, so it is fine to widen the list in a second pass.
- The [outreach](../create-link-building-plan/references/outreach.md) credits and handoff rules apply: contacts last and only for the shortlist; never send, post or buy links.

## Related skills

- Where links should come from, and a 90-day plan: [create-link-building-plan](../create-link-building-plan/SKILL.md).
- Other ways to choose the sites: [find-best-of-lists](../find-best-of-lists/SKILL.md), [find-unlinked-mentions](../find-unlinked-mentions/SKILL.md), [reclaim-lost-links](../reclaim-lost-links/SKILL.md), [find-affiliate-partners](../find-affiliate-partners/SKILL.md).
- Who the competitors are, when the user has none: [find-competitors](../find-competitors/SKILL.md).
- People by job title at target companies, for sales rather than links: [build-lead-list](../build-lead-list/SKILL.md).
