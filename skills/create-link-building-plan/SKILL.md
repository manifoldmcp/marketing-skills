---
name: create-link-building-plan
description: When the user wants a link building plan. Compares the site's backlinks with its competitors', finds the link gap, the pages that need links and what earns competitors theirs, picks the tactics that get the most links for the team's hours, and ends in a 30-60-90 day plan. Also holds the floors and contact steps every outreach list shares. Also use when the user mentions link building, a link building strategy, plan our backlinks, how do we get more links, off-page SEO, an off-page SEO plan, growing domain authority or DR, or a link plan for a few hours a week. A list of sites to email goes to find-backlink-targets, best X lists to find-best-of-lists, a press plan to create-digital-pr-plan, journalists to find-journalists, lost or broken links to reclaim-lost-links, and links from pages that already mention the brand to find-unlinked-mentions.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Link building

A link building strategy answers three questions before anyone sends a pitch: which pages need links, where competitors got theirs, and which tactics get the most links for the hours the team has. It hands back a 90-day plan, not a prospect list. Every tactic it picks ends the same way: a short list of sites, a person at each, and an address that works, by the shared [outreach](references/outreach.md) rules.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_referring_domains` and `seo_get_backlink_summary` (hosts often add a prefix, for example `mcp__manifold__seo_get_referring_domains`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the `seo_*` tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). The plan needs no contacts; say so when a chosen tactic will.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the competitors and their domains, the ICP, the pages that need links, the team and the budget) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: the pages that should rank (default: the pages from step 3 that rank 4 to 20) and what success means (default: more referring domains above the floor, and those pages on page one).
- **Stage**: a new site with almost no links, or an established one. `seo_get_domain_overview` in step 2 answers it if the user does not know.
- **ICP**: who buys, so the prospects are sites those people read.
- **Budget**: credits for the research (this skill costs about 140) and outreach hours per week. Default: 1,000 credits and 3 hours a week.
- **Team**: who writes and sends the pitches, and who can produce an asset worth linking to (a study, a free tool, a guide). Default: one founder, no designer.
- **Competitors**: two or three. Default: the top three from `seo_get_serp_competitors` that are real businesses, not publishers.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 55 credits for the site and three competitors. Call `seo_get_domain_overview` on the site (5 credits) for domain rank, traffic and top pages; `seo_get_backlink_summary` on the site and on each competitor (10 credits each) for referring domains, dofollow share and broken backlinks; and `seo_get_serp_competitors` on the site (10 credits) only if the user named no competitors.
3. **Gaps.** About 85 credits more.
   - Link intersect: `seo_get_referring_domains` with `limit: 300` on each competitor and on the site (15 credits each). Count the domains that link to at least two competitors and not to the site, and how many of them clear the [floors](references/outreach.md#floors).
   - What earns links: `seo_get_backlinks` with `limit: 300` on the strongest competitor (15 credits). Group the rows by `url_to` and name the three pages that earn most links and what they are (a tool, a study, a list, the homepage).
   - Pages that need links: `seo_get_ranked_keywords` on the site (10 credits). Keep the pages ranking 4 to 20 for keywords with volume and commercial or transactional intent; links move those first.
   - Lost ground: `broken_backlinks` above zero in the baseline, or `lost` rows in the site's referring domains.
4. **Tactics.** Choose two or three, in this order of cost per link, and say why each fits the numbers:
   - [reclaim-lost-links](../reclaim-lost-links/SKILL.md) when the site has broken backlinks or lost referring domains: the cheapest links there are.
   - [find-unlinked-mentions](../find-unlinked-mentions/SKILL.md) when the brand has press, reviews or guest appearances: the writer already chose to name it, so the ask is one line.
   - [find-backlink-targets](../find-backlink-targets/SKILL.md) when the intersect has 30 or more domains above the floors.
   - [find-best-of-lists](../find-best-of-lists/SKILL.md) when the category has "best X" or "X alternatives" keywords with listicles on page one.
   - [find-affiliate-partners](../find-affiliate-partners/SKILL.md) when competitor backlinks carry affiliate parameters or review sites rank for competitor names.
   - [find-journalists](../find-journalists/SKILL.md) for a media list and [create-digital-pr-plan](../create-digital-pr-plan/SKILL.md) for a PR plan, when competitors' best links come from media and the team can produce a story.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - KPIs the tools can measure again later: referring domains and dofollow share (`seo_get_backlink_summary`), new referring domains above the floor (`seo_get_referring_domains` with `lost`), links to the target pages (`seo_get_backlinks` on each page), and the position of the target pages for their keywords (`seo_get_position`, 6 credits each).
   - **Deliver** one document: the inputs with defaults marked, a baseline table (site against each competitor: domain rank, referring domains, dofollow share, broken backlinks), the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions.

## Judgment

- Links move rankings over months, not weeks. The 30-day KPI is activity and links won; rank moves belong to the 90-day KPI.
- Set outreach volume from the hours the team has, not from the size of the prospect list. A short list of well-matched sites beats a long generic one.
- A competitor with far more referring domains but a similar domain rank has many weak links. Match the strong ones, not the count.
- A new site with a domain rank near zero passes the "above the site's own rank" test everywhere. Use the floor and relevance instead.
- If the strongest competitor pages are assets (a free tool, a data study), say that the plan needs one asset of that kind; outreach alone will not match them.
- Backlink data comes from an index that lags the live web by weeks. Say so when a KPI is re-measured within a month.
- The credits and handoff rules in [outreach](references/outreach.md) apply to every tactic: never send, post or buy links; the deliverable is a table.

## Related skills

- Mentions and citations in AI answers (ChatGPT, Perplexity, Google AI Overviews): [build-ai-citations](../build-ai-citations/SKILL.md), which uses the same contact steps; the plan for them: [create-ai-search-plan](../create-ai-search-plan/SKILL.md).
- Keywords, content and comparison pages: [create-seo-plan](../create-seo-plan/SKILL.md) and [plan-comparison-pages](../plan-comparison-pages/SKILL.md). Crawls, traffic drops and migrations: [audit-technical-seo](../audit-technical-seo/SKILL.md), [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md), [create-migration-plan](../create-migration-plan/SKILL.md).
- Who the real competitors are: [find-competitors](../find-competitors/SKILL.md).
- People by job title at target companies, for sales rather than links: [build-lead-list](../build-lead-list/SKILL.md).
- Press for a launch: [create-launch-plan](../create-launch-plan/SKILL.md). Creators to sponsor rather than pitch: [find-creators](../find-creators/SKILL.md). Podcasts to guest on: [find-podcasts](../find-podcasts/SKILL.md).
