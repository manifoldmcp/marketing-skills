# Field notes

What Manifold has seen in its own runs, United States, English. Small samples: read them as evidence of what a sample contains and how the tools behave, not as market facts. Newest first.

## Who is in a problem search (15 October 2026)

Three consumer and small-business pains, the way a pain-point run starts: `reddit_search_posts` with the default relevance sort and `time_range: "year"` on "chasing late invoices freelance", "hate doing bookkeeping small business" and "meal planning for family is exhausting"; `reddit_get_comments` on the three busiest invoice threads; `youtube_search_videos` for "freshbooks review" and `tiktok_search_videos` for "meal planning app" (`since: "year"`, `sort: "popular"`); `tiktok_get_comments` on the top TikTok. 10 credits.

**Reddit posts.** Each search returned 7 posts, all on topic.

- Invoices: 4 of 7 were not plain customer voice. Three came from people building a tool and said so ("I started building a tool around this problem", "building out the invoicing side of my app", "I originally built this system just for myself"); the fourth, a post that worked through the cost of a late invoice and said it was "not selling anything", was called out as LLM-written by four commenters, and r/smallbusiness's bot reminded posters that the community is "not your focus group".
- Meal planning: one author posted the same "it's the deciding, not the cooking" question in two communities eleven days apart, a common validation pattern. The other posts read as parents.
- Bookkeeping: one post was cross-posted word for word to two communities; dedupe on author and title.

**Reddit replies.** 44 replies from people other than the poster and bots across three threads: 8 (18%) were vendors or builders. A vendor's brand-named account answered in two of the three threads with its product's reminder feature; others linked their own free tool, offered "the beta" or asked a validation question. The share varied from 1 in 17 (r/indiehackers) to 6 in 13 (r/freelancing).

**Saturation.** The three invoice threads each added two or three themes the earlier ones lacked: the first gave awkwardness, escalation schedules and "make the system ask, not me"; the second statutory late fees and international clients with no recourse; the third "cost of doing business" and pricing slow payers into the terms. Three threads were nowhere near saturation. One reply was the most useful demand signal of the run: a friend's well-built product for exactly this pain had failed for lack of demand.

**TikTok.** Of 30 popular videos for "meal planning app" in the past year, 7 had `is_ad: true`. At least 19 more promoted a named app without the flag: two disclosed in the caption ("#ad", "Anzeige"), four were accounts owned by the app, and the other 13 named or tagged one app, mostly with near-identical scripts across small accounts ("This can't be real", "I just found my new favorite app"). At most 4 of 30 were not visibly promoting an app. Under the top video (29 million views, 2,653 comments), the first page of 18 comments held 9 referral codes or "what's it called", jokes at the top (the most liked had 60,000 likes), and 2 or 3 real desires (scan my receipts and plan from what I already have).

**YouTube.** "freshbooks review" returned 20 videos with likes and comments null and `top_comments[]` empty on every row; `created_at` values were ages rounded to the month. 18 of 20 had under 1,000 views, mostly small review and affiliate channels. Comment counts need `youtube_get_video`.

**Data notes.** `sort: "comment_count"` across all of Reddit dropped the query: 0 of 7 rows on "client won't pay invoice" were about invoices (the busiest threads on Reddit instead). Relevance sort kept every row on topic. A `reddit_get_comments` page returned up to 25 comments and `rows_available` matched the thread's comment count.

**What it means.** Filter founders, vendors, ads and referral codes before counting; dedupe on author; search problem words, not "<category> app"; and expect a first pass of three threads per theme to be far from complete.
