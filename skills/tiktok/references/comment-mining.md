# TikTok comment mining

The questions, objections and words people use in the comments under the niche's top TikTok videos. Comments are where buyers say what they doubt and what they want, in their own language. It ends in a table of themes with counts, quotes and what to do with each.

## Inputs to settle first

- **Niche**: two or three search terms for the category and the problem, plus competitor brand names: reviews of rivals collect the sharpest objections. Or competitor handles, to read the comments on their own videos.
- **Window**: `since: "year"` by default, which on TikTok reaches back about six months; `since: "month"` for a new product or a news-driven topic.
- **Sample**: 15 videos with two pages of comments each by default.
- **Budget**: a default run costs about 3 x 2 + 15 x 2 = 36 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the videos.** `tiktok_search_videos` for each term with `since: "year"` and `sort: "popular"`, two pages each (1 credit a page). For competitor handles, `tiktok_get_videos` with `sort: "popular"`, one page each (1 credit). Keep the 15 videos with the most `comments`, from different authors, mixing creator reviews and brand videos. Label any row with `is_ad: true`: comments on an ad are objections to the ad.
2. **Pull the comments.** `tiktok_get_comments` on each video, two pages (1 credit a page). Keep `text`, `likes` and `replies`. Replies themselves do not come back; a comment with many `replies` is a debate worth noting.
3. **Clean.** Drop spam, emoji-only comments and comments that only tag a friend (count those: they show people sharing the video). Keep the creator's own comments apart (the `author` is the video's author): they answer, they do not ask.
4. **Code each comment.** One type per comment: question, objection, desire (the outcome they want), comparison (names a competitor or an alternative), or experience (they used it, good or bad). Group them into themes, and weight each theme by the likes on its comments: likes show how many people agree.
5. **Pull the language.** List the exact phrases people repeat for the problem and the outcome across videos. These are the words for hooks, ads and landing pages.
6. **Deliver** a table: theme, type, comments, total likes, videos it appears on, two or three quotes word for word with the video link, and what to do with it (a video to make, an FAQ entry, an ad angle, a landing page line). Below it, the list of recurring phrases, and how many videos and comments the table rests on.

## Judgment

- Comments under a creator's video are partly about the creator. Keep the ones about the product or the problem.
- A theme found on one video belongs to that video. Report themes that recur across three or more videos first.
- TikTok has no search across comments, so the table covers only the videos picked. Say so, and pick more videos rather than more pages when a theme is thin.
- A comment with 500 likes outweighs fifty comments with none.
- Quote commenters as evidence for the user. In public copy, paraphrase and never name the commenter.
- For pain points across Reddit, reviews and several platforms, the [customers](../../customers/references/pain-points.md) group's pain points playbook calls this one and merges the results.
