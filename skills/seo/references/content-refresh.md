# Content refresh

Pages the site already has are cheaper to lift than new ones: they are indexed, they have links, and Google already ties them to a topic. This playbook finds the pages that decayed or sit on page two, sees what outranks each one, and ends in a refresh list that says what to add to each page, or whether to merge or retire it.

## Inputs to settle first

- **Site**: the user's domain, or the section to refresh (the blog, the guides).
- **Search Console**: whether the `console_*` tools are there. This job is much better with them, because only clicks over time show decay; the estimates show where pages rank now. The [router](../SKILL.md#search-console) says how to check.
- **Capacity**: pages the team can refresh a month. Default: four, so the list stops at the ten best.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: about 30 + 10 x (1 + 10) = 140 credits for ten pages without Search Console, about 110 with it, plus 12 for each page you plan to merge or retire; `seo_get_page` is free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the candidates.**
   - With Search Console: `console_list_properties`, then `console_get_search_analytics` with `dimensions: ["page"]` and `limit: 1000` for the last three months and for the same three months a year earlier (two calls, free). Keep pages that lost 30 percent or more of their clicks from a base of at least about 100 clicks. Add pages whose average `position` is 8 to 20 with high impressions: page two with demand.
   - Without it: `seo_get_ranked_keywords` on the domain with `limit: 500` (30 credits), filtered to the section's URLs. Per `url`, add up the `volume` of keywords ranking 4 to 30 and the estimated `traffic` it earns. Pages with the biggest gap between the two are under-performing on topics Google already gives them. Ask the user for pages their analytics shows falling; the estimates cannot see decay over time.
2. **Pick each page's main keyword.** The keyword with the most volume that the page's topic answers. For the ten best candidates, keep the page's other ranking keywords as well: a refresh must not lose them.
3. **See what outranks it.** `seo_get_serp` for each main keyword (1 credit each). Note the top three: their format, their titles (a year in a title signals freshness wins here), and `features`. If the winners are a different format from the page, the refresh is a rewrite into that format.
4. **Compare the pages.** `seo_get_page` on the user's page and the top three results (free, rate limited). Headings in `h2` that two or more winners share and the page lacks are the sections to add. Compare `word_count` with the winners' median, `schema_types` (FAQPage, HowTo, Article, Product), and whether the `title` still matches the query.
5. **Find the missing subtopics.** `seo_get_ranked_keywords` on the top result's URL (10 credits each, the URL as `target`). Keywords it ranks for that the user's page does not are the questions and subtopics Google rewards for this topic.
6. **Decide per page.** Refresh when the page matches the intent and lacks sections. Merge when two pages of the site split one keyword: keep the stronger URL, fold the other in and 301 it. Retire when a page has no rankings, no links and no fit with the product. Before retiring or merging, check links with `seo_get_backlinks` on the URL (12 credits): a page with links is redirected, never deleted.
7. **Deliver** a table: URL, main keyword, volume, position now, clicks lost or traffic gap (say which source), the top result and its format, sections to add (from the headings and keywords), other fixes (title, schema, a stale year), action (refresh, merge into which URL, retire), priority.

## Judgment

- A refresh keeps the URL. Changing it throws away the page's history and links; if it must change, it is a 301 and a line in the table.
- Update the visible date only when the content changed. Google and readers both notice a new date on an old page.
- Word count is a symptom. Add the sections the winners cover; do not pad to their length.
- If demand fell (`trend[12]` from `seo_get_keyword_metrics`, 10 credits for up to 100 keywords), the page did not decay, the topic did. Refreshing it will not bring the traffic back.
- The tools cannot read the body text or the publish date. If the host can open the page, it can find stale facts, dead screenshots and old prices; otherwise the user checks them.
- Refresh the pages closest to page one first. A page at 12 moves with a new section; a page at 45 usually needs a rewrite, which is a [brief](brief.md) for its keyword.
