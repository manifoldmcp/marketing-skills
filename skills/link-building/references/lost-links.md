# Lost and broken links

Links the site already earned and is losing value from: links that point at a page that no longer works (fixed with a redirect, no outreach), and links a site took down (one polite email). The cheapest links there are. Checked October 2026.

## How fast links decay

- **About a third of new links are gone within a year.** Linkody tracked 865,000 backlinks its users monitor (June 2026): 63% were still live after a year, 46% at two to five years, 31% after five years. Of the links that died, a fifth went within three months. Links on sites under Moz DA 10 survived worst (33% still live) and DA 30 to 49 best (about 70%).
- **The average hides two kinds of account.** Among 139 accounts that each added 30+ links in 2025, the median lost 1.8% while one in ten lost more than 70%; Linkody suspects where the links were bought, but has no field to prove it (August 2026 addendum).
- **Over long periods most links rot.** Of the links pointing at 2.06M sites since 2013, 66.5% had rotted by 2024; of the lost links, 47.7% disappeared because the linking page dropped out, 34.2% because the link was removed (Ahrefs, link rot study, updated February 2024).
- So a link plan that only adds links loses ground. Budget a quarterly check, and count net referring domains, not links won.

## Broken pages that still have links

1. `seo_get_backlink_summary` on the site (10 credits): `broken_backlinks` and `broken_pages` size the job.
2. `seo_get_backlinks` with `limit: 1000` (25 credits; `cursor` for more). Keep rows with `broken: true` (4xx or 5xx at the last crawl) or a 3xx `url_to_status`, group by `url_to`, and count distinct linking domains per page.
3. Confirm with `seo_get_page` (free) on each `url_to`, most-linked first, since it may be fixed since the crawl. A 404, 410 or 5xx is broken. A redirect to the homepage (`final_url` is the root) wastes most of the value; count it as broken too.
4. With a Bing Webmaster property connected, `console_get_crawl_issues` (free) adds `Code4xx` and `Code5xx` URLs Bing found, with `inlinks`: broken pages the backlink index may not list yet.
5. Map each to the closest live page (the moved page's new URL, the same topic, the parent category; `seo_get_ranked_keywords` lists the live pages that rank). The fix is a 301 redirect. Redirect work during a migration belongs to [technical-seo](../../technical-seo/SKILL.md).

Do the redirects first: they recover every link to a page at once, cost nothing and need nobody's reply.

## Links other sites removed

**Ask for them by status.** Both backlink tools list live links unless told otherwise, so `lost` is false on every default row ([field notes](field-notes.md)). `seo_get_backlinks` with `status: "lost"` lists the removed links, each with `last_seen` for about when it went; `seo_get_referring_domains` with `status: "lost"` lists the domains that dropped a link (same price, 12 credits per 100 rows). A domain there may still link elsewhere: compare it with the live list before calling it lost. The index only knows links it crawled, so also:

- **The user's own list.** An export from Search Console's Links report (a sample, up to 100,000 rows, with no nofollow flag), Ahrefs, Semrush or a link monitor, or a spreadsheet of links the team won.
- **Then check each linking page.** `seo_get_page` (free) on the old linking URL: if it is gone or redirects elsewhere, the page was removed, not the link, and there is nothing to ask. If it is live, the site took the link out or rewrote the page; that is worth one email.

Rate the kept domains ([floors](authority-metrics.md#floors)), find a contact for the top 20 ([contacts](contacts.md)), and ask to restore the link or update it to the current URL. A link dropped in a rewrite is usually a lost citation, not a rejection: "you cited us before; here is the current page." Links lost from directories, spam or scrapers are not worth an email.

About 220 credits for a site with 20 contacts.

## Deliverables

Two tables. Redirects: broken URL, status, linking domains, best linking domain's rank, anchor, suggested target. Outreach: linking page, the page it linked to, domain rank, anchor, contact, email, verification status, and the ask.
