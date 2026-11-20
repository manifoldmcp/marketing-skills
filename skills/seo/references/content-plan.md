# A content plan

A set of pages to write, grouped into clusters Google treats as one topic, in the order that pays back first. Each page later gets a [brief](briefs.md). This plan is for pages meant to rank in Google; social posts and a calendar across channels belong to [growth-strategy](../../growth-strategy/SKILL.md). Checked October 2026.

## What the evidence changes

- **Fewer, better pages.** Google's AI optimisation guide (July 2026) says more pages do not make a site better and a page per query variation is scaled content abuse; its systems understand synonyms. The sites that won the 2025 to 2026 updates were focused, had a product and owned assets ([updates](google-updates.md#what-winners-had)). A plan of 40 near-identical posts is the pattern that lost.
- **Commodity topics are the weakest bet.** Informational head terms now carry AI Overviews on most results and lose more than half their top-of-page clicks ([keyword data](keyword-data.md#clicks-per-position)). Keep an informational cluster when it feeds a money page or the site has an asset nobody else has for it; drop it when traffic was the only reason.
- **Topical focus is a measured winner trait** (76% of winners against 61% of losers, Zyppy 2026), so prefer depth in the product's own topics over reach into adjacent ones.

## Candidates

- `seo_search_keywords` per seed (10 credits each, 100 rows). The default `mode: "suggestions"` returns phrases containing the seed; `mode: "ideas"` for a new category where the seed itself has little volume; `mode: "related"` for SERP-similar terms. Seeds are the category and the problem in buyers' words, never the product name.
- `seo_get_keyword_gap` against each competitor (10 credits each): keywords they rank for and the site does not, with `competitor_url`. It sorts by volume, so the first rows are usually the competitor's brand terms and head terms beyond reach; read past them.
- Drop what the site already ranks for: `seo_get_ranked_keywords` on the site. Keywords in its top 20 go to [quick wins](quick-wins.md) or [refresh](refresh.md), not new pages.
- Filter by the [keyword floors](keyword-data.md#keyword-floors), and drop competitors' brand and login terms, topics the product has nothing to say about, and one-word variants.

## Clusters

- **Cluster by Google's page one, not by wording.** Two keywords belong on one page when three or more of their top 10 URLs match (`seo_get_serp`, 1 credit each, on each cluster head and any pair in doubt). Very young SERPs churn week to week; there, grouping by wording is the fallback, and say so.
- Name each cluster's pillar (the broad term) and its supporting pages (specific questions and use cases). Each supporting page links to its pillar; the pillar links to the money page.
- **Format from the same SERPs.** The top three's format is what to write. If they are all tools, forum threads (`type: "discussions_and_forums_element"`) or videos, a blog post will struggle: say so and suggest the format. Lists rank where the query asks for options and rarely otherwise.

## Priority

Score each cluster by volume (as buckets, not sums), `kd` against the site's reach, intent close to the product, whether an asset the site owns can anchor it, and whether a competitor ranks with a page the team can beat. Where `ai_overview` shows on most of its SERPs, `seo_get_keyword_metrics` with `ai_volume: true` on the head terms (about 11 credits for 20) shows how often AI engines get the same question; a high value makes the cluster an [ai-search](../../ai-search/SKILL.md) job as well, since the clicks go to whoever the answer cites. Clusters that feed a comparison, a use case or a template leading to the product go first.

Size the months to the team: one good page a week beats five thin ones. A new site starts with low-KD questions, which build the reach for the head term later.

## Cost and deliverable

About 100 credits for three seeds and three competitors with SERP checks on the cluster heads, plus 10 if the competitors need finding (`seo_get_serp_competitors` returns sites that share results, often publishers, so ask the user for business rivals first).

The table: month, cluster, page role (pillar or supporting), target keyword, secondary keywords, volume, KD, intent, the format that ranks, the competitor URL that ranks now, the asset that anchors it, priority. Under it, the internal links.
