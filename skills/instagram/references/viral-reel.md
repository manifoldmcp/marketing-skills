# Why one reel went viral

Why one reel did far better than its account usually does: set against the account's own baseline, its transcript, its comments and its timing, ending in a table of likely causes with the evidence for each and what the user can repeat. About 12 credits, 22 when the reel is not in the account's listing.

## Method

1. **Baseline.** `instagram_get_profile` (1 credit) and two pages of `instagram_get_reels` (2 credits): median views, engagement over views, comments over likes and `duration_s`, read as [the data notes](data.md) say. Count the account's other outliers in those pages: is this its one hit or one of several?
2. **The reel.** Take its row from the listing; only if it is missing, `instagram_get_post` (estimated 10). Work out its multiple of the median, views over followers, and its ratios against the account's medians. If `is_ad: true`, the views may have been bought: say so first and weaken every factor below.
3. **What was said.** `instagram_get_transcript` on the reel and on three reels near the account's median (1 credit each): hook (first sentence), structure (setup, turn, payoff, call to action), topic and length, and what differs. `NoData` means the hook is on screen; a reel over two minutes has no transcript.
4. **What people reacted to.** `instagram_get_comments`, three pages, replies off (3 credits). Comments arrive newest first, so rank the pulled ones by likes yourself. Count friends tagged (people sharing it, the nearest public trace of sends), questions, disagreement, requests for a part 2, and lines people quote back.
5. **Timing and trend.** From `created_at` in the audience's time zone: day and hour against the account's usual, the gap since its previous reel, and the reel's age (under 48 hours, the numbers are still moving). Then `instagram_search_posts` on its main tag with `since: "month"`, two pages, and the `music` on those rows: many earlier posts on the topic or sound mean it rode a trend; few mean it may have started one.
6. **Deliver** a table: factor (hook, topic, format, length, timing, trend or sound, what drove comments, collab or repost), this reel, the account's usual, verdict (likely cause, possible, not a cause) and the evidence. Then three lines: what to repeat, how the user would do it on their account, and what cannot be copied.

## Judgment

- One reel proves little. Rank factors by the strength of their evidence, and drop any factor the account's ordinary reels share.
- Views well above the follower count mean Instagram showed it far beyond the followers: the cause is in the reel, not the audience. That reach runs on sends and watch time to non-followers (see [ranking](ranking.md)), neither of which the tools can see; for the user's own reel, Insights shows sends, non-follower views, skip rate and the retention curve, and those settle most arguments.
- Comments far above the account's usual share of likes mean debate, a giveaway, or a comment-for-DM prompt; read which before calling it controversy.
- A reel posted as a Collab, or reposted by a large account, borrows another audience. Check `collaborators` and the comments for that before crediting the content.
- Some breakouts have no cause the data shows. Say so rather than invent one.
- Write a script only when asked, from what the table shows; never pass off the creator's reel or words as the user's, and remember a near-copy is unoriginal content under Instagram's rules.
