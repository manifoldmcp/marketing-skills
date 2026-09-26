# Link building strategy

A strategy answers three questions before anyone sends a pitch: which pages need links, where competitors got theirs, and which of this group's playbooks gets the most links for the hours the team has. It ends in a 90-day plan, not a prospect list.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: the pages that should rank (default: the pages from step 3 that rank 4 to 20) and what success means (default: more referring domains above the floor, and those pages on page one).
- **Stage**: a new site with almost no links, or an established one. `seo_get_domain_overview` in step 2 answers it if the user does not know.
- **ICP**: who buys, so the prospects are sites those people read.
- **Budget**: credits for the research (this playbook costs about 140) and outreach hours per week. Default: 1,000 credits and 3 hours a week.
- **Team**: who writes and sends the pitches, and who can produce an asset worth linking to (a study, a free tool, a guide). Default: one founder, no designer.
- **Competitors**: two or three. Default: the top three from `seo_get_serp_competitors` that are real businesses, not publishers.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 55 credits for the site and three competitors. Call `seo_get_domain_overview` on the site (5 credits) for domain rank, traffic and top pages; `seo_get_backlink_summary` on the site and on each competitor (10 credits each) for referring domains, dofollow share and broken backlinks; and `seo_get_serp_competitors` on the site (10 credits) only if the user named no competitors.
3. **Gaps.** About 85 credits more.
   - Link intersect: `seo_get_referring_domains` with `limit: 300` on each competitor and on the site (15 credits each). Count the domains that link to at least two competitors and not to the site, and how many of them clear the [floors](../SKILL.md#floors).
   - What earns links: `seo_get_backlinks` with `limit: 300` on the strongest competitor (15 credits). Group the rows by `url_to` and name the three pages that earn most links and what they are (a tool, a study, a list, the homepage).
   - Pages that need links: `seo_get_ranked_keywords` on the site (10 credits). Keep the pages ranking 4 to 20 for keywords with volume and commercial or transactional intent; links move those first.
   - Lost ground: `broken_backlinks` above zero in the baseline, or `lost` rows in the site's referring domains.
4. **Tactics.** Choose two or three from this group, in this order of cost per link, and say why each fits the numbers:
   - [Lost links](lost-links.md) when the site has broken backlinks or lost referring domains: the cheapest links there are.
   - [Backlink targets](backlink-targets.md) when the intersect has 30 or more domains above the floors.
   - [Best-of lists](best-of-lists.md) when the category has "best X" or "X alternatives" keywords with listicles on page one.
   - [Affiliate partners](affiliate-partners.md) when competitor backlinks carry affiliate parameters or review sites rank for competitor names.
   - [Journalists](journalists.md) and a [PR strategy](pr-strategy.md) when competitors' best links come from media and the team can produce a story.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - KPIs the tools can measure again later: referring domains and dofollow share (`seo_get_backlink_summary`), new referring domains above the floor (`seo_get_referring_domains` with `lost`), links to the target pages (`seo_get_backlinks` on each page), and the position of the target pages for their keywords (`seo_get_position`, 6 credits each).
   - **Deliver** one document: the inputs with defaults marked, a baseline table (site against each competitor: domain rank, referring domains, dofollow share, broken backlinks), the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and the three first actions.

## Judgment

- Links move rankings over months, not weeks. The 30-day KPI is activity and links won; rank moves belong to the 90-day KPI.
- Set outreach volume from the hours the team has, not from the size of the prospect list. A short list of well-matched sites beats a long generic one.
- A competitor with far more referring domains but a similar domain rank has many weak links. Match the strong ones, not the count.
- A new site with a domain rank near zero passes the "above the site's own rank" test everywhere. Use the floor and relevance instead.
- If the strongest competitor pages are assets (a free tool, a data study), say that the plan needs one asset of that kind; outreach alone will not match them.
- Backlink data comes from an index that lags the live web by weeks. Say so when a KPI is re-measured within a month.
