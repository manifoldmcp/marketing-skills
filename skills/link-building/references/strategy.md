# A link building plan

When the user wants a plan rather than a list: which pages need links, where competitors got theirs, which tactics get the most links for the team's hours, over 90 days. Checked October 2026.

## Inputs

Take them from the product context file; ask in one message for what it lacks, and state the default used for anything unanswered.

- **Pages that should rank.** Default: the user's pages ranking 4 to 20 for keywords with volume and commercial or transactional intent (`seo_get_ranked_keywords`, 10 credits). Links move those first; a page on page five needs content work before links ([seo](../../seo/SKILL.md)).
- **Stage.** New site with almost no links, or established; `seo_get_domain_overview` answers it.
- **Team and hours.** Who writes and sends, and who can make an asset worth linking to. Default: one founder, three hours a week, no designer.
- **Competitors.** Two or three real businesses, not publishers. Default: the top of `seo_get_serp_competitors`.
- **Budget.** Research costs about 155 credits; contacts about 31 a site on top.

## Baseline (about 155 credits)

- `seo_get_domain_overview` on the user (15) and `seo_get_backlink_summary` on the user and each competitor (10 each): domain rank, referring main domains, dofollow share, broken backlinks.
- Link intersect: `seo_get_referring_domains` with `limit: 1000` on each competitor and the user (25 each), rated in one `seo_get_domain_ratings` call ([prospecting](prospecting.md#sources-and-what-each-costs)). Count domains that link to two or more competitors and not the user, and how many clear the [floors](authority-metrics.md#floors).
- What earns competitors links: `seo_get_backlinks` on the strongest competitor (`limit: 1000`, 25), grouped by page after stripping query strings, counted by distinct linking domain.
- Deliver a table: user against each competitor on domain rank, Ahrefs DR (with its attribution), referring main domains, dofollow share, broken backlinks.

## Choosing tactics

Pick two or three, cheapest per link first, and say which number justified each.

| Signal in the baseline | Tactic | Reference |
|---|---|---|
| `broken_backlinks` above zero, or a past migration | Redirect broken pages; then ask back removed links | [Lost links](lost-links.md) |
| Press, reviews, podcasts or guest appearances exist | Turn mentions into links | [Unlinked mentions](unlinked-mentions.md) |
| 30+ intersect domains above the floors | Outreach to sites that link to competitors | [Prospecting](prospecting.md) |
| "best X" or "X alternatives" keywords with publisher lists on page one | Get onto lists that rank | [Prospecting](prospecting.md#lists-that-rank-on-google) |
| Competitors' most-linked pages are tools, studies or data | One linkable asset, then digital PR | [digital-pr](../../digital-pr/SKILL.md) |
| Competitors' links carry affiliate parameters, or review sites rank for competitor names | Affiliate and partner programme | [creators](../../creators/SKILL.md) |
| The goal is AI answers, not Google | Sources the engines cite, chosen by citation | [ai-search](../../ai-search/SKILL.md) |

## The 30-60-90 plan

| Period | Typical work | KPI the tools re-measure |
|---|---|---|
| Days 1 to 30 | Redirects shipped; unlinked mentions and removed links asked; first 20 prospects contacted; asset chosen if needed | Redirects live (`seo_get_page`); asks sent; links won |
| Days 31 to 60 | Intersect and list outreach; asset built and pitched; follow-ups | New referring main domains above the floor (diff of `seo_get_referring_domains` snapshots) |
| Days 61 to 90 | Second outreach round; re-measure | Referring main domains and dofollow share (`seo_get_backlink_summary`); links to target pages (`seo_get_backlinks` on each URL); target pages' positions (`seo_get_position`, 6 credits each) |

Give each period an owner, and three first actions for this week.

## Setting targets honestly

- **Rank moves belong to the 90-day KPI, not the 30-day one.** No controlled study measures how long a link takes to move a ranking; the index also lags the live web by weeks, so a re-measure inside a month undercounts.
- **Promise net gains, not links won.** About a third of new links disappear within a year ([lost links](lost-links.md#how-fast-links-decay)).
- **Links are one input among many.** They correlate with rankings at about 0.25, weakly (Ahrefs, January 2025); Google says they are not a top-three factor. A plan that promises page one from links alone overpromises; say what else the target pages need.
- **Volume follows the hours.** Three hours a week is roughly 20 researched, personalised asks a week and one follow-up each; at 4% to 8% replies, a handful of links a month. Plans that promise 50 links a month on that budget are buying them.
