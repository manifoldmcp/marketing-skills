# Lost links

The cheapest links are the ones the site already earned. Two kinds are worth recovering: links that point at a page on the site that no longer works (fixed with a redirect, no outreach), and links a site removed (worth one polite email).

## Inputs to settle first

- **Site**: the user's domain, or one section of it (a URL prefix).
- **Redirects**: whether the user can add 301 redirects themselves. If not, the first table goes to whoever runs the site.
- **Budget**: a default run costs about 10 + 25 + 4 + 20 x 8 + 20 = 219 credits. `seo_get_page` is free but rate limited, so checking many URLs takes time rather than credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Size it.** `seo_get_backlink_summary` on the site (10 credits). `broken_backlinks` counts links that point at broken pages; `referring_domains` sets the scale.
2. **Pull the links.** `seo_get_backlinks` on the site with `limit: 1000` (25 credits); pass `meta.cursor` for more if the site has many. Split the rows: `lost: true` means the linking site removed the link; the rest are live links.
3. **Find broken pages that still have links.** Group the live rows by `url_to` and count the referring domains of each. Call `seo_get_page` (free) on each `url_to`, starting with the most linked 50. A `status` of 404, 410 or 5xx is a broken page with links. A redirect to the homepage (`final_url` is the root) wastes most of the value; count it as broken too.
4. **Map each broken page to a live one.** Pick the closest live page on the site: the same topic, the new URL of a moved page, or the parent category. If you need the list of live pages, `seo_get_ranked_keywords` on the site (10 credits) gives the pages that rank. The fix is a 301 redirect; no outreach needed.
5. **Check the removed links.** For each `lost: true` row, `seo_get_page` on `url_from` (free). Drop the row when that page is gone or redirects elsewhere: the page was removed, not the link. Keep it when the page is live: the site took the link out or changed it, and a short email may bring it back.
6. **Rate and contact.** `seo_get_domain_ratings` on the domains of the kept removed links (4 credits), apply the [floors](../SKILL.md#floors), then run the [contact steps](../SKILL.md#contact-steps) on the top 20 with `limit: 10`.
7. **Deliver** two tables. Redirects: broken URL, status, referring domains, best `dr_from`, anchor text, suggested redirect target. Outreach: linking page, the page it linked to, domain rank, anchor, contact, email, verification status, and the ask (restore the link, or update it to the new URL).

## Judgment

- Do the redirects first. They recover every link to a page at once, cost nothing and need nobody's reply.
- A removed link on a page that was rewritten is often a lost citation, not a rejection. The ask is "you cited us before; here is the current page".
- Links lost from spam, directories or scraper sites are not worth an email. The floors apply.
- The same method on a competitor's domain is broken link building: `seo_get_backlinks` on the competitor, `seo_get_page` on their linked pages, and a pitch of the user's equivalent page to every site that links to a dead one. Run it only when the user has a page that truly replaces the dead one.
- The backlink index lags the live web by weeks. A link marked lost may have come back; step 5 catches most of those.
