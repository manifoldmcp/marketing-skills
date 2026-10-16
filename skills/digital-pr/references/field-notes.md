# Field notes

What Manifold has seen in its own pulls for PR work, United States, English. Small samples: read them as evidence of how the data behaves, not as benchmarks. Newest first.

## Reporter contacts, news queries and podcast sizing (15 October 2026)

31 credits in all.

**Reporter addresses at three outlets.** `leads_get_domain_emails` on techcrunch.com, martech.org and saastr.com with `department: ["communication"]` and `limit: 10` (18 credits).

- TechCrunch returned 10 editorial rows (54 available): editors, writers, columnists, the editor-in-chief. Only 3 had a source on techcrunch.com itself seen in 2026 (the About page). The other 7 rested on scraped "tech journalist emails" lists (one from 2014), old bylines from 2018 and 2019, or, in one case, only a Google search URL; several named reporters known to have left the outlet years ago. Every row had `confidence` 82 to 85 regardless.
- The domain was `accept_all`, so all ten addresses were unprovable by verification.
- MarTech, a trade publication, returned only two generic addresses, `newsletters@` and `promotions@`, neither editorial. SaaStr returned none in communication.
- Lesson: at a large outlet, filter on `sources[]` (the outlet's own page or a recent byline), not on `confidence`; at a trade outlet, go to the byline and the author page.

**News queries.** `seo_get_serp` with `depth: 20` (2 credits each).

- "email marketing survey after:2025-10-15": the date filter worked (every dated result was from the past year), but all 9 results were vendor, tool and how-to pages. No journalism.
- The same without the date filter: 23 rows, results dated back to 2009, still no journalism.
- "email marketing "study finds" after:2025-10-15": 19 rows including TechRadar, MarTech, Tech.co and MarTechCube articles, plus vendor newsrooms and off-topic studies. Headline phrasing finds coverage; topic phrasing finds vendors.

**Podcast sizing.** `youtube_search_videos` "saas founder podcast" with `since: "year"` (1 credit), then `youtube_get_channel` and `youtube_get_videos` on three small B2B interview shows (6 credits).

| Show | Subscribers | Median views, latest 10 episodes over 14 days old | Median as share of subscribers | Cadence |
|---|---|---|---|---|
| Practical Founders | 3,220 | about 1,980 | 62% | weekly |
| Verticals | 7,960 | about 1,610 (one episode at 6,362, an outlier) | 20% | weekly |
| SaaS Club | 8,690 | about 1,270 | 15% | weekly, about 500 episodes since 2014 |

- The share of subscribers who watch ranged fourfold across three shows of similar size, which is why subscribers cannot size a show. SaaS Club's long-running audio podcast is not visible here at all.
- On the search rows `author_followers` was null for all 20, and `author` was null for 2 (Decoder from The Verge among them), so group by `author_url` or `author_name`.
- One result was a channel re-uploading a famous founder's talk: not a show to pitch.
- The channel listings returned `likes` for most episodes, although the tool description says likes come only with `youtube_get_video`. Do not rely on them being present.
