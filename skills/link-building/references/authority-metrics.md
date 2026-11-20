# Authority metrics and floors

What domain rank, Ahrefs DR, Moz DA and spam score measure, how they map onto each other, and the floors a site must clear before it goes on any outreach list. Other references and other skills apply these floors. Checked October 2026.

## What the numbers are

None of them is used by Google. John Mueller, January 2026, on a third-party spam score: scores like that are made up by SEO tools and not used by Google. They are useful as a cheap filter on a list, never as a goal.

- **DataForSEO domain rank** (`domain_rank`, 0 to 1000). The original PageRank formula over DataForSEO's link graph with a damping factor of 0.5; nofollow, ugc and sponsored links pass nothing; a domain's rank is a log of the summed rank of its pages ([DataForSEO help](https://dataforseo.com/help-center/what_is_rank_in_backlinks_api)). DataForSEO's own bands: 25 to 35 for a new domain with few links, 200 to 300 for a large domain with many good links, 500+ very rare. It rates the host as given, so www.example.com and example.com can differ.
- **Ahrefs Domain Rating** (`ahrefs_dr`, 0 to 100). Purely link-based and logarithmic: each followed linking domain passes its DR divided by how many domains it links to; the first link from a domain counts, nofollow counts for nothing, traffic and spam are ignored ([Ahrefs](https://ahrefs.com/blog/domain-rating/)). Always rated for the registered domain, so every notion.site or blogspot.com subdomain gets the platform's 90-plus. It is the number users and vendors quote, so show it beside domain rank, with its attribution ("Domain Rating by Ahrefs", linked) wherever it appears.
- **Moz Domain Authority** (1 to 100). Moz's own link index and a machine-learning model of how likely a domain is to rank (moz.com, March 2025); no Manifold tool returns it. If a user quotes DA, compare it with DR, not with domain rank.
- **DataForSEO spam score** (`spam_score`, 0 to 100). Not a link-spam judgment: 18 rule-based on-page signals, each adding or subtracting a fixed amount (HTTP without HTTPS +30, a "spammy" TLD such as .xyz or .top +20, over 100 external links +20, contact details −20), averaged over the site's pages; the method page was last updated in 2022 ([DataForSEO help](https://dataforseo.com/help-center/what-is-spam-score-and-how-is-it-calculated)). Bands: 0 to 30 low, 31 to 60 medium, 61 to 100 high. An honest HTTP-only hobby blog scores 30 on that one signal.

## How domain rank maps to DR

From our own rating of 197 domains (see [field notes](field-notes.md)): Spearman 0.83 between `domain_rank` and `ahrefs_dr`, so they agree on order most of the time, but the scales are not linear.

| domain_rank | Typical Ahrefs DR (median, our sample) |
|---|---|
| 0 | Not in DataForSEO's index; DR usually 0 to 5 |
| 1 to 100 | About 2 |
| 100 to 200 | About 13 |
| 200 to 300 | About 33 |
| 300 to 500 | About 50 |
| 500 to 700 | About 90 (the large platforms and publishers) |
| 700+ | 91 to 100 |

- **When the two disagree badly, look at the site.** Six of 197 had domain rank 200+ and DR under 10 (AI-tool directories, a status-check site): a profile DataForSEO's graph scores and Ahrefs does not, often from link networks. The reverse (domain rank 0, DR above 0) is usually a new or unindexed domain.
- **Moving DR or domain rank is not the goal.** In Ahrefs' own 1M-keyword study (January 2025), DR correlated with position at only 0.131, against 0.255 for referring domains to the page's site. Google ranks pages, and a competitor with a similar DR can still outrank the user.

## Floors

Apply these to every outreach list: prospects, lists, unlinked mentions, lost links and the contacts other topics look up.

- **Rate with `seo_get_domain_ratings`, never with `rank_passed` on a referring-domain row.** That row field is the rank the domain passes to the target, and correlated 0.08 with the same domain's own domain rank in our test: Product Hunt showed 158 on the row and 672 rated. It is not the linking site's authority, and sorting by it does not give the strongest linkers first.
- **Domain rank floor: about 100** (roughly DR 10 and up). The old floor of 20 dropped almost nothing: DataForSEO puts a brand-new domain at 25 to 35, and in our sample everything at 20 or below had DR 0 to 2. For backlink prospects, also prefer sites above the user's own domain rank from `seo_get_domain_overview`; for a new site near zero, that second test passes everything, so use the floor and relevance.
- **Do not apply a rank floor** to a list page that ranks on Google's first page (its position is the proof), to a page an AI engine cites (see [ai-search](../../ai-search/SKILL.md)), or to an unlinked mention on a small site that reviewed the product. Rank decides little there; relevance decides.
- **Spam:** drop `spam_score` above 60 outright; between 31 and 60, look at the site before keeping it (often HTTP or an odd TLD, sometimes a link network). Drop directories, link farms, scraper sites, "submit your AI tool" lists and aggregators whatever their numbers.
- **Relevance beats rank.** A site that covers the user's topic at domain rank 150 beats an unrelated site at 600. Give one line of evidence (a page title, a URL) for why each site fits.
- **Traffic beats rank when they disagree.** A site with a high rank and almost no organic traffic sells links or was built for them. `seo_get_traffic_estimates` checks a shortlist at about ten times the cost of rating; use it on the final 20, not the list.
