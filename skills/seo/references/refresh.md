# Refreshing, merging and retiring pages

Pages that decayed or sit on page two and three, and pairs of the site's own pages competing for one keyword. Existing pages are cheaper to lift than new ones: they are indexed, they have links, and Google already ties them to a topic. Checked October 2026.

## The evidence on refreshes

- **Updating works through substance, not the date.** Google lists changing a date without substantially changing the page as a sign of search-engine-first content (helpful content guidance, October 2026), and date-only "2026" refreshes were among the patterns at the SaaS sites that lost visibility in January 2026 ([updates](google-updates.md#self-ranking-listicles-contested)). For AI answers, pages cited are far more often recently updated than recently published (see [ai-search](../../ai-search/SKILL.md)).
- **Unknown:** no controlled study since 2025 measures how much a refresh lifts rankings or how long it takes. Practitioner case studies are uncontrolled and published by the people who sold the refresh. Promise the work, not a lift.

## Finding the candidates

- **With Search Console:** `dimensions: ["page"]` and `limit: 1000` for the last three months and the same three months a year earlier. Keep pages that lost 30% or more of their **clicks** from a base of at least about 100. Do not use impressions year over year across September 2025: the `num=100` change cut them for most sites with no real loss ([Search Console](search-console.md#the-september-2025-break)). Add pages averaging position 11 to 30 with high impressions: page two and three with demand.
- **Without it:** `seo_get_ranked_keywords` on the domain, `limit: 500` (30 credits). Per `url`, sum the `volume` of keywords ranking 11 to 30 against the estimated `traffic` it earns; the biggest gaps are under-performing on topics Google already gives them. Estimates cannot see decay over time; ask the user what their analytics shows falling.
- **Check demand before blaming the page.** `seo_get_keyword_metrics` (10 credits for up to 100 keywords): if `trend[12]` fell, the topic decayed, not the page, and a refresh will not bring the traffic back. Check also whether an AI Overview arrived on the main keyword (`seo_get_serp` with `ai_overview: true`): clicks falling with position and impressions steady is the overview, not decay.

## What to change

For each page's main keyword, compare the page with the top three (`seo_get_page`, free) and read the top result's other keywords (`seo_get_ranked_keywords` on its URL, 10 credits): `h2` sections two or more winners share that the page lacks, subtopics the winner ranks for, whether the title still matches the query, stale facts, prices, screenshots and years in the body (the host reads these if it can open the page). If the winners are a different format, the refresh is a rewrite into that format, which is a [brief](briefs.md) for the same URL.

A page at 12 moves with a new section; a page at 45 usually needs a rewrite. Refresh the pages closest to page one first.

## Refresh, merge or retire

- **Refresh** when the page matches the intent and lacks sections or currency. Keep the URL.
- **Merge** when two of the site's pages split one keyword (see below): keep the stronger URL, fold in the other's unique sections, 301 the old one.
- **Retire** when a page has no rankings, no links and no fit with the product. Before merging or retiring, `seo_get_backlinks` on the URL (12 credits): a page with links is redirected to its closest match, never deleted.

## Cannibalisation

Two of the site's pages aimed at one keyword split clicks and links, and Google swaps which one it shows. It is not a penalty: Google chooses which of the site's pages to show, and two of them on page one is a win, not a problem.

- **Finding pairs with Search Console:** `dimensions: ["query", "page"]`, 90 days, following `cursor`. Keep queries where two or more pages each hold at least about 10% of the query's impressions (a rule of thumb). Run the 90 days before as well: a query whose top page changed between windows is a pair even if one page now holds it.
- **Without it:** `seo_get_ranked_keywords` on the domain with `limit: 1000` (55 credits); rows with the same `keyword` and a different `url`. The index may keep only the best-ranking URL per keyword, so also run it on each suspect page's URL (10 credits each) and compare.
- **Keep only pairs that cost traffic:** both pages below the top three, or Google showing the wrong page. `seo_get_position` (6 credits) shows the page Google shows today in `url`; a ranking URL that flips between runs in [tracking](tracking.md) is the same finding.
- **Decide:** merge when both pages serve the same intent (keep the URL with more clicks and links); retarget when the intents differ (a guide and a product page): give each its own keyword in `title` and `h1`, cut the overlap from the weaker one, and point every internal link with that anchor at the owner; leave it when both hold page one or the overlap is long-tail only.
- **Do not fix it with `noindex` or a canonical between pages that differ.** Google's docs (July 2026) treat a canonical as a strong signal for duplicate or very similar pages, decided by content similarity, and advise against noindex for choosing a canonical. Merge or retarget.
- Internal links decide ties. If the blog links to the guide with the keyword as anchor and the navigation links to the product page, Google gets two answers.

## Deliverable

A table: URL, main keyword, volume, position now, clicks lost or traffic gap (and which source), the top result and its format, sections to add, other fixes, action (refresh, merge into which URL, retarget to which keyword, retire), links on any page that goes, priority. For each refreshed page, the date the change ships, so the re-check compares like with like.
