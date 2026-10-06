---
name: create-seo-content-plan
description: When the user wants a plan of blog posts and pages to write so the site ranks on Google. Collects keyword candidates from the category and the competitors' keyword gap with DataForSEO, drops what the site already ranks for, clusters the rest by shared page-one results, reads the format each cluster needs, and hands back a month-by-month schedule of pillar and supporting pages with a target keyword each. Also use when the user mentions a content plan, what should we blog about, topic clusters, pillar pages, keyword research for our blog, an SEO editorial plan, or plan our blog around the keywords a competitor ranks for and we don't. One page's outline goes to write-seo-brief, the overall SEO plan to create-seo-plan, social post ideas to find-content-ideas, a calendar across channels to create-content-calendar.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# SEO content plan

A content plan is a set of pages to write, grouped into clusters Google treats as one topic, in the order that pays back first. This skill collects keyword candidates from the category and from competitors, drops what the site already covers, clusters the rest by the pages Google shows for them, reads the format each cluster needs, and ends in a schedule. Each page in it later gets its own [content brief](../write-seo-brief/SKILL.md).

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `seo_get_keyword_gap` (hosts often add a prefix, for example `mcp__manifold__seo_search_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Then follow the tool checks in the [Google search notes](../create-seo-plan/references/platforms/google.md#tools): the `seo_*` tools are required, the `console_*` tools optional.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, what it sells, the ICP, the competitors, the market, customer language for seeds) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Site**: the user's domain, and what it sells.
- **Seeds**: two or three words for the category and the problem it solves ("payroll software", "run payroll for restaurants"). Ask; the product name is not a seed.
- **Competitors**: two or three sites that rank for the topic. Default: the top three businesses from `seo_get_serp_competitors` on the site (10 credits).
- **Capacity and horizon**: pages the team can publish a month, and for how long. Default: four a month for three months, so the plan holds about 12 pages.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 3 x 10 + 3 x 10 + 10 + 15 + 11 = 96 credits, plus 10 if the competitors need finding. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Collect candidates.** `seo_search_keywords` for each seed (10 credits each, 100 rows). Use the default `mode: "suggestions"` for phrases that contain the seed, and `mode: "ideas"` for a new category where the seed itself has little volume. Then `seo_get_keyword_gap` with each competitor (10 credits each): the keywords they rank for and the site does not, with the `competitor_url` that ranks.
2. **Drop what the site already covers.** `seo_get_ranked_keywords` on the site (10 credits for the top 100 by traffic; `limit: 500` for 30 on a larger site). Keywords where the site already ranks in the top 20 go to [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md) or [refresh-content](../refresh-content/SKILL.md), not to new pages.
3. **Filter.** Apply the [keyword floors](../create-seo-plan/references/platforms/google.md#keyword-floors): volume, the site's reach in `kd`, and intent. Drop competitors' brand and login terms, topics the product has nothing to say about, and near-duplicates that differ by one word.
4. **Cluster by the page one.** Group keywords that one page can rank for. Word similarity is a guess; the test is Google's page one. For the head keyword of each candidate cluster, and for any pair you are unsure of, `seo_get_serp` (1 credit each): two keywords share a page when three or more of their top 10 URLs are the same. Name each cluster's pillar (the broad head term) and its supporting pages (the specific questions and use cases).
5. **Read intent and format.** From the same SERPs: what the top three are (a guide, a list, a template or tool, a product page, videos, forum threads with `type: "discussions_and_forums_element"`), and `features` such as `featured_snippet` and `ai_overview`. That format is what to write. If the top three are all tools, forum threads or videos, a blog post will struggle: say so, and suggest the format instead.
6. **Prioritise.** Score each cluster by total volume, `kd` against the site's reach, intent close to the product, and whether a competitor ranks with a page the team can beat. Where `ai_overview` shows on a cluster's SERPs, `seo_get_keyword_metrics` with `ai_volume: true` on the cluster head terms (about 11 credits for 20; `seo_search_keywords` does not return `ai_volume`) says how often AI engines get the same question: a high `ai_volume` makes the cluster an AI search play as well ([create-ai-search-plan](../create-ai-search-plan/SKILL.md)), since the clicks will go to whoever the answer cites. Put first the clusters that feed a money page (a comparison, a use case, a template that leads to the product). Fill the months to the team's capacity: the pillar first, then the supporting pages that link to it.
7. **Deliver** a table: month, cluster, page role (pillar or supporting), target keyword, secondary keywords, volume, KD, intent, the format that ranks, the competitor URL that ranks now, priority. Under it, the internal links: each supporting page links to its pillar, and the pillar to the money page.

## Judgment

- Follow the [Google search notes](../create-seo-plan/references/platforms/google.md): [Search Console](../create-seo-plan/references/platforms/google.md#search-console) against estimates, the [keyword floors](../create-seo-plan/references/platforms/google.md#keyword-floors), the [credits](../create-seo-plan/references/platforms/google.md#credits) and the [handoff](../create-seo-plan/references/platforms/google.md#handoff).
- One keyword, one page. Two pages written for close variants split the rankings; the SERP overlap test in step 4 prevents it.
- Summed volumes overstate the prize. Variants of one query share searchers, and a page earns a share of its cluster, not the total.
- `seo_get_keyword_gap` sorts by volume, so its first rows are often a competitor's brand terms and generic head terms beyond the site's reach. Read past them.
- `kd` is a model of the links a page needs. For a new site, a cluster of low-KD questions beats one high-KD head term, and builds the reach to go after it later.
- Where `ai_overview` sits on most of a cluster's SERPs, the clicks will be lower than the volume says. Keep those clusters when they support a money page; drop them when traffic was the only reason.
- **Pages, not posts.** This plan is for pages meant to rank on Google. Ideas for social channels belong to [find-content-ideas](../find-content-ideas/SKILL.md), and one calendar across channels to [create-content-calendar](../create-content-calendar/SKILL.md).

## Related skills

- A brief for each page in the plan: [write-seo-brief](../write-seo-brief/SKILL.md).
- Keywords the site already ranks 4 to 20 for: [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md). Old pages to lift: [refresh-content](../refresh-content/SKILL.md).
- Where the plan sits in a quarter's SEO work: [create-seo-plan](../create-seo-plan/SKILL.md). Comparison and alternatives pages: [plan-comparison-pages](../plan-comparison-pages/SKILL.md).
- Social post ideas and a calendar across channels: [find-content-ideas](../find-content-ideas/SKILL.md), [create-content-calendar](../create-content-calendar/SKILL.md).
