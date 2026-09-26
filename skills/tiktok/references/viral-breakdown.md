# Viral breakdown

Why one TikTok video did far better than its account usually does. It sets the video against the account's own baseline, reads its transcript and comments, and checks its timing. It ends in a table of likely causes with the evidence for each, and what the user can repeat.

## Inputs to settle first

- **Video**: the URL (required). The account is the handle in it.
- **Question**: why it worked, how the user could repeat it, or both. Default: both.
- **User's account**: the user's handle, if they want the "repeat it" part fitted to their own numbers. Optional.
- **Budget**: a default run costs about 1 + 2 + 1 + 10 + 1 + 3 + 3 + 2 = 23 credits, or 13 when the video is already in the account's listing. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **The baseline.** `tiktok_get_profile` on the author (1 credit) for followers. `tiktok_get_videos` with `sort: "latest"`, two pages (1 credit a page): the median views, median engagement rate, median shares over views, median comments over likes, median `duration_s`, and the days and hours the account usually posts. `tiktok_get_videos` with `sort: "popular"`, one page (1 credit): is this the account's one hit, or one of several?
2. **The video.** Take its row from the listings if it is there. If not, `tiktok_get_video` with the URL (estimated 10 credits, 1 when the vendor does not fetch the media). Work out its multiple of the median views, views over followers, and its shares over views and comments over likes against the account's medians. If `is_ad: true`, the video is an ad or a paid partnership and its views may have been bought: say so first, and treat every factor below as weaker evidence.
3. **What was said.** `tiktok_get_transcript` on the video (1 credit; `ai_fallback: true` makes it 11 when TikTok holds no transcript and the video is under 2 minutes). Read the hook (the first sentence), the structure (setup, turn, payoff, call to action), the topic and the length against the account's median. Then `tiktok_get_transcript` on three of the account's videos near its median (1 credit each) and say what differs.
4. **What people reacted to.** `tiktok_get_comments`, three pages (1 credit a page). Sort by likes. Count friends tagged (people sharing it), questions, disagreement, requests for a part 2, and any line people quote back. A liked comment that quotes the video names the line that landed.
5. **Timing.** From `created_at`: the day and hour against the account's usual, the gap since its previous video, and the video's age (under 48 hours, the numbers are still moving). Then `tiktok_search_videos` on the video's topic with `since: "month"`, two pages (1 credit a page): if many videos on the topic came before it, it rode a trend; if few did, it may have started one.
6. **Deliver** a table: factor (hook, topic, format, length, timing, trend, what drove comments, share rate), this video, the account's usual, verdict (likely cause, possible, not a cause), and the evidence (a quote, a number, a link). Below it, three lines: what to repeat, how the user would do it on their account, and what cannot be copied.

## Judgment

- One video cannot prove a cause. Rank the factors by the strength of their evidence, and drop any factor the account's ordinary videos share: it is not what made this one different.
- Views above the follower count mean TikTok showed the video far beyond the account's followers. The cause is in the video, not the audience.
- Shares over views well above the median mean people sent it on: it is useful or relatable. Comments over likes well above the median mean a debate: it divides people.
- The biggest factors in TikTok's distribution, watch time and completion rate, are not in the tools, and neither is where the views came from. Say so. For the user's own video, TikTok Studio has them.
- The rows carry no sound data. If the video rides a song or a sound, the user has to check it in the app.
- Some breakouts have no cause the data shows. When the evidence is thin, say so rather than invent one.
