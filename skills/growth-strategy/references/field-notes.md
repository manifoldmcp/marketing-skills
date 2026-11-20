# Field notes

What Manifold has seen in its own runs, United States, English. Small samples: read them as evidence of how the data behaves, not as market facts. Newest first.

## A cheap baseline on one category (15 October 2026)

The growth-plan baseline on "invoice app for freelancers", competitor FreshBooks: `seo_search_keywords` (mode related), `reddit_search_subreddits`, `reddit_get_new_posts` on three subreddits, `linkedin_search_posts`, `tiktok_search_videos`, `ads_get_advertiser_ads` on Google. 14 credits.

- **Brand names swamp category demand.** The keyword set returned 28 rows totalling about 37,500 searches a month; about 30,300 (81%) were competitors' brand terms (Zoho Invoice, Wave, Invoice Simple). The seed itself had 320 a month, with a `trend[12]` running from 20 to 1,900 within the year; "invoicing software free" moved between 1,300 and 12,100. Strip brands before applying the 1,000-a-month floor, and read the median month.
- **Charged on rows returned.** 28 rows cost 7 credits, not the 10 estimated.
- **One page of new posts is not a week.** `reddit_get_new_posts` with `since: "7d"` and one page covered about three days of r/freelancing, two of r/Freelancers and nine hours of r/smallbusiness; `complete` was false on all three, and one matching post came back. A weekly count needs `pages: 3`, and on a busy subreddit still a shorter `since` or several calls.
- **`subscribers` came back null** on every row of `reddit_search_subreddits`, and the largest `posts_in_sample` was 2, so the floor of 3 posts in the sample excluded every community, including r/freelancing, the obvious one. Treat the floor as a hint and confirm with new posts.
- **TikTok search for an app category is mostly vendors talking to nobody.** 23 of 30 videos were makers promoting their own app or template: median 138 views, one above 10,000. The 7 videos by creators had a median of 724, led by a how-to on making an invoice (23,892 views). 26 of 30 were under 1,000 views.
- **LinkedIn:** 10 posts on one page; vendor company pages drew 1 or 2 likes; the most-discussed post (37 likes, 29 comments) was a founder's opinion on freelancers' cost, not a product post. `views` was null throughout.
- **Google ads:** `active` was null on every row, so read `last_shown`. Querying freshbooks.com returned 40 of 800 creatives under three advertiser names: 2ndSite Inc. (FreshBooks' company) and two others. Group rows by `advertiser` before counting a competitor's ads. Several creatives first shown in 2022 and 2023 were still showing in October 2026: long-running ads are the paid signal.

## What a Product Hunt page earns in search (15 October 2026)

`seo_get_domain_overview` on producthunt.com with `history: true` (61 credits) and `seo_get_ranked_keywords` (10 credits).

- Estimated US organic traffic rose from about 298,000 (November 2025) to 445,000 a month (October 2026) while ranking keywords fell from 64,500 to 45,700 (down 29%). Traffic concentrates on fewer pages.
- The top pages by keywords were category and "alternatives" pages and famous products (Google Sheets' page alone about 26,000 visits a month). The sample of 100 ranked keywords was mostly a launched product's own name ("cityvibes", "flight penguin", "badavas") and "alternatives to X".
- So a launch page's lasting search value is catching people who already search for the brand, plus a place on category and alternatives pages. It is not a source of category demand.
- **Data note:** `seo_get_ranked_keywords` returned this page out of traffic order (a keyword with 578 visits sat in the middle), since the vendor's default order is by rank. The tool now asks for traffic order, highest first.
