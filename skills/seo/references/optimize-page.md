# Optimize a page

One existing URL, measured against the keyword it should rank for and the pages that beat it. This playbook confirms which keyword the page targets, checks that nothing technical holds it back, compares it with the top results element by element, and ends in a change list for that one page. For a new page, use a [brief](brief.md); for many pages at once, [quick wins](quick-wins.md) or [content refresh](content-refresh.md).

## Inputs to settle first

- **URL**: the page. As a `target`, the full URL means that page only and the bare domain means the whole site; each step says which it passes.
- **Keyword**: the query the page should win. Default: chosen in step 1 from what the page already ranks for.
- **Search Console**: whether the `console_*` tools are there. This job is better with them: the page's real queries, impressions and CTR. The [router](../SKILL.md#search-console) says how to check.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: about 10 + 6 + 2 + 10 = 28 credits without Search Console, about 18 with it, plus 20 for the link comparison in step 5 when on-page is already level; `seo_get_page` is free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **What the page ranks for.**
   - With Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["query"]` and a `filters` entry of dimension page equal to the URL, for the last 90 days (free): clicks, impressions, `ctr` and `position` per query.
   - Without it: `seo_get_ranked_keywords` with the URL as `target` (10 credits): keyword, `rank`, `volume` and estimated `traffic`.

   Pick the target keyword: the one with the most volume whose intent the page serves. Then `seo_get_position` with that keyword and the domain as `target` (6 credits). If its `url` is another page of the site, the two pages compete: decide which one owns the keyword before changing either.
2. **Read page one.** `seo_get_serp` for the keyword with `ai_overview: true` (2 credits): the format and titles of the top three, `features`, and whether the `ai_overview` cites anyone.
3. **Check the page against the winners.** `seo_get_page` on the URL and on the top three results (free). Go element by element:
   - Indexable: `status` 200, `canonical` pointing at the page itself, no noindex in `robots_meta`. Any failure here comes first.
   - `title`: the keyword or a close variant near the start, under about 60 characters, a reason to click the others lack.
   - `meta_description`: present, under about 155 characters, answers the query.
   - `h1`: one, matching the query. `h2`: the sections the winners share that the page lacks.
   - `word_count` against the winners' median, `schema_types` against theirs, `images_without_alt`, and `links_internal`.
4. **Find the missing subtopics.** `seo_get_ranked_keywords` on the top result's URL (10 credits): the keywords it ranks for that this page does not. Each is a question or section to add.
5. **Compare links, when on-page is level.** If the page already matches the winners on intent, format and sections, the gap is usually links. `seo_get_backlink_summary` on the URL and on the top result (10 credits each): compare `referring_domains`. A large gap goes to the `link-building` group's [backlink targets](../../link-building/references/backlink-targets.md) playbook, with this URL as the page that wants links.
6. **Deliver** a table: element, now, recommended, evidence (which winners do it, or which keyword asks for it), priority. Above it: the keyword, its volume, the position now and its source, so the next check has a baseline.

## Judgment

- Keep the URL. A new URL restarts the page's history; if it must change, it gets a 301.
- Match the intent before anything else. A product page will not rank for a query whose top ten are guides, however good its title.
- A low CTR at a good position is a title and snippet problem; a low position with a good CTR is a content or links problem. Search Console shows which; without it, compare the `title` with the winners'.
- Change the title and the content in one pass, then wait. Changing again every week makes it impossible to tell what worked. Re-check in two to four weeks with [rank check](rank-check.md).
- The tools cannot read the page's body text. If the host can open the page, it can judge the copy; otherwise the headings and word count are the evidence.
