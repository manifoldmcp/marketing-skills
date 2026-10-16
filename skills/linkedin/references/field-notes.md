# Field notes

What Manifold has seen in its own reads of LinkedIn through the `linkedin_*` tools. Small samples: read them as evidence of how the data behaves, not as benchmarks. Newest first.

## Three niches, one page each, plus spot checks (15 October 2026)

Inputs: `linkedin_search_posts` with `since: "month"` on "revenue operations", "cold email" and "product-led growth", one page each (30 posts); then `linkedin_get_post` on one row, `linkedin_get_company` and one page of `linkedin_get_company_posts` on a company page from the results, `linkedin_get_comments` on one post, and `linkedin_get_profile` on two authors (one failed). About 9 credits.

**What the rows carry.**

- Every search row had `likes`, `comments` and `author_followers` (company pages too); none needed `linkedin_get_post` for counts. `shares`, `saves` and `views` were null on all 30.
- `top_comments` held up to 10 commenters; `linkedin_get_comments` on the same post returned exactly the same 10, with no time, likes or replies. It adds nothing once a row is in hand.
- The company feed (9 posts on its first page) carried no `likes`, `comments`, `author_followers`, `author_name` or `top_comments`, as its description says. `linkedin_get_company` did return `followers`, which matched the `author_followers` on that page's search row.
- The profile's `bio` came back cut to about 80 characters; `recent_posts` held six posts with their first 280 characters and no engagement; `following` was null. The second profile call failed in transport on a character in the profile and was not retried.
- `created_at` came back as millisecond timestamps for ages LinkedIn shows only as "3d" or "2w".

**`media` read video for any attachment.** 14 of 30 rows said video, 16 text, none image. Only one row carried a `transcript`, and it was a real talking-head video. Comment text on other "video" rows referred to a graph or a GIF, and a fetch of two such posts found no video. A later search showed why: the vendor's media URL is a video stream for a video and an image URL for a photo, a document carousel or a GIF still, and the tool read any URL as video. It now reads video only for a stream and image otherwise; the company feed carries no media at all, so there `media` is null.

**The niche searches looked like this.**

- Median likes 19.5, median comments 4 across the 30. The ranking was not by engagement: a one-like post sat fourth on its page.
- 4 of 30 authors were company pages (all in the revops search, median 5 likes); counting consultants and agencies selling into the topic, about half the revops results were vendors.
- 6 of 30 posts carried a link in the text, all lnkd.in short links; two more pointed to "the first comment".
- Two giveaway patterns: one post with 165 likes and 459 comments whose 10 visible comments were all the same one word, and a "comment MOTION" post from a 135-follower account with 132 likes and 0 comments (a likes count near the follower count on a tiny account is a pod or bought-engagement sign).
- Branded pages named "Sales Tips", "Sales AI", "Marketer Tips", "Daily Business Tips" and "Email Marketing Tips" left one-line generic comments on several posts: the automated commenting LinkedIn says it now demotes, visible in `top_comments`.
