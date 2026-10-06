---
name: reclaim-lost-links
description: When the user wants to recover backlinks the site already earned. Finds links that point at broken or homepage-redirected pages on the site and maps each to a live page for a 301 redirect, then finds links other sites removed and a verified contact to ask for each one back. Also use when the user mentions lost backlinks, broken backlinks, reclaim links, link reclamation, 404 pages with links, which links did we lose, inbound links broken by a migration, or broken link building on a competitor's dead pages. A whole traffic drop goes to diagnose-traffic-drop, redirects for a planned move to create-migration-plan, new link prospects to find-backlink-targets, and pages that mention the brand without ever linking to find-unlinked-mentions.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Lost links

The cheapest links are the ones the site already earned. Two kinds are worth recovering: links that point at a page on the site that no longer works (fixed with a redirect, no outreach), and links a site removed (worth one polite email). It hands back two tables: redirects, and outreach.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_backlinks` and `leads_get_domain_emails` (hosts often add a prefix, for example `mcp__manifold__seo_get_backlinks`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the `seo_*` tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and deliver the redirects and the removed links without contacts.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the Search Console or Bing property) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Site**: the user's domain, or one section of it (a URL prefix).
- **Redirects**: whether the user can add 301 redirects themselves. If not, the first table goes to whoever runs the site.
- **Bing Webmaster**: whether the `console_*` tools are there with a Bing property (`console_list_properties`, free). Step 3 is better with it; the [Google search notes](../create-seo-plan/references/platforms/google.md#search-console) say how to check.
- **Budget**: a default run costs about 10 + 25 + 4 + 20 x 8 + 20 = 219 credits. `seo_get_page` is free but rate limited, so checking many URLs takes time rather than credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Size it.** `seo_get_backlink_summary` on the site (10 credits). `broken_backlinks` counts links that point at broken pages; `referring_domains` sets the scale.
2. **Pull the links.** `seo_get_backlinks` on the site with `limit: 1000` (25 credits); pass `meta.cursor` for more if the site has many. Split the rows: `lost: true` means the linking site removed the link; the rest are live links.
3. **Find broken pages that still have links.** Group the live rows by `url_to` and count the referring domains of each. Call `seo_get_page` (free) on each `url_to`, starting with the most linked 50. A `status` of 404, 410 or 5xx is a broken page with links. A redirect to the homepage (`final_url` is the root) wastes most of the value; count it as broken too. With Bing connected, `console_get_crawl_issues` (free) adds the `Code4xx` and `Code5xx` URLs Bing found, with `inlinks`: broken pages the backlink index may not list yet.
4. **Map each broken page to a live one.** Pick the closest live page on the site: the same topic, the new URL of a moved page, or the parent category. If you need the list of live pages, `seo_get_ranked_keywords` on the site (10 credits) gives the pages that rank. The fix is a 301 redirect; no outreach needed.
5. **Check the removed links.** For each `lost: true` row, `seo_get_page` on `url_from` (free). Drop the row when that page is gone or redirects elsewhere: the page was removed, not the link. Keep it when the page is live: the site took the link out or changed it, and a short email may bring it back.
6. **Rate and contact.** `seo_get_domain_ratings` on the domains of the kept removed links (4 credits), apply the [floors](../create-link-building-plan/references/outreach.md#floors), then run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on the top 20 with `limit: 10`.
7. **Deliver** two tables. Redirects: broken URL, status, referring domains, best `dr_from`, anchor text, suggested redirect target. Outreach: linking page, the page it linked to, domain rank, anchor, contact, email, verification status, and the ask (restore the link, or update it to the new URL).

## Judgment

- Do the redirects first. They recover every link to a page at once, cost nothing and need nobody's reply.
- A removed link on a page that was rewritten is often a lost citation, not a rejection. The ask is "you cited us before; here is the current page".
- Links lost from spam, directories or scraper sites are not worth an email. The floors apply.
- The same method on a competitor's domain is broken link building: `seo_get_backlinks` on the competitor, `seo_get_page` on their linked pages, and a pitch of the user's equivalent page to every site that links to a dead one. Run it only when the user has a page that truly replaces the dead one.
- The backlink index lags the live web by weeks. A link marked lost may have come back; step 5 catches most of those.
- The [outreach](../create-link-building-plan/references/outreach.md) handoff applies: never send; the deliverable is the two tables.

## Related skills

- A traffic drop that may trace to lost links: [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md). Redirect maps before and after a move: [create-migration-plan](../create-migration-plan/SKILL.md).
- Where links should come from next, and a 90-day plan: [create-link-building-plan](../create-link-building-plan/SKILL.md). New prospects: [find-backlink-targets](../find-backlink-targets/SKILL.md).
- Pages that name the brand but never linked: [find-unlinked-mentions](../find-unlinked-mentions/SKILL.md).
