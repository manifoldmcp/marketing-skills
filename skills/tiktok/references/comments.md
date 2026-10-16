# Mining TikTok comments

The questions, objections, desires and words people use under the TikTok videos that matter. Comments are where buyers say what they doubt, in their own language. Ranking pains across Reddit, reviews and every platform belongs to [voice-of-customer](../../voice-of-customer/SKILL.md); this page is the TikTok part.

## Picking the videos

- TikTok has no search across comments, so the sample is only as good as the videos picked. Pick more videos rather than more pages when a theme is thin.
- **From terms:** `tiktok_search_videos` for the category, the problem and rival brand names (creator reviews of rivals collect the sharpest objections), `since: "year"` (about six months back) or `since: "month"` for a news-driven topic, `sort: "popular"`. **From accounts:** `tiktok_get_videos` with `sort: "popular"`. Keep about 15 videos with the most `comments`, from different authors, mixing creator reviews and brand videos.
- **Skip** giveaways and contests (their comments are entries) and comment-to-DM videos ("comment CRM and I'll send it"): their comments are one keyword repeated. Label `is_ad` videos: comments on an ad are objections to the ad.
- `tiktok_get_comments`, two pages each (about 20 top-level comments a page, sorted roughly by likes first, then newest). Keep `text`, `likes` and `replies`. Replies themselves do not come back; a comment with many `replies` is a debate worth noting. About 36 credits for 15 videos on three terms.

## Coding

- **Clean:** drop spam, emoji-only comments and friend tags (count those: they show sharing). Keep the creator's own comments apart; `is_creator` came back null, so match `author` to the video's author.
- **One type per comment:** question, objection, desire (the outcome wanted), comparison (names a rival or alternative), experience (used it). Group into themes and weight each by likes: a comment with 500 likes outweighs fifty with none.
- **Language:** list the exact phrases people repeat for the problem and the outcome across videos. These are the words for hooks, captions, ads and landing pages, and the phrases people search TikTok with.
- **A question asking for a recommendation** is the strongest buying intent a comment carries. Views and likes on the video measure attention, not intent.
- **Comments under a creator's video are partly about the creator.** Keep the ones about the product or problem. Report themes that recur on three or more videos or across several creators first; one video's theme belongs to that video.
- **Trust signals count.** Comments calling a video fake, AI-made or an ad are themes too: they tell the brand what its audience will not believe.

## The deliverable

A table: theme, type, comments, total likes, videos it appears on, two or three quotes word for word with the video link, and what to do with it (a video that answers it, an FAQ line, an ad angle, a landing page line, an objection for sales, a product request). Below it, the recurring phrases, and how many videos and comments the table rests on.

## Rules

- Quote word for word with the link; a paraphrase loses the language. In public copy, paraphrase and never name the commenter.
- Commenters are private people. Never turn them into a lead list, reply, or contact anyone.
