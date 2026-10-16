# Finding video ideas

Which videos to make, ranked by evidence that people watch them: outlier videos on YouTube, Google demand for the same question, a gap in what already ranks, and whether AI answers cite videos for it. The deliverable is a table of ideas with the proof and the angle, not a content calendar.

## The evidence, strongest first

1. **Outliers.** A video with three times its channel's median views or more ([channel data](channel-data.md#channel-health)). One from a small channel is the strongest proof: the topic did the work, not the audience. A video with 40,000 views on a channel whose median is 2,000 says more about demand than 2 million on a channel whose median is 1.5 million.
2. **Google demand with a video result.** `seo_search_keywords` volume for the phrase, and `video` in the `features` of `seo_get_serp`, meaning Google shows videos for the query, so one video can earn YouTube and Google traffic. YouTube URLs among the organic `results` rank on their own. A keyword under about 50 searches a month is a phrasing, not a topic to plan around.
3. **A freshness gap.** The top results for the query are more than two years old, or their facts are (prices, screenshots, versions). An up-to-date video can take it.
4. **AI answers cite videos for it.** Google's AI Overviews and AI Mode cite YouTube more than any other domain, and they cite small, recent videos ([AI answers](ai-answers.md)). A buyer question where Google's AI answer cites a video is a question a new video can enter fast.

A query with volume, a video feature and outliers is the best case; one signal alone is a hint. Many YouTube topics have no Google volume at all, and some Google queries never get a video.

## The method

About 65 credits for ten queries.

- **Questions.** `seo_search_keywords` on two or three seed phrases (10 credits a 100 rows). Keep the phrases a video answers: questions, "how to", "tutorial", "for beginners", "review", "vs", "alternative", "template", "example". Add "<competitor> review" and "<competitor> vs" when competitors are known. Take the top ten by volume.
- **What YouTube shows.** For each query, `youtube_search_videos` twice: `since: "year"` for current demand, and `since: "all"` with `sort: "popular"` for the evergreen winners.
- **Outliers.** For the channels behind the top results, up to 15, the recent median from `youtube_get_videos`, and every result marked against it.
- **Google.** `seo_get_serp` on the ten queries; with `ai_overview: true` (2 credits each) where AI answers matter, keeping the youtube.com `references`.
- **The angle.** Transcripts of the top three videos for the three strongest ideas: what they cover, what they skip, how old their facts are. Busy comment sections hold the questions viewers still ask ([comments](comments.md)).
- **Cut.** Drop topics the user's channel already covers well (its popular sort, 1 credit), and topics where every outlier comes from a channel with millions of subscribers and nothing smaller ranks.

**Deliver** a table: working title, query, Google volume (marked as Google's), video feature on Google, AI answer cites a video (yes, no, not checked), top video (URL, views, age, length), best outlier (channel, median, views, ratio), typical length of the top results, format, and the angle the existing videos miss.

## Judgment

- Compare views per month since upload, not lifetime views. A three-year-old video with 300,000 views earned less a month than a six-month-old one with 100,000. Since August 2026 newer videos' counts also run higher ([platform](platform.md#what-a-view-is-now)).
- Tutorials, comparisons and reviews match buyers; entertainment topics with huge views rarely do. Keep the audience test strict for B2B: a tutorial with 2,000 views from buyers beats a vlog with 50,000 from anyone.
- Give the angle the existing videos miss; do not copy the winning titles. A title that names the question the video answers serves YouTube search, Google and AI answers alike; all 14 videos cited in our AI answer runs put the category and the buyer's question in the title ([field notes](field-notes.md)).
- A topic that only works as a templated series at volume is the content YouTube's inauthentic-content rules target ([platform](platform.md#mass-produced-content)). An idea needs a point of view the channel can bring.
- The tools see what people search and watch, not what converts. When the user asks which video will bring customers, say their own analytics answer that.
- Never invent a number the evidence does not hold. Every row names its query and volume, or its video URL and views.
- Ideas across every channel, not only YouTube, belong in [growth-strategy](../../growth-strategy/SKILL.md).
