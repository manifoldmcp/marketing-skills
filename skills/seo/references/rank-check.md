# Rank check

Where a site ranks on Google right now, for a handful of keywords, once. The job is picking the cheapest method that answers the question, reading the result right (which page ranks, who sits above it), and handing back a dated table. Checking the same keywords every week is the `monitoring` group's [rank tracking](../../monitoring/references/rank-tracking.md), which builds on this playbook.

## Inputs to settle first

- **Target**: a domain (`example.com` covers every subdomain), one host (`www.example.com`), or one page (a full URL).
- **Keywords**: the list to check. If the user has none, take the top keywords from `seo_get_ranked_keywords` on the site (10 credits) and confirm them.
- **Competitors**: other sites to compare on the same keywords. Optional.
- **Market**: `location`, `language` and `device` (desktop or mobile). Rankings differ across all three; default United States, English, desktop.
- **Search Console**: for the user's own site, whether the `console_*` tools are there. They give the average position the site actually had, free. The [router](../SKILL.md#search-console) says how to check.
- **Budget**: 6 credits per keyword and target with `seo_get_position`: 10 keywords for one site is 60 credits. With competitors, `seo_get_serp` is cheaper: 10 keywords at `depth: 30` is 30 credits for every site at once. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the method.**
   - One target, up to about 15 keywords: `seo_get_position` per keyword (6 credits each). It scans the top 100 and returns the rank, the ranking URL and who is above.
   - Several targets on the same keywords: `seo_get_serp` per keyword with `depth` deep enough to reach them, 20 or 30 (2 or 3 credits). Every site's position is on one page. Use `seo_get_position` only for a target that is not within the depth.
   - Every keyword a site ranks for, not a chosen list: `seo_get_ranked_keywords` (10 credits per 100 rows). It is an index snapshot, refreshed on the provider's schedule and cached for 7 days, not a live check.
   - The user's own site with Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["query"]` and `limit: 1000`, then pick the keywords from the rows; add `device` to `dimensions` to split mobile and desktop (free). Every entry in `filters` must match, so filter on one query per call if you filter at all. This is an average over the window, not a snapshot; report it next to the live check, not instead of it.
2. **Run it** with the market from the inputs on every call, so all rows are comparable.
3. **Read the result.** `rank` counts every item on the page, including features; `organic_rank` counts organic results only. Report both where they differ, since a rank of 7 can be the third organic result under an AI overview and a video pack. `rank` null means not in the top 100. Check `url`: the wrong page ranking (a blog post for a product query, an old page after a migration) is a finding in itself. From `above[]`, or the SERP rows, name who sits directly ahead. From `seo_get_serp`, note `features`; add `ai_overview: true` (+1 credit) when the user asks whether an AI overview shows.
4. **Deliver** a table: keyword, target, rank, organic rank, ranking URL, the result directly above, SERP features, and the date, location and device checked. Save nothing on the server: if the user wants to compare next month, the host keeps the table; if they want it every week, hand over to [rank tracking](../../monitoring/references/rank-tracking.md).

## Judgment

- A rank is a snapshot from one location on one device. The user's own search is personalised and will differ; that is not an error in the check.
- `seo_get_position` and `seo_get_serp` results are cached for 24 hours. Checking again the same day returns the same answer, free for this account.
- Search Console's average position blends every query variant, location and device over the window. A 9.6 there and a 6 in a live check can both be right.
- For more than about 20 keywords on one site, `seo_get_ranked_keywords` answers most of them for a fraction of the cost; run `seo_get_position` only on the ones that matter most.
- A domain target counts any subdomain. If the user asks about their blog on `blog.example.com` alone, pass that host.
