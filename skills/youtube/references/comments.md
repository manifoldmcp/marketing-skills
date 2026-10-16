# Mining YouTube comments

What viewers say under the videos that matter: questions, objections, feature requests, the products they compare, and the words they use. Reviews of rivals collect the sharpest objections; tutorials collect confusion and feature requests. The deliverable is a table of themes with counts and verbatim quotes, from a few hundred comments rather than a guess.

## Picking the videos

YouTube has no search across comments, so the table covers only the videos picked. Pick them deliberately.

- **For a topic:** `youtube_search_videos` with `since: "year"` on queries such as "<competitor> review", "<category> tutorial", "<competitor> vs <competitor>", "is <competitor> worth it". **For a channel:** its popular sort.
- Then `youtube_get_video` on the 15 most viewed candidates, since only it carries a reliable `comments` count, and keep the 10 with the most comments, from different channels where the source is a topic.
- Skip giveaways and contests: their comments are entries, not opinions.
- For what people said recently, pick recent videos. Comments come most liked first, not newest, and their `created_at` is an approximate age.

About 41 credits for 10 videos and two pages of comments each.

## Coding

- Drop spam, timestamps, jokes, emoji-only comments and "great video". Keep the channel's own replies (`is_creator`) apart: they answer, they do not ask.
- Code each remaining comment once: question, objection or complaint, feature request, comparison (names another product), praise, use case ("I use it to...").
- Group codes into themes, count each, and weight by `likes`: a comment with 300 likes speaks for many viewers, and one with 500 outweighs fifty with none.
- Read replies (`depth` above 0) before calling a question unanswered; they often hold the answer, and sometimes a competitor's support team.
- When comments argue with something the video says, the transcript shows exactly what was said.

## Delivering

A table: theme, code, comments, total likes, videos it appears on, two or three verbatim quotes with the video URL, and what to do with it (a video to make, an FAQ or sales-page question, a phrase for copy, an objection for sales, a feature request for the product team). Below it, the phrases people repeat, and how many videos and comments the table rests on. Offer to add the objections and customer language to `.agents/product-marketing.md`.

## Judgment

- Quote word for word, with the link. The value is the commenter's language; a paraphrase loses it.
- A creator's fans defend the creator, and a review's comments argue with the reviewer. Separate complaints about the product from reactions to the video.
- A theme on one video belongs to that video. One that recurs across several channels' videos is stronger than one busy thread; report those first.
- A question asking for a recommendation is the strongest buying intent a comment carries. Views and likes on the video measure attention, not intent.
- Commenters are private people. Quote them as evidence for the user; in public copy, paraphrase and never name them. Never turn commenters into a lead list, and never reply or contact anyone.
- Pains ranked across Reddit, reviews and every platform, and personas built from them, belong to [voice-of-customer](../../voice-of-customer/SKILL.md).
