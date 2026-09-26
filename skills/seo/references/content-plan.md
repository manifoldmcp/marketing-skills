# Content plan

A content plan is a set of pages to write, grouped into clusters Google treats as one topic, in the order that pays back first. This playbook collects keyword candidates from the category and from competitors, drops what the site already covers, clusters the rest by the pages Google shows for them, reads the format each cluster needs, and ends in a schedule. Each page in it later gets its own [brief](brief.md).

## Inputs to settle first

- **Site**: the user's domain, and what it sells.
- **Seeds**: two or three words for the category and the problem it solves ("payroll software", "run payroll for restaurants"). Ask; the product name is not a seed.
- **Competitors**: two or three sites that rank for the topic. Default: the top three businesses from `seo_get_serp_competitors` on the site (10 credits).
- **Capacity and horizon**: pages the team can publish a month, and for how long. Default: four a month for three months, so the plan holds about 12 pages.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 3 x 10 + 3 x 10 + 10 + 15 = 85 credits, plus 10 if the competitors need finding. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Collect candidates.** `seo_search_keywords` for each seed (10 credits each, 100 rows). Use the default `mode: "suggestions"` for phrases that contain the seed, and `mode: "ideas"` for a new category where the seed itself has little volume. Then `seo_get_keyword_gap` with each competitor (10 credits each): the keywords they rank for and the site does not, with the `competitor_url` that ranks.
2. **Drop what the site already covers.** `seo_get_ranked_keywords` on the site (10 credits for the top 100 by traffic; `limit: 500` for 30 on a larger site). Keywords where the site already ranks in the top 20 go to [quick wins](quick-wins.md) or [content refresh](content-refresh.md), not to new pages.
3. **Filter.** Apply the [keyword floors](../SKILL.md#keyword-floors): volume, the site's reach in `kd`, and intent. Drop competitors' brand and login terms, topics the product has nothing to say about, and near-duplicates that differ by one word.
4. **Cluster by the page one.** Group keywords that one page can rank for. Word similarity is a guess; the test is Google's page one. For the head keyword of each candidate cluster, and for any pair you are unsure of, `seo_get_serp` (1 credit each): two keywords share a page when three or more of their top 10 URLs are the same. Name each cluster's pillar (the broad head term) and its supporting pages (the specific questions and use cases).
5. **Read intent and format.** From the same SERPs: what the top three are (a guide, a list, a template or tool, a product page, videos, forum threads with `type: "discussions_and_forums_element"`), and `features` such as `featured_snippet` and `ai_overview`. That format is what to write. If the top three are all tools, forum threads or videos, a blog post will struggle: say so, and suggest the format instead.
6. **Prioritise.** Score each cluster by total volume, `kd` against the site's reach, intent close to the product, and whether a competitor ranks with a page the team can beat. Put first the clusters that feed a money page (a comparison, a use case, a template that leads to the product). Fill the months to the team's capacity: the pillar first, then the supporting pages that link to it.
7. **Deliver** a table: month, cluster, page role (pillar or supporting), target keyword, secondary keywords, volume, KD, intent, the format that ranks, the competitor URL that ranks now, priority. Under it, the internal links: each supporting page links to its pillar, and the pillar to the money page.

## Judgment

- One keyword, one page. Two pages written for close variants split the rankings; the SERP overlap test in step 4 prevents it.
- Summed volumes overstate the prize. Variants of one query share searchers, and a page earns a share of its cluster, not the total.
- `seo_get_keyword_gap` sorts by volume, so its first rows are often a competitor's brand terms and generic head terms beyond the site's reach. Read past them.
- `kd` is a model of the links a page needs. For a new site, a cluster of low-KD questions beats one high-KD head term, and builds the reach to go after it later.
- Where `ai_overview` sits on most of a cluster's SERPs, the clicks will be lower than the volume says. Keep those clusters when they support a money page; drop them when traffic was the only reason.
- Content ideas for social channels, or across channels, belong to the `content` group; this plan is for pages meant to rank on Google.
