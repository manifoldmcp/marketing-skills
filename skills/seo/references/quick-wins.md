# Quick wins and one-page fixes

Pages the site already ranks 4 to 20 with, and one URL tuned against the pages that beat it. Google already finds these pages relevant, so a better title, a missing section or an internal link can move them. Checked October 2026.

## What a win is worth

The click curve is steep at the top and almost flat below it ([keyword data](keyword-data.md#clicks-per-position)): position 3 earns about three to four times position 5 on desktop, and positions 6 to 10 earn under 1% each. So rank the candidates by how close each is to the top three, not by how many places it could climb, and by volume and intent after that. A keyword at 11 to 20 that needs a whole new section is a refresh ([refresh](refresh.md)), not a quick win; keep this list to changes of an hour or less per page.

## Finding them

- **With Search Console**: `console_get_search_analytics` with `dimensions: ["query", "page"]`, the last 90 days and `limit: 1000`. Keep rows at `position` 4 to 20 with at least about 100 impressions in the window; 30 impressions at position 6 is noise. Also keep rows at 1 to 3 whose `ctr` is under about half what the site's other queries earn at that position: a title or snippet problem, not a rank problem.
- **Remember what the rows miss.** Anonymised queries are dropped from any query table, often most of a small site's clicks ([Search Console](search-console.md#what-is-hidden)), and impressions at deep positions are partly machines. Rank on clicks and on positions under 20.
- **Without it**: `seo_get_ranked_keywords` on the domain with `limit: 500` (30 credits), rows with `rank` 4 to 20. An index snapshot: check the few that matter with `seo_get_position` (6 credits) before rewriting for them.
- Apply the [keyword floors](keyword-data.md#keyword-floors). Drop rows where the ranking page cannot serve the intent (a blog post ranking for a pricing query needs a different page, not a fix).

## Per page, not per keyword

One page usually carries several of these keywords. Read every keyword the page ranks for before changing its title, so a rewrite for one does not cost another that already brings clicks. Where two of the site's URLs rank for the same keyword, leave it out of this list: that is [cannibalisation](refresh.md#cannibalisation).

## The checks, in order

For each page, `seo_get_page` on it and on the top three results (free, rate limited), and `seo_get_serp` with `ai_overview: true` for its main keyword (2 credits):

1. **Indexable.** `status` 200, `canonical` pointing at itself, no noindex in `robots_meta`. Any failure comes first and goes to [technical-seo](../../technical-seo/SKILL.md).
2. **Format and intent.** If the top three are a different format from the page (tools, forum threads, videos, a list), a title fix will not do; it is a rewrite.
3. **Title.** The keyword or a close variant near the start, under about 60 characters, with a reason to click the others lack. Google rewrites titles it finds unhelpful; a title that agrees with the `h1` and the query is rewritten less.
4. **Meta description.** Present, answers the query in under about 155 characters. Google often shows its own snippet; this is the default it falls back to.
5. **Headings.** One `h1` matching the query; `h2` sections for the subtopics two or more winners share. `seo_get_ranked_keywords` on the top result's URL (10 credits) lists the other queries it wins: each is a question the page could answer.
6. **Internal links.** `seo_get_domain_overview` (5 credits) gives the site's strongest pages in `top_pages`. Suggest one or two links from a related strong page with the keyword as anchor text. The tools cannot see existing inbound internal links; the user checks.
7. **Links, only when on-page is level.** If the page already matches the winners on intent, format and sections, the gap is usually links: `seo_get_backlink_summary` on the page and the top result (10 credits each), compare `referring_domains`. A large gap goes to [link-building](../../link-building/SKILL.md).

The tools cannot read the body copy. If the host can open the page, it judges the copy (stale facts, a missing direct answer); otherwise headings and `word_count` are the evidence.

## Rules

- **Keep the URL.** A new URL restarts the page's history; if it must change, 301 it.
- **Change once, then wait.** Titles and snippets move clicks in days to weeks; sections and links in weeks to months. Changing again every week makes it impossible to tell what worked. Re-check after two to four weeks ([tracking](tracking.md)), and ask the user to mark the change with a Search Console annotation.
- **Low CTR at a good position is the snippet; low position with good CTR is content or links.** Search Console shows which.
- **Where an AI Overview answers the query,** put a direct two-sentence answer near the top. It is what the overview can quote, and the click that remains wants more than the overview gave.
- **Many candidates on one template with one fault** (noindex, a wrong canonical, a slow template) is a technical fix, not forty title rewrites.

## Deliverable

One row per page: URL, main keyword, other keywords in range, volume, position (and whether measured or estimated), intent, the format above it, the fix (new title, new meta description, a missing section, an internal link from which page), and effort. Titles and descriptions are drafts the user approves.
