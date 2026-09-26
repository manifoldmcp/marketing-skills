# Quick wins

The cheapest organic traffic is on keywords the site already ranks 4 to 20 for: Google already thinks the page is relevant, and a better title, a missing section or an internal link can move it up. This playbook finds those keywords, keeps the ones with volume and intent that matter, and ends in a fix list per page.

## Inputs to settle first

- **Site**: the user's domain, or one section of it.
- **Search Console**: whether the `console_*` tools are there. This job is much better with them, because impressions show demand that the estimates miss. The [router](../SKILL.md#search-console) says how to check.
- **Focus**: money pages only, the blog only, or everything. Default: everything, money pages first.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: about 30 + 10 + 5 = 45 credits without Search Console, about 15 with it; `seo_get_page` is free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the keywords.**
   - With Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["query", "page"]`, the last 90 days as `start_date` and `end_date`, and `limit: 1000` (free). Keep rows with `position` from 4 to 20 and at least about 100 impressions in the window. Also keep rows in positions 1 to 3 whose `ctr` is under about half of what the others in that position earn on the site: that is a title problem, not a rank problem.
   - Without it: `seo_get_ranked_keywords` on the domain with `limit: 500` (30 credits). Keep rows with `rank` from 4 to 20.
2. **Filter.** Apply the [keyword floors](../SKILL.md#keyword-floors): volume, intent and the site's reach. Drop other brands' navigational keywords and keywords where the ranking `url` cannot serve the intent (a blog post ranking for a "pricing" query needs a different page, not a fix). Rank what is left: positions 4 to 10 first (moving from 8 to 4 roughly doubles the clicks), then 11 to 20, then by volume and intent.
3. **Group by page.** One page usually carries several of these keywords. Fix per page, and read every keyword the page ranks for before changing its title, so a rewrite for one keyword does not cost another that already brings clicks. Where two URLs of the site rank for the same keyword, flag it: the fix is to pick one page, not to tune both.
4. **See what beats it.** `seo_get_serp` for the main keyword of the top 10 pages (1 credit each). Note the format of the top three, the titles that win, and `features` (`featured_snippet`, `people_also_ask`, `video`, `ai_overview`). If the top three are a different format from the user's page, a title fix will not do: move the page to the [content refresh](content-refresh.md) list.
5. **Read the pages.** `seo_get_page` on each page (free, rate limited). Check that `status` is 200, `canonical` points at the page itself and `robots_meta` does not say noindex. Then the fixes: the keyword or a close variant near the start of `title`, a title under about 60 characters, a `meta_description` that answers the query in under about 155 characters, one `h1`, and `h2` headings for the subtopics the winners cover.
6. **Add internal links.** `seo_get_domain_overview` on the site (5 credits): its `top_pages` are the strongest pages. Suggest one or two links from a related top page to each fixed page, with the keyword as anchor text. The tools cannot see which pages already link to it; the user checks before adding.
7. **Deliver** a table, one row per page: URL, main keyword, other keywords in range, volume, current position (say whether Search Console or estimate), intent, what ranks above (format), the fix (new title, new meta description, a missing section, an internal link from which page), and effort (minutes or a rewrite).

## Judgment

- Search Console's `position` is an average over every impression, location and device. A 9.6 can be 4 on desktop and 15 on mobile; check the keywords that matter with `seo_get_position` if the fix depends on it.
- Few impressions make noise. A keyword with 30 impressions in 90 days at position 6 is not a win yet.
- Google rewrites titles it finds unhelpful. A title that matches the page's `h1` and the query is rewritten less often.
- Title and meta fixes move clicks in days to weeks; section additions and links take weeks to months. Re-check after two to four weeks with [rank check](rank-check.md), or schedule it with the `monitoring` group.
- A keyword at 11 to 20 that needs a whole new section is a refresh, not a quick win. Keep this list to changes of an hour or less per page.
- If every candidate sits on one page type that has a technical problem (noindex, a wrong canonical, a slow template), the win is the [audit](audit.md), not the titles.
