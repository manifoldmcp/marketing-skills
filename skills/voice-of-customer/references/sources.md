# Sources

Where buyers talk, which question each place answers, whose voice it carries, and what reading it with Manifold costs. Pick sources before collecting: the source decides the voice, and the voice decides what the findings can claim.

## Public posts against interviews

The best test so far is Timoshenko and Hauser (Marketing Science, 2019). A professional voice-of-the-customer firm had found 86 needs for oral care from experiential interviews. Its analysts then coded 12,000 sentences sampled from 115,099 Amazon reviews.

- 8,000 sentences, slightly cheaper to review than a typical set of interview transcripts, surfaced 74 of the 86 interview needs and 8 the interviews missed. The next 4,000 found 9 of the 12 still missing: 97% of all needs.
- Only 52% of sentences held a need, and the corpus was repetitive: the top tenth of needs filled 54% of informative sentences while 17 needs appeared five times or fewer. Coverage of the long tail comes from volume and from deliberately sampling diverse text, not from reading the loudest posts.
- Reviews marked "helpful" were not more informative.
- Interviews take 20 to 30 people and analyst time worth roughly 40% to 55% of what scheduling and interviewing cost; reviews skip the recruiting. The authors still advise mixing sources where self-selection is a fear: one category is one test.

So: public text is a legitimate first source for needs and language, not a lesser substitute. It is weaker for anything that needs the person (budget, who signs, what they did last Tuesday) and for buyers who never post.

## Who is talking

Every source over-represents someone. Name it next to the finding.

- **Reviews** are J-shaped: mostly five stars, a bump at one, few in between, because people with extreme experiences review and buyers who expected to like a product bought it (Hu, Pavlou and Zhang, 2009 and 2017). Polarity varies by platform and comes from who chooses to review: people who review rarely write the most extreme reviews (Schoenmueller, Netzer and Stahl, Journal of Marketing Research, 2020). Read reviews for needs and words, not for the average rating.
- **Reddit** in the US skews young, educated and well-off: 48% of adults aged 18 to 29 use it against 6% over 65, 29% of men against 23% of women, 37% of graduates against 15% with high school or less, 37% at $100,000 or more against 17% under $30,000 (Pew, 5,022 adults, February to June 2025). Its users lean technical, hands-on and price-sensitive; enterprise buyers' procurement, security and legal worries are under-represented. Some communities ban research outright: r/smallbusiness's bot tells posters "we are not your focus group".
- **Comments under video** are partly about the creator and the video. The most-liked comment is often a joke (60,000 likes for one under the top meal-planning video in our run). Comments under reviews and comparisons come from people deciding; under a brand's own posts, from fans and support seekers; under ads, objections and trolls.
- **LinkedIn** is the polished, public, career-safe voice: goals, metrics, wins. Take goals from it, objections from Reddit.
- **Facebook groups** hold local, older and non-technical buyers that Reddit misses, but only public groups can be read and admins' rules decide what members may say.
- **Search queries** (`seo_search_keywords`) are the silent majority's doubts, without words around them.

## Sellers in the sample

In 2026 a problem search returns founders validating ideas, vendor accounts answering, AI-written posts and sponsored creators. In our October 2026 run ([field notes](field-notes.md)): 3 of 7 top Reddit posts on late invoices came from people building a tool and a fourth read as AI-written, 8 of 44 replies were vendors or builders, and 26 of 30 popular TikTok videos on meal planning apps promoted a named app. Before counting, drop or tag:

- Posts that end in "would something like this solve it" or describe "a tool I started building", and the same author posting the same question in several communities.
- Accounts named after a product, replies that link the replier's own tool, and threads whose top reply is a product link from an account that only posts about it.
- Videos with `is_ad: true`, and the many the flag misses: "#ad", "Anzeige", "partner" hashtags, one app's hashtag across many small accounts with the same script, and accounts named after the app.
- Referral codes and "what's it called" comments.

Keep them in a separate row when the user wants to know what competitors and new entrants are building: that is market intelligence, not customer voice.

## Which source for which question

| Question | Best sources | Manifold tools |
|---|---|---|
| Problems and workarounds | Reddit threads and replies, Facebook groups, reviews of tools in the category | `reddit_search_posts`, `reddit_search_comments`, `reddit_get_comments`, `facebook_get_group_posts` |
| Doubts before buying | Google queries, "worth it" threads, comments under reviews | `seo_search_keywords`, `reddit_search_posts`, `youtube_get_comments` |
| Consumer desire and language | TikTok and Instagram comments, YouTube tutorials | `tiktok_search_videos`, `tiktok_get_comments`, `instagram_search_posts`, `instagram_get_comments` |
| Roles, goals, metrics | LinkedIn posts, company databases | `linkedin_search_posts`, `linkedin_get_profile`, `leads_search_people` |
| Why people pick or leave a rival | Reviews, "switched from" threads | see [competitor-intel](../../competitor-intel/SKILL.md) |

Defaults: business software, Reddit and YouTube; consumer products, Reddit, TikTok and Instagram; Facebook only when the user has group URLs or wants public groups found. If unsure whether a platform has the conversation, one search (1 credit) settles it.

## Costs and samples

Defaults the old job skills measured and that still hold against the tool descriptions:

| Source | Default sample | Credits |
|---|---|---|
| Reddit | 12 post searches, 3 comment searches, 15 threads read, 3 posts read in full | about 33 |
| Facebook groups | 3 groups x 5 pages of posts, 15 posts' comments, 3 searches to find groups | about 33 |
| TikTok | 3 searches x 2 pages, 15 videos x 2 pages of comments | about 36 |
| Instagram | 3 hashtags x 2 pages, 15 posts x 2 pages, no replies | about 36 |
| YouTube | 4 searches, 15 `youtube_get_video` for the comment count, 10 videos x 2 pages | about 41 |
| Facebook page comments | 5 pages of posts, 15 posts x 2 pages | about 35 |

Traps per source:

- **Reddit:** one search page returns about 7 posts; `reddit_get_comments` returns up to 25 comments a page with `rows_available` giving the thread's total. Comment search leans to the last few weeks whatever the window, and `time_range` only trims that page: for older comments use `sort: "top"` with `time_range: "all"`. The full rules are in [reddit](../../reddit/SKILL.md).
- **YouTube:** search rows have null likes and comments and empty `top_comments[]`, and `created_at` is an age rounded to the month. Comments come most liked first, so page one is what the audience agrees with most; replies (`depth` above 0) often answer a question, sometimes from a competitor's support team.
- **TikTok:** `since: "year"` reaches back about six months; replies do not come back, so a comment with many `replies` is a debate worth noting. `tiktok_get_video` can cost 10: the search row already has the counts.
- **Instagram:** search is by hashtag only and sees only tagged posts.
- **Facebook:** groups come newest first with no window; note how many days five pages covered, which is the group's pace. Rows do not mark boosted posts: ask the user.
- **LinkedIn:** `linkedin_get_comments` returns at most 10 comments however busy the post.
- **Reviews:** app store reviews (`apps_get_reviews`, 1 credit per 100), Google Shopping reviews pooled across stores (`shopping_get_reviews`, 5 per 100) and the review topics on a Google Business Profile (`maps_get_place`, 2) are readable. G2, Capterra and Trustpilot are not: `seo_get_serp` (1 credit) for "<competitor> reviews" finds those pages; read them by hand or with the host's own fetch, and ask the user for their own reviews and support tickets, which are the best source of all.
