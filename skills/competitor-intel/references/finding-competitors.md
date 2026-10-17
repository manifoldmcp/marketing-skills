# Finding competitors

Who the user really competes with: the companies buyers weigh them against, the other ways buyers get the job done, and the sites that only take their search traffic. Checked October 2026.

## Three classes

- **Direct:** the same job for the same buyer. These go on a battlecard and into a plan.
- **Indirect:** the same job done another way: a spreadsheet, an agency, a feature inside a bigger platform, hiring someone, doing nothing. For an early product the biggest competitor is often here, and it usually has no domain. Keep it in the table anyway.
- **Search-only:** competes for the user's keywords, not their buyers: publishers, marketplaces, review sites, directories, forums, video sites and vendors in adjacent categories. Matters for SEO, never for sales.

Carry two or three direct competitors into the next step. A teardown of ten is expensive and nobody reads it.

## Sources vote

No single source finds the competitive set. A company named by several independent sources is a real competitor whatever its traffic; a domain found only by search overlap is search-only until something else names it.

| Source | What it finds | Trap |
|---|---|---|
| Search overlap (`seo_get_serp_competitors`, 10 credits for 100 rows) | Who takes the same Google results | Top rows are platforms and publishers. For pipedrive.com, 4 of the top 24 sold a CRM ([field notes](field-notes.md)) |
| AI answers (`aeo_run_ai_answers`, 27 a prompt on five engines) | The shortlist engines hand a buyer who asks | `mentions[]` only detects the brands passed in `brands`; read each `answer` for the rest. Brands that recur across engines matter; one engine's long tail is noise |
| Comparison searches (`seo_search_keywords` with "<rival> vs" as `seed`, 10) | Who buyers weigh against the best-known rival, and how often (`volume`) | A brand in several "vs" keywords is in the buyer's set whatever its own size |
| Category ads (`ads_search_ads`, 1 a page per library) | Who pays for the category's words now | Only Facebook honours `active_only`; on LinkedIn and TikTok read `last_shown`, which LinkedIn often leaves empty |
| Reddit (`reddit_search_posts` and `reddit_search_comments`, 1 each) | Who people actually switch to, and the indirect options ("we use a spreadsheet") | A ranked sample, never complete. Count names across threads |
| A best-of list's headings (`seo_get_serp` then `seo_get_page`, 1 plus free) | The vendors a list ranks | Many lists are written by one of the vendors |

## Why AI answers now matter most

- Buyers arrive with a shortlist already formed: four of their five final vendors were on it on day one of a formal evaluation, and the top-ranked vendor won about 77% of deals (6sense, 10,000+ buyers, July 2026, vendor survey with no method published). Sales sees the competitive set after it is chosen.
- AI engines hand out a stable core and a shuffled tail. Brand lists almost never repeat exactly, but a core recurs in most runs (SparkToro and Gumshoe, 2,961 runs, late 2025). On "best Pipedrive alternatives" all five engines named HubSpot, Zoho, Close and monday.com, four named Freshsales, Salesforce and Attio, and Claude alone added six smaller CRMs ([field notes](field-notes.md)).
- The engines build that list from list pages, and a competitor's own "alternatives to X" page is one of them: Attio's Pipedrive alternatives page was cited by Perplexity and Google's AI Overview. A rival that writes the page about the user's category writes part of the shortlist.
- So a competitor the user has never heard of, named on several engines, is the finding of the run. Say it first. Measuring this properly (prompt sets, margins of error, per engine) is [ai-search](../../ai-search/SKILL.md).

## Classifying a candidate

1. `seo_get_page` on the homepage (free): `title`, `meta_description` and `h1[]` say what it sells and to whom. The same job for the same buyer makes it direct.
2. Where that is unclear, `leads_get_company` (1 credit) for `description`, `industry` and `keywords[]`. The description can be old; `linkedin_get_company` `bio` is the current one.
3. Size it last, as a signal and not a ranking: `organic_traffic` from `seo_get_domain_overview` (15 each) for up to about three domains, `seo_get_traffic_estimates` above that (50 plus 50 per 100 domains). Traffic sizes SEO-led companies well and sales-led ones badly: an enterprise vendor can have little traffic and a large sales team ([reading the numbers](estimates.md)).

## Cost

A default run (homepage, search overlap, two SERPs, three AI prompts, one "vs" keyword pull, three ad libraries, three Reddit searches and three threads, traffic for 20 domains) costs about 172 credits, plus 1 for each company record. Put every known competitor (up to ten) in `brands` on the one AI run, since it is never cached.

## Deliver

One table: company, domain, class, what it sells (quoted from its title, with the URL), the sources that named it (with the AI cell count), traffic estimate labelled as an estimate, known to the user or not. Direct first, sorted by the number of sources; indirect next; search-only last. Name the two or three worth a [teardown](teardown.md).

## Without the connector

Ask the three AI prompts in a private window on each engine the buyers use, search Google for "<category> alternatives" and "<best-known rival> vs", read the Meta and LinkedIn ad libraries for the category words, and search Reddit for "alternative to <rival>". Log which names each source gives. Say that a hand-run sample is small.
