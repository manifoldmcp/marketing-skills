# Field notes

What Manifold has seen in its own runs, United States, English. Small samples: read them as evidence of how the tools and platforms behave, not as benchmarks. Newest first.

## Mention search precision, watch coverage and branded volume (15 October 2026)

48 credits in all. Brands chosen for range: established (HubSpot, Notion, Pipedrive), fast-growing (Attio, Profound, Peec AI), common-word (Linear, Folk, Manifold) and too small to measure (Manifold MCP).

### Platform search for one brand name

"attio" (a distinctive name with an active launch on 14 October), one page each, 6 credits:

| Call | Rows | About the brand | Notes |
|---|---|---|---|
| `tiktok_search_videos`, week, latest | 7 | 0 | Matched "aito", "atico", "atio", "aoto" |
| `youtube_search_videos`, week | 15 | 2 | Both were the brand's own uploads; 13 were about "80" |
| `linkedin_search_posts`, week | 10 | 10 | 3 from the brand's own page and co-founder; 1 partner-tagged post; 1 churn story |
| `reddit_search_posts`, month | 7 | 7 | One author's post in two subreddits |
| `reddit_search_comments`, month | 9 | 7 | All 9 dated 2023 to 2024: the time range was ignored. 2 used a Bengali word spelled "attio" |

Qualifying the term, 2 credits: "attio crm" over a month returned 20 of 20 on-brand videos on YouTube, but 6 of 30 on TikTok (the rest only said "crm"; three were near-identical "comment CRM" posts from small accounts). `transcript` was null on every TikTok row.

**So:** filter every row on the term in its text, check the brand's own accounts, and on TikTok expect most rows to be noise even with a qualifier. LinkedIn and Reddit post search were precise. Reddit comment search needs its own date filter.

### Watching versus searching Reddit

`reddit_get_new_posts` on r/CRM, r/sales, r/SaaS, r/startups and r/hubspot, `since` 7 days, `match` "hubspot" and "attio", one page each (5 credits): 23 posts, all with `complete` false. One page covered:

| Subreddit | Window one page reached |
|---|---|
| r/SaaS | about 8 hours |
| r/sales | about 2 days |
| r/startups | about 2.7 days |
| r/hubspot | about 6 days |
| r/CRM | about 6 days |

`reddit_search_posts` for "hubspot" over the same week (1 credit) returned 7 posts. One was among the 23; the other six were in communities not watched, including the week's highest-scoring thread (HubSpot layoffs, r/techsales, score 42, 25 comments). Three of the seven read like seeded posts: lowercase titles built around the brand name ("has anyone done a hubspot crm review for a small business?"), generated two-word usernames. The Attio search had one of the same shape. We cannot prove seeding; we leave such posts out of sentiment counts.

**So:** at one page a day, busy subreddits are cut within hours; use `pages: 3` and a daily run for them. Search is a thin net, but it is the only one for communities nobody named.

### Branded volume

`seo_get_keyword_metrics` on 12 names with `ai_volume: true` (11 credits), then the same names with `source: "google_ads"` (24 credits).

- Both sources returned the same `volume` and the same bucketed monthly values (for example 1,600 / 1,900 for "folk crm" across the year). The default source's `trend` ended in August 2026 (data dated 16 September); the Keyword Planner source's ended in September. Keyword Planner returned null for "peec ai", which the default source had at 2,400.
- Common words: "linear" 90,500 against "linear app" 6,600; "folk" 60,500 against "folk crm" 1,900; "manifold" 40,500. The bare common word is mostly not the brand.
- A small brand's name was null in both sources. Its own Search Console showed 21 impressions and 12 clicks over 90 days for that query, free with `console_get_search_analytics`. One branded impression came from an AI agent's query carrying a dozen `-site:` exclusions.
- `ai_volume` fell for every established brand from October 2025 to August 2026 (HubSpot 15,821 to 9,776, Notion 25,388 to 15,608, Pipedrive 1,082 to 598, the common words too) and recovered partly in September, while the fast-growing brands rose (Attio 71 to 217, Profound 36 to 196, Peec AI 3 to 31). The drop across unrelated names points to the index, not the brands: read `ai_volume` as a share within the set.
- The charge for 12 names with `ai_volume` was 11 credits; the per-100 prices in the tool description are prorated.
