---
name: create-digital-pr-plan
description: When the user wants a digital PR plan that earns press coverage, and the links and mentions it brings. Finds the stories that earned competitors their coverage, the publications that link to rivals but not to the user, and a story the user can own from search trends and Reddit debates, then lays out a 30-60-90 day plan with owners and KPIs. Also use when the user mentions a digital PR strategy, a PR plan, how do we get press coverage, data-led PR, a data study or survey for press, newsjacking, reactive PR, or expert comment. A media list of reporters for one story goes to find-journalists, press timed to a launch day to create-launch-plan, coverage that names the brand without linking to find-unlinked-mentions, and a whole link plan to create-link-building-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Create a digital PR plan

Digital PR earns links and mentions with stories: data, expert comment and launches that a journalist can use. This skill starts from the coverage competitors already earned, finds the story the user can own, and turns it into a 90-day plan that [find-journalists](../find-journalists/SKILL.md) executes one story at a time. A journalist wants a story, not a link request, so the plan is built around a beat and an angle.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_backlinks` and `seo_get_backlink_summary` (hosts often add a prefix, for example `mcp__manifold__seo_get_backlinks`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `seo_*` tools are not, the SEO tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- If the `reddit_*` tools are missing, that group is switched off: look for a story in the search trends alone, and say so. The plan needs no contacts, so the `leads_*` tools are not used here.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the competitors and their domains, the ICP, the proof points and data the user holds, the markets) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: links to specific pages, brand awareness, or authority for the whole domain. Default: authority for the domain, measured in referring domains from publications.
- **Stage**: pre-launch, just launched or established. It decides whether launch news is a story at all.
- **ICP**: who buys, so the plan targets what they read (trade press or national press).
- **Budget**: credits for the research (this plan costs about 120) and hours a week for pitching. Default: 1,000 credits and 3 hours a week. Say the research figure before starting; pass `max_credits` if the user gave a budget.
- **Team**: who pitches, who can be quoted as an expert, and whether anyone can run a survey or analyse data. Default: the founder, quoted as the expert.
- **Competitors**: two or three whose coverage shows who writes about the space. Default: the top three from `seo_get_serp_competitors`.
- **Assets**: data the user holds (usage numbers, a customer survey), a launch, a strong opinion. Default: none, so the plan builds one from public data.
- **Market**: `location` and `language` if not the United States and English.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 95 credits. `seo_get_backlink_summary` on the site and each competitor (10 credits each) for referring domains. Then the coverage competitors earned: `seo_get_backlinks` with `limit: 500` on each competitor domain (18 credits each). Keep rows whose `domain_from` is a publication and whose `page_title` reads as an article, not a directory or a list of tools. Each is a story that already mentioned a rival: note its title and `url_from`, and name what earned it (a data study, a funding round, a founder quote, a free tool).
3. **Gaps.** About 25 credits more. `seo_get_referring_domains` on the site (12 credits) shows which of those publications already link to the user; the rest are the gap. Then look for a story the user can own: `seo_search_keywords` on the category (10 credits) and read the `trend[12]` of the biggest terms for rising or falling demand, and `reddit_search_posts` with the category and `time_range: "year"` (1 credit a page) for the arguments people are having. A trend or a debate with numbers behind it is a data story.
4. **Tactics.** Choose from these, and say why each fits:
   - [find-journalists](../find-journalists/SKILL.md) for each story: the media list of reporters on the beat. Every story in the plan runs through it.
   - [find-best-of-lists](../find-best-of-lists/SKILL.md) for evergreen placements that do not need news.
   - [reclaim-lost-links](../reclaim-lost-links/SKILL.md) to recover press links the site already earned and broke.
   - [find-unlinked-mentions](../find-unlinked-mentions/SKILL.md) after each story runs: coverage that names the user without a link is the easiest link to ask for.
   - [find-backlink-targets](../find-backlink-targets/SKILL.md) for resource pages that link to data once it exists.
   - [find-podcasts](../find-podcasts/SKILL.md) when the expert can talk: shows to guest on.
   - For press around a launch day, [create-launch-plan](../create-launch-plan/SKILL.md).
5. **Plan.** A 30-60-90 day plan: by day 30, one reactive pitch (expert comment on a trend from step 3) to a media list; by day 60, one data story with its own list; by day 90, a second story and the reclaim of any broken press links. Give each period an owner and a KPI.
   - KPIs the tools can measure again later: referring domains from publications (`seo_get_referring_domains` on the site, filtered to the publication domains), links to the story page (`seo_get_backlinks` on its URL), and branded demand (`seo_get_keyword_metrics` on the brand name with `ai_volume: true`, 10 credits: `trend[12]` for Google, `ai_trend[12]` for AI engines).
6. **Deliver** one document: the inputs with defaults marked, the stories that earned each competitor coverage, the publication gap, two or three story ideas with the data behind each, the 30-60-90 table, and three first actions for this week.

## Judgment

- No story, no coverage. If the user has no asset and no opinion, the first 30 days build one from public data before any pitch goes out.
- Reactive comment is fastest: a trend in the news plus a quotable founder. A data study takes longer and earns more links.
- Data from these tools is public, so say where every number came from. A journalist checks.
- Coverage without a link still counts for brand demand and for AI answers. Report mentions, not only links.
- Keep the plan to what the team's hours allow. One story pitched well beats three pitched badly.
- The [outreach](../create-link-building-plan/references/outreach.md) floors, credits and handoff apply: never send a pitch; the deliverable is a document.

## Related skills

- The media list for each story, with a verified address per reporter: [find-journalists](../find-journalists/SKILL.md).
- Press around a launch day, its timing and its list: [create-launch-plan](../create-launch-plan/SKILL.md).
- Coverage that names the user without a link: [find-unlinked-mentions](../find-unlinked-mentions/SKILL.md). Press links the site earned and broke: [reclaim-lost-links](../reclaim-lost-links/SKILL.md).
- Evergreen placements that need no news: [find-best-of-lists](../find-best-of-lists/SKILL.md). A whole link plan: [create-link-building-plan](../create-link-building-plan/SKILL.md).
- Podcasts to guest on: [find-podcasts](../find-podcasts/SKILL.md).
