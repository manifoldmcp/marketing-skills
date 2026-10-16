# Why one TikTok went viral

Breaking down one video that did far better than its account usually does: what differed, how strong the evidence is, and what the user can repeat. One video cannot prove a cause; the job is to rank likely causes by evidence and drop the ones the account's ordinary videos share.

## What to pull

| Question | Call | Credits |
|---|---|---|
| The account's usual: medians of views, engagement, shares and comments over views, `duration_s` | `tiktok_get_profile`, then `tiktok_get_videos` with `sort: "latest"`, two pages | 3 |
| One hit or one of several? | `tiktok_get_videos` with `sort: "popular"`, one page | 1 |
| The video's own numbers | its row from the listings; `tiktok_get_video` only if it is not there | 0, or up to 10 |
| What was said, against ordinary videos | `tiktok_get_transcript` on it and on three videos near the median | 4 |
| What people reacted to | `tiktok_get_comments`, three pages (about 60 comments) | 3 |
| Did it ride a trend? | `tiktok_search_videos` on its topic, `since: "month"`, two pages | 2 |

About 13 credits when the video is in the listing, 23 when it is not, 33 with `ai_fallback: true` on a video with no transcript.

## Reading it

- **Paid first.** If the row is `is_ad: true`, its views may be bought (a Spark Ad runs a creator's post as an ad). Say so before anything else and treat every factor as weaker evidence. A like rate far under the account's organic median is the tell.
- **Views above followers** mean For You carried it well beyond the followers: the cause is in the video, not the audience.
- **Shares over views** well above the account's median mean people sent it on; **saves** mean it was worth keeping (a recipe, a list, a how-to); **comments over likes** well above the median mean a debate, or a comment-to-DM mechanic ("comment PATTY"): count one-word comments before calling it a debate.
- **The comments name the line that landed.** A liked comment that quotes the video, requests for a part 2, friends tagged, recurring questions. They also show distrust: under an AI-made product video with 7.1M views, the most liked comments asked where the paper came from and called it an AI ad ([field notes](field-notes.md)). Views are not approval.
- **Hook, structure, length, topic** against the account's ordinary videos: the first sentence, setup, turn, payoff and call to action, and whether it ran longer or shorter than usual. Hold length loosely: long videos earn more views only when people stay (median watch time on 1 to 10 minute videos is about 11 seconds, Buffer 2025).
- **Timing.** `created_at` in the audience's time zone against the account's usual hours; the gap since its previous video; its age (under 48 hours, the numbers are still moving).
- **Trend or first mover.** Many videos on the topic before it: it rode a trend. Few: it may have started one. Check the `music` field; a borrowed sound shared with other fast videos points to a sound trend.
- **What is not there.** Watch time, completion, retention and traffic sources decide distribution and are not in the tools. For the user's own video, TikTok Studio has them; ask.
- **No cause the data shows** is a valid answer. Say so rather than invent one.

## The deliverable

A table: factor (hook, topic, format, length, timing, trend or sound, comment drivers, share rate, paid), this video, the account's usual, verdict (likely cause, possible, not a cause) and the evidence (a quote, a number, a link). Then three lines: what to repeat, how the user would do it on their account, and what cannot be copied (the creator's face, an audience, a moment). Never pass off the creator's video or words as the user's.
