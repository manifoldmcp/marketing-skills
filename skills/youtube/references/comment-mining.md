# Comment mining

What viewers say under the videos that matter: their questions, objections, complaints, the products they compare, and the words they use. It ends in a table of themes with verbatim quotes and what to do with each, from a few hundred comments rather than a guess.

## Inputs to settle first

- **Source**: which videos. The user's own, a competitor's channel, or a topic (reviews and tutorials of the category or of a competitor).
- **Question**: what the user wants to learn: objections before buying, feature requests, what confuses people, or the language for copy. Default: all of them, coded in step 3.
- **Size**: default 10 videos and two pages of comments each.
- **Budget**: a default run costs about 4 + 15 + 10 x 2 + 2 = 41 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the videos.** For a topic, `youtube_search_videos` with `since: "year"` (1 credit each) for four queries such as "<competitor> review", "<category> tutorial", "<competitor> vs <competitor>" and "is <competitor> worth it". For a channel, `youtube_get_videos` with `sort: "popular"` (1 credit). Then `youtube_get_video` on the 15 most viewed candidates (1 credit each), since only it carries the `comments` count, and keep the 10 with the most comments. Reviews and comparisons hold objections; tutorials hold confusion and feature requests.
2. **Pull the comments.** `youtube_get_comments` on each video, two pages (1 credit a page). They come most liked first, so the first page is what the audience agrees with most. Keep `text`, `likes`, `replies` and `depth`.
3. **Code them.** Put each comment in one code: question, objection or complaint, feature request, comparison (names another product), praise, or use case ("I use it to..."). Drop spam, timestamps, jokes and "great video". Group codes into themes and count each theme, weighting by `likes`: a comment with 300 likes speaks for many viewers.
4. **Check a disputed claim.** When comments argue with something the video says, `youtube_get_transcript` on that video (1 credit) shows exactly what was said.
5. **Deliver** a table: theme, code, comments, total likes, two or three verbatim quotes with the video URL, and what to do with it: a video to make, a question for the FAQ or the sales page, a phrase for copy, an objection for sales to answer, a feature request for the product team.

## Judgment

- Quote comments word for word. The value is the viewer's language, and a paraphrase loses it.
- A creator's fans defend the creator, and a review's comments argue with the reviewer. Separate complaints about the product from reactions to the video.
- Replies (`depth` above 0) often hold the answer to a question, and sometimes a competitor's support team. Read them before calling a question unanswered.
- `created_at` is approximate and the order is by likes, not time. For what people said last month, pick recent videos rather than sorting comments.
- Viewers are not all buyers. A theme that recurs across several channels' videos is stronger than one busy thread.
- For pain points across Reddit, reviews and every platform, use the [customers](../../customers/references/pain-points.md) group, which calls this playbook for YouTube.
