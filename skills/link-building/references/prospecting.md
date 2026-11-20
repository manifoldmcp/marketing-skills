# Prospecting

Where to find sites worth asking for a link, which kinds of link still work, and how to read the backlink data without being misled. Contacts for the shortlist come from [contacts](contacts.md); the floors from [authority metrics](authority-metrics.md). Checked October 2026.

## What works now

- **Digital PR is what practitioners rate highest.** In Editorial.link's March 2026 survey of 518 SEOs, 48.6% named digital PR the most effective tactic, guest posting 16%, linkable assets 12%. In BuzzStream's 2026 survey of digital PR teams, 85.8% said links are what digital PR does best and data-led stories were the most used tactic (95.9%). A survey of opinions, not of outcomes, but consistent across vendors. Campaign work belongs to [digital-pr](../../digital-pr/SKILL.md); this reference covers the asks that need no story.
- **Pages competitors earn links with are the template.** Group a competitor's links by the page they point to: when the most-linked pages are a free tool, a data study or a glossary, outreach alone will not match them; the plan needs one asset of that kind.
- **Sites that link to two or more competitors and not to the user** are the most likely to link to the user. They already cover the category and link out in it.
- **Page-one results for a keyword** already cover the topic, so a short, precise list with a pitch that writes itself.
- **Best-of lists that rank on Google** send buyers and a link at once (below). Lists AI engines cite are chosen by citations instead: [ai-search](../../ai-search/SKILL.md).
- **Resource pages and broken links** still work as one-off asks when the user has a page that truly fits or replaces a dead one; no study since 2019 measures their yield, so do not promise a rate.
- **Guest posts work only as editorial contributions.** A guest post that passes ranking credit with optimised anchors is named link spam in Google's policies; a placement bought from a marketplace usually is one ([spam and paid links](spam-and-paid-links.md)).

## Sources, and what each costs

1. **Competitor link intersect.** `seo_get_referring_domains` with `limit: 1000` on two or three competitors (25 credits each) and on the user (25 credits, to drop sites that already link). Keep the domains on two or more competitor lists and not on the user's. With `limit: 300` (15 credits) you get 300 rows sorted by a rank field that is not the linking site's authority (see the trap below), so take the larger pull when the profile is big.
2. **One competitor page.** `seo_get_backlinks` on the page URL (100 rows, 12 credits), then take `domain_from`. Best when one page earns most of a competitor's links.
3. **A keyword's first page.** `seo_get_serp` (top 10, 1 credit). Keep publishers, drop vendors and marketplaces.
4. **No competitors named.** `seo_get_serp_competitors` on the user (10 credits) and take the top two that are real businesses, not publishers; [competitor-intel](../../competitor-intel/SKILL.md) for who the real competitors are.

Then rate every candidate in one `seo_get_domain_ratings` call (7 credits plus 2.5 per 100) and cut to the best 30 by relevance first, rank second. A default run from one competitor to 30 contacted sites costs about 25 + 25 + 8 + 30 × 31 + 30 ≈ 1,020 credits, 25 more for each extra competitor.

## Reading the backlink data

- **`rank_passed` on a referring-domain row is not the linking site's authority.** It is what the domain passes to the target, and correlated 0.08 with the domain's own rank in our test; always rate with `seo_get_domain_ratings` ([field notes](field-notes.md)).
- **Both endpoints list live links unless asked.** `lost` is false on every default row; `status: "lost"` lists the links taken down ([lost links](lost-links.md)).
- **`seo_get_backlinks` rows are dominated by sitewide links.** In our test, 1,000 rows came from only 106 domains and one directory supplied 548 of them, each with its own UTM-tagged URL. To find what earns links, strip query strings and `www` from `url_to`, then count distinct `domain_from` per page, not rows.
- **`dr_from` is the linking host's rank and inherits platforms.** A Medium or beehiiv subdomain shows the platform's 700-plus. Judge a post on a platform by the author, not the number.
- **Referring domains are counted by registered domain on the row list** (`referring_main_domains` in `seo_get_backlink_summary`), while the summary's `referring_domains` counts subdomains too; compare like with like.
- **A competitor with far more referring domains at a similar domain rank** has many weak links. Match its strong, relevant ones, not its count.
- **Directories and spam networks inflate every new SaaS profile.** In our test about a quarter of the top 200 linkers of a CRM startup were AI-tool directories and SaaS lists, judged by name, and 169 of 1,000 linking domains had spam scores above 30, 101 of them first seen in 2026 (networks such as numbered seo-anomaly .xyz domains). Never copy a competitor's directory links.
- **The index lags the live web by weeks.** A link placed this month may not be there yet; a link listed may be gone. Cached results are free for seven days to the account that paid, so a second, wider pass costs little.

## Lists that rank on Google

1. `seo_search_keywords` with `seed: "best <category>"` and again with `seed: "<top competitor> alternatives"` (10 credits each). Keep keywords that read as a list (best, top, alternatives, vs, tools, software, apps) and the top 10 by volume.
2. `seo_get_serp` for each (1 credit each). Keep results whose title reads as a list on a publisher, not a vendor; a vendor's own "best X" never names a rival. Note Reddit threads and videos for [ai-search](../../ai-search/SKILL.md).
3. `seo_get_page` on each list (free, rate limited): products sit in `h2[]`. Mark each: names the user, names competitors but not the user, or unclear.
4. Rank: lists naming two or more competitors and not the user first, then by keyword volume and position. Position on page one is the proof a list is worth the effort; a list that fell off page one is not.

- A list the user is already on is not a target, but if it ranks, ask the editor to update the entry.
- In a new category with no "best X" lists yet, "alternatives to <incumbent>" lists exist earlier.
- Many lists are affiliate pages and the editor may ask for a fee or a commission deal. Flag the likely ones (many paid tools reviewed, "we earn a commission") and let the user decide. A paid placement must be disclosed and its link marked sponsored.
- About 674 credits for 20 lists with contacts.

## Broken link building on a competitor

`seo_get_backlinks` on the competitor, keep rows with `broken: true` or a 404 or 410 `url_to_status`, confirm each dead page with `seo_get_page` (free), and pitch the user's equivalent page to every site linking to it. Run it only when the user's page truly replaces the dead one; otherwise the ask is spam.
