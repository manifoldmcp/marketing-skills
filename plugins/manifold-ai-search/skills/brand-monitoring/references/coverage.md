# What can be seen

What each platform lets anyone (Manifold, a listening vendor, a person) read in October 2026, and what stays dark. Platforms closed most of their research and API doors between 2023 and 2026; every listening tool now runs on a mix of paid APIs, partner feeds and scraping of public pages, and none is complete. Re-check anything here older than six months.

## By platform

| Platform | What its own access allows (2026) | What Manifold reads | Dark to everyone outside the platform |
|---|---|---|---|
| Reddit | Self-service API access closed 11 November 2025 (Responsible Builder Policy); new access needs approval. Reddit sued SerpApi, Oxylabs, AWMProxy and Perplexity for scraping on 22 October 2025 | Every post in watched subreddits (7 days back), ranked search of posts and comments, full threads | Deleted and removed content, private subreddits, vote detail |
| X | Pay-per-use since February 2026: $0.005 per post read, capped at 3M reads a cycle before Enterprise (X developer docs) | One account's recent posts (`twitter_get_tweets`), one post, one profile. No keyword search, no replies | Everything for anyone not paying per read; replies and quote chains are costly even then |
| TikTok | Research API for academic and not-for-profit researchers only; commercial users excluded | Ranked keyword search with views, likes, shares, saves, creator size; comments one video at a time | A complete window; Stories; most transcripts (null in our sample) |
| YouTube | Public Data API with quotas | Ranked keyword search, channel, video, comments, transcripts | Exact upload times through search (ages such as "3 weeks ago"); no newest-first search |
| LinkedIn | No public post search API; partner programmes only | Ranked post search, company pages and posts, up to 10 comments a post | Reactors, comments beyond the 10 shown, anything not public to a logged-out reader |
| Instagram | Graph API covers the user's own accounts and hashtags with limits | Posts under one hashtag, profiles, posts, reels, comments | Free-text search, Stories, untagged mentions |
| Facebook | CrowdTangle closed August 2024; the Meta Content Library replaced it for approved researchers | Posts in groups the user names, page posts, comments | Search across Facebook, private groups, most profile posts |
| Threads | Keyword search API needs Meta's approval and is capped (2,200 queries a rolling 24 hours per user, Meta docs, January 2026) | Not covered | For tools without approval, most of it |
| Bluesky | Open firehose of public posts | Not covered | Little; it is the most open network |
| Web and news | Search engines and news APIs | `seo_get_serp` for what ranks on the brand name; `seo_get_backlinks` for new linking pages | Paywalled and unindexed pages |
| AI answers | No engine publishes mention data (Bing's AI Performance report counts citations to the user's own site) | `aeo_run_ai_answers` across six engines ([ai-search](../../ai-search/SKILL.md)) | What a person sees in a signed-in, personalised session |

## What this means for a report

- **Name the gaps in the first report, once.** X, private groups, Stories, Threads and Bluesky are not watched; comments under TikTok, YouTube, Instagram and LinkedIn posts are read one post at a time, not searched. "No mentions found" means none found in what was searched.
- **A vendor claiming full coverage of social is wrong for at least X, Facebook and comments.** Enterprise listening tools document per-network limits (some Facebook, Instagram, LinkedIn and TikTok sources need account authentication or a source list; ads and dark posts are not reliably indexed). Ask a vendor for its source documentation before trusting a share-of-conversation number.
- **Counts across platforms do not add up.** Each search is a ranked page; one platform's 30 rows and another's 10 say nothing about relative volume. Compare a platform with itself over time, with the same query, and compare brands only on the same queries run the same day.
- **Watch where the buyers are, not everywhere.** For B2B software, LinkedIn and a handful of subreddits usually carry the talk; for consumer brands, TikTok and YouTube. A platform with no mentions after a month of runs can drop to weekly.

## Filling the gaps by hand

- **X:** the user's own X notifications and X's search, sorted by latest, for the brand and its handle. For a competitor's announcements, `twitter_get_tweets` on their handle.
- **Bluesky:** its public search, or a firehose consumer if the brand is talked about there.
- **Threads:** the app's own search.
- **Hacker News:** hn.algolia.com, sorted by date, for posts and comments.
- **Reviews:** G2, Capterra and Trustpilot notify the claimed listing's owner and no Manifold tool reads them. App store reviews (`apps_get_reviews`, newest first), Google Shopping reviews (`shopping_get_reviews`) and a Google Business Profile's rating and review topics (`maps_get_place`) can be read and re-read on a schedule.
