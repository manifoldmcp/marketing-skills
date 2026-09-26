# Video ideas

A ranked list of videos worth making, each backed by evidence that people watch it: outlier videos on YouTube, Google demand for the same question, and a gap in what already ranks. It ends in a table of ideas with a working title, the proof and the angle, not in a content calendar.

## Inputs to settle first

- **Topic**: the category and the jobs the buyer does ("cold email", "bookkeeping for freelancers"). Two or three seed phrases.
- **Audience**: who should watch, so a popular topic for the wrong people is dropped.
- **Channel**: the user's handle, if they have one, to skip topics they already covered.
- **Competitors**: optional; their names add "<competitor> review" and "<competitor> vs" queries.
- **Market**: `location` and `language` for the Google calls if not the United States and English.
- **Budget**: a default run costs about 10 + 10 x 2 + 15 + 10 + 9 + 1 = 65 credits for ten queries. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the questions.** `seo_search_keywords` with `seed: "<topic>"` (10 credits for 100 rows). Keep the phrases a video answers: questions, "how to", "tutorial", "for beginners", "review", "vs", "alternative", "template", "example". Take the top 10 by volume, plus any competitor queries. The volume is Google's, a proxy for YouTube demand; say so in the table.
2. **See what YouTube shows.** For each of the 10 queries, `youtube_search_videos` twice (1 credit each): with `since: "year"` for current demand, and with `since: "all"` and `sort: "popular"` for the evergreen winners. Read `views`, `created_at`, `duration_s`, `author` and the title in `text`.
3. **Find the outliers.** For the channels behind the top results, up to 15, `youtube_get_videos` with the default sort (1 credit each) gives the channel's median views of its latest 10. Mark every result with 3 times its channel's median or more: the [outlier rule](../SKILL.md#channel-health). An outlier from a small channel is the strongest proof: the topic did the work, not the audience.
4. **Check Google.** `seo_get_serp` on the 10 queries (1 credit each). A `video` in `features` means Google shows videos for the query, so a good video can earn Google traffic too. YouTube URLs among the organic `results` rank on their own.
5. **Find the angle.** For the three strongest ideas, `youtube_get_transcript` on the top three videos (1 credit each). Note what they cover, what they skip, and how old their facts are (prices, screenshots, versions). For busy comment sections, the [comment mining](comment-mining.md) playbook finds the questions viewers still ask.
6. **Score and cut.** Rank by: outlier evidence first, then Google volume with a video feature, then a freshness gap (the top results are more than two years old), then fit with the audience and the user's product. Drop topics the user's channel already covers well (one `youtube_get_videos` with `sort: "popular"` on it, 1 credit), and topics where the only outliers come from channels with millions of subscribers and nothing smaller ranks.
7. **Deliver** a table: idea (working title), query, Google volume, video feature on Google (yes, no), top video (URL, views, age, length), best outlier (channel, median views, the video's views, ratio), typical length of the top results, format (tutorial, comparison, review, list, teardown), and the angle: what the existing videos miss.

## Judgment

- An outlier beats a big number. A video with 40,000 views on a channel whose median is 2,000 says more about demand than 2 million views on a channel whose median is 1.5 million.
- Compare views per month since upload, not lifetime views. A three-year-old video with 300,000 views earned less a month than a six-month-old one with 100,000.
- Many YouTube topics have no Google volume at all, and some Google queries never get a video. Treat a query with volume, a video feature and outliers as the best case; treat one signal alone as a hint.
- Tutorials, comparisons and reviews match buyers; entertainment topics with huge views rarely do. Keep the audience test strict for B2B.
- Do not copy the winning titles. Give the angle the existing videos miss; the user writes the title.
- Search results are ranked and incomplete. An idea missing from them is not proof nobody made it.
