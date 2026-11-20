# Field notes

What Manifold has seen in its own calls. Small samples: read them as evidence of how the tools and surfaces behave, not as benchmarks. Newest first.

## Local searches: Facebook in Google and AI answers (15 October 2026)

**Google results.** `seo_get_serp` with `ai_overview: true` on six local searches: "best family dentist in columbus ohio", "reliable plumber in sacramento reviews", "best yoga studio in austin tx", "best bakery in portland maine" (United States), "best garden centre near leeds", "best dog groomer in manchester" (United Kingdom). 12 credits.

| Search | Facebook rows | Yelp | Reddit |
|---|---|---|---|
| Dentist, Columbus | none | organic 8 | organic 6 |
| Plumber, Sacramento | 2 group threads in discussions; group thread organic 6; fb-answers page organic 5 | organic 1, 4, 8 | discussions, organic 2 |
| Yoga, Austin | 1 group thread in discussions | organic 6 | discussions, organic 2 |
| Bakery, Portland | 1 group thread in discussions; group threads organic 3 and 7 | none | discussions |
| Garden centre, Leeds | 1 group thread in discussions; business page organic 8 (snippet "92% recommend (20 reviews)") | none | discussions |
| Dog groomer, Manchester | none | none | none |

Only the yoga search showed an AI Overview, naming three studios from Google's local pack with no cited sources.

**AI answers.** `aeo_run_ai_answers` on five local prompts (the dentist, plumber, bakery and yoga prompts above, and "good independent bookshop in leeds") on chatgpt, gemini, ai_overview and ai_mode, `brands` facebook.com and yelp.com, location United States for all five (so the Leeds prompt was asked from the US). 40 credits. Models: gpt-6 (ChatGPT app), Gemini 3.5 Flash-Lite (app).

- AI Overview returned no answer on all five.
- No answer cited facebook.com or yelp.com, and neither appeared in ChatGPT's 64 retrieved pages. Neither was named in any answer's text.
- Gemini's citations were all maps.google.com (Google Business Profiles), 20 of 20.
- ChatGPT cited local guides and press (Boston Globe, Afar, Press Herald, Leeds List), directories (toprateddentist.com, the California licensing board), business sites, one Tripadvisor page and one Reddit thread. Its retrieval leaned on directories (Healthgrades, Zocdoc, Angi, HomeAdvisor, ClassPass).
- AI Mode cited business sites, local press, Angi and Reddit.

## What the tools return (15 October 2026)

Calls on Leeds Garden Centre's page (1,400 followers), Gymshark's page, and the public groups Portland Maine (portlandblue) and an ADU builders group (howtoadu). 13 credits.

- **Rows per call.** `facebook_get_posts` returned 3 posts a call on both pages, three calls running. `facebook_get_group_posts` returned 3 to 5. `facebook_get_comments` returned 10.
- **Reel views.** Gymshark's reels had `views` null in the listing; `facebook_get_post` on one returned 7,310 views, 6 shares and `duration_s` 53. The `facebook_get_group_posts` fixture recorded on 12 October did carry a view count on a group video, so the gap is reels in page listings at least.
- **Group threads from Google read fine.** `facebook_get_post` and `facebook_get_comments` on a year-old Portland group thread (a request for a local bakery, 29 comments) returned the post and 10 comments naming at least eight local businesses. Comment `author_url` was null, names present, `depth` 0.
- **Group pace.** Portland's group posted 8 times in about 7 hours. Some group posts had null `text` (image posts); one author was "Anonymous participant".
- **Engagement bait.** Leeds Garden Centre's six most recent posts had median `likes` plus `comments` of 2 (about 1.4 per 1,000 followers, close to the 0.15% cross-page average). Its like-share-tag giveaway had 51 plus 23 shares, about 25 times the median.
- **Transcript.** `facebook_get_transcript` on a music-heavy Gymshark reel returned garbled lyric fragments with `language` null.
- **Search operator.** `seo_get_serp` on "site:facebook.com/groups airbnb host cleaning fee" returned unfiltered results (Reddit, blogs, one Facebook video page).
