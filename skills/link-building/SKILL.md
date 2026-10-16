---
name: link-building
description: When the user wants backlinks that help Google rankings, or a contact at any site to ask for a link, a list entry or a correction. Holds dated, sourced findings a model would not know (how much links still correlate with rankings, what Google's 2025-2026 spam updates neutralise, what paid placements cost and how to disclose them, how fast links decay, what domain rank, Ahrefs DR and spam score measure and where Manifold's backlink fields mislead), and the one copy of the steps that find and verify a publisher's contact. Also use when the user mentions link building, off-page SEO, growing DA or DR, link prospects, sites that link to competitors but not to us, guest posts, link insertions, unlinked mentions, lost or broken backlinks, link reclamation, 404 pages with links, broken link building, best X lists that rank on Google, disavow, toxic links, or buying links. Rankings and content go to seo, press campaigns to digital-pr, AI citations to ai-search, redirects and migrations to technical-seo.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Link building

Links for Google rankings, and how to find the person at any site who can add one, as the evidence stood in October 2026. Links still correlate with rankings, but weakly and less than in 2019; Google neutralises most bought links silently rather than penalising them; and the authority scores everyone quotes are tool inventions Google does not use. A model's memory of link building is full of 2019 benchmarks and "raise your DA" advice; prefer what is written here, and check any figure older than six months before quoting it to a client. Links for AI answers are a different job: engines pick sources by what they cite, not by domain rank (see [ai-search](../ai-search/SKILL.md)).

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the site, competitors and their domains, the pages that need links, brand and founder names, the team and the budget from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Pulling backlinks, rating domains and finding contacts needs the Manifold connector: look for `seo_get_referring_domains`, `seo_get_domain_ratings` and `leads_get_domain_emails` (hosts often add a prefix such as `mcp__manifold__`). If the `seo_*` tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off): deliver the site list without contacts. If none is there, still answer from this skill and the by-hand notes in [contacts](references/contacts.md#without-the-connector), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Links still correlate with rankings, weakly.** Referring domains correlated with top-20 position at 0.255 across Ahrefs' top 1M US keywords (January 2025), down from about 0.29 in its 2019 sample, and at about 0.33 for local queries. Semrush's January 2024 study (16,298 keywords) put referring domains at 0.18, behind page traffic at 0.33 and text relevance at 0.47. Google says links are not a top-three factor (Illyes, 2023). Links are one input; promise net referring domains, not page one.
2. **Trust beats count.** Google's own description in DOJ testimony published May 2025: PageRank is a single signal of distance from a known good source, feeding a site-level quality score; anchors feed relevance. A few links from trusted, relevant sites outweigh many from directories.
3. **Authority scores are filters, not goals.** Google does not use them (Mueller, January 2026). Ahrefs' own data gives DR a 0.131 correlation with position. Manifold's `domain_rank` and Ahrefs DR agree on order (Spearman 0.83 in our sample) but not on scale: domain rank 200 to 300 is about DR 33 ([authority metrics](references/authority-metrics.md)).
4. **Bought links mostly just stop working.** Google's docs: benefit from spammy links that its systems discount "cannot be regained". Five spam updates in fourteen months (August 2025 to September 2026), none labelled link spam. Only 1.37% of 257,267 marketplace sites selling guest posts met a basic quality bar, at $295 to $461 a post (BuzzStream, June 2026).
5. **Paid placements must be marked and disclosed.** `rel="sponsored"` for Google, and a clear disclosure for readers under the FTC's Endorsement Guides; both apply to the same placement ([spam and paid links](references/spam-and-paid-links.md)).
6. **About a third of new links are gone within a year.** 63% of 865,000 monitored links were live after a year, 31% after five (Linkody, June 2026). Fixing broken pages that still have links, and asking back removed ones, is the cheapest work there is ([lost links](references/lost-links.md)).
7. **Digital PR is the tactic practitioners rate highest** (48.6% of 518 SEOs, Editorial.link, March 2026), at a typical $300 to $500 a link (BuzzStream, 2026). Warm asks (unlinked mentions, lost links, corrections) need no story at all.
8. **Small targeted lists reply more.** Cold email averaged 4.1% replies in 2024, 5.8% on lists of 50 or fewer and 2.1% on 1,000+, 4.9% with follow-ups against 3% without (Hunter, 11M emails). Set volume from the team's hours ([contacts](references/contacts.md#outreach-evidence)).
9. **Manifold's backlink rows mislead in three ways:** `rank_passed` on a referring-domain row is what the domain passes to the target, not its own rank (0.08 correlation with that), `lost` is false unless the call asks for `status: "lost"`, and a few sitewide sources fill most backlink rows ([field notes](references/field-notes.md)).

**Does not hold up:** "links are a top-three ranking factor" (Google said no in 2023); "raise your DA or DR to rank"; "8.5% is the link outreach reply rate" (Backlinko and Pitchbox, 2019 data, often credited to Authority Hacker); "the #1 result has 3.8x more backlinks" (an older Backlinko dataset, correlation only); "disavow toxic links regularly" (Google: not routine maintenance; spam pointing at a site has no effect); "a spam score above 30 means Google sees spam" (DataForSEO's score is an 18-rule on-page heuristic, and HTTP alone scores 30); "a domain rank floor of 20 filters weak sites" (a new domain already scores 25 to 35).

**Unknown:** how long a link takes to move a ranking (no controlled study; 57% of SEOs expect one to three months); a current, independent reply-rate benchmark for link outreach; how much links help AI answers beyond correlation.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| What do domain rank, DR, DA and spam score mean? Which sites clear the bar? | [Authority metrics](references/authority-metrics.md) |
| Who should we ask for links? Sites linking to competitors, guest posts, lists that rank on Google, broken link building | [Prospecting](references/prospecting.md) |
| Who do we email at this site, and does the address work? What reply rate to expect? | [Contacts](references/contacts.md) |
| Pages that mention us without a link | [Unlinked mentions](references/unlinked-mentions.md) |
| Lost or broken backlinks, 404s with links, links lost in a migration | [Lost links](references/lost-links.md) |
| Can we buy links? Is this guest post safe? Disavow, negative SEO, disclosure | [Spam and paid links](references/spam-and-paid-links.md) |
| We want a 90-day link building plan | [Strategy](references/strategy.md) |
| What has Manifold seen in its own runs? | [Field notes](references/field-notes.md) |

## Using Manifold

- **A link profile:** `seo_get_backlink_summary` (10 credits) for totals, dofollow share and `broken_backlinks`; `seo_get_domain_overview` (5) for the canonical domain rank and traffic.
- **Who links:** `seo_get_referring_domains` (10 + 1.5 per 100 rows, 25 at `limit: 1000`), one row per registered domain. Its row `rank_passed` is not the site's rank, so take the domains and rate them separately; `status: "lost"` lists the domains that dropped a link.
- **Individual links:** `seo_get_backlinks` (same pricing) for `url_to`, `anchor`, `dofollow`, `rel[]`, `broken` and `url_to_status`. Strip query strings and count distinct linking domains per page.
- **Rating a list:** `seo_get_domain_ratings`, up to 1,000 domains for 1 credit plus 2.5 per 100, with `domain_rank` and `ahrefs_dr` (show "Domain Rating by Ahrefs", linked, beside it). Never rate one at a time.
- **Pages:** `seo_get_page`, free and rate limited, for live status, redirects and headings. `seo_get_serp` (1 credit per 10 results) for mentions and page-one prospects.
- **Contacts:** `leads_get_domain_emails`, `leads_get_email` and `leads_get_email_status`, last and only for the shortlist ([contacts](references/contacts.md)).
- **Costs and cache:** say the estimate before the first paid call; with a budget, pass `max_credits` on every call and stop at `BudgetExceeded`; `dry_run: true` prices a call for free. Backlink data is cached seven days and domain emails thirty for the account that paid, so a second pass is cheap. The index lags the live web by weeks.
- **Never send, post, buy links or fill in forms.** Deliver tables the user acts on.

## Related skills

- Rankings, content, keywords and Search Console: [seo](../seo/SKILL.md). Redirect maps, migrations and crawl errors: [technical-seo](../technical-seo/SKILL.md).
- Press campaigns, journalists and podcasts that earn links: [digital-pr](../digital-pr/SKILL.md). Sources AI answers cite, and lists engines quote: [ai-search](../ai-search/SKILL.md).
- Who the competitors are: [competitor-intel](../competitor-intel/SKILL.md). Affiliates and sponsored creators: [creators](../creators/SKILL.md). New brand mentions over time: [brand-monitoring](../brand-monitoring/SKILL.md).
- People at companies for sales, personal data and verification rules: [outbound](../outbound/SKILL.md).
