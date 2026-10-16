# Field notes

What Manifold has seen in its own TikTok runs. Small samples: read them as evidence of how the data and the platform behave, not as market benchmarks. Newest first.

## Three niches, one account, one comment thread (15 October 2026)

Three `tiktok_search_videos` calls of one page each (30 rows): "meal prep" (`since: "month"`, `sort: "popular"`), "crm software" (same) and "budget app" (`since: "year"`, `sort: "popular"`); one `tiktok_get_videos` page on @hubspot; two `tiktok_get_transcript` calls; one `tiktok_get_comments` page. 7 credits.

**Consumer and B2B niches live on different scales.**

| Term | Median views, top 30 | Median likes plus comments over views, organic rows | Over 60 seconds | Rows marked `is_ad` | Views above followers |
|---|---|---|---|---|---|
| meal prep (month) | 2.2M | 8.9% | 23 of 30 (median 88 s) | 3 | 22 of 30 |
| budget app (six months) | 140K | 6.5% | 12 of 30 | 8 | 19 of 30 |
| crm software (month) | about 1,900 | not computed | not computed | 1 | not computed |

- In "crm software" half of the top 30 had under 2,000 views, the 30 rows came in five languages (English, Indonesian, Spanish, Italian, Arabic) plus unlabelled rows, and the leader was an explainer account's history of Salesforce (335,645 views). HubSpot's video there (a paid post) had 1,142 views and Salesforce's 1,014. In B2B the reach is with explainers and creators, not brand accounts.
- In "budget app" the 8 paid rows held 87% of all views. Four of them had a like rate of 0.5% of views or less (0.13% on a 17.5M-view Monarch partnership), against 6.5% for the organic median: bought views. One 2,486-follower UGC account had a 29M-view paid video.
- A 6,131-follower account had the meal-prep month's most viewed video (7.1M views, 12 seconds). It was an AI-generated product clip; the top comments asked where the paper came from (3,161 likes) and called it an AI ad (766 likes). The rest of its first comment page was one-word "Patty" replies to a comment-to-DM prompt.

**What the fields really contain.**

- `sort: "popular"` in search returned all 30 rows in strictly falling likes, not views.
- `since: "year"` reached back to 20 April 2026, about six months.
- `transcript` and `top_comments[]` were empty, and `author_verified` null, on all 100 search and listing rows. `is_creator` was null on all comment rows.
- `music` was filled on every row: "original sound - <author>" for original audio (26 of 30 meal-prep rows), "original sound - <another handle>" for a borrowed sound, or a song title and artist.
- A listing page held 10 rows; a search page 30; a comment page 19 of 589.
- HubSpot's latest listing put three pinned videos first (June, April, September) before September's newest; 6 of its 10 latest were `is_ad`. Its photo post had a `/photo/` URL, `media: "image"` and a null `duration_s`.
- A bookkeeper's talking-head video returned a full transcript (`language` null) that matched its caption almost word for word: creators paste transcripts into captions for search. A recipe video set to a song returned `NoData` and still cost 1 credit.
- Hashtags #creatorsearchinsights and #TikTokCreatorSearchInsightsIncentive appeared on budgeting videos from May and September 2026: TikTok's search-gap programme for creators is live.
