# Auditing a channel, and why one video took off

Two jobs on public numbers: what a channel publishes and what works for it (the user's or a competitor's, side by side), and why one video did far better than its channel usually does. Both rest on the rules in [channel data](channel-data.md#channel-health); read that first.

## A channel audit

**Inputs.** Two or three competitor channels by handle, and the user's own to compare. Given only company names, search YouTube for the brand and take the channel whose `author_name` matches; a channel that only reviews the brand is not theirs. If the user does not know the competitors, [competitor-intel](../../competitor-intel/SKILL.md) finds them first.

**What to read, per channel** (about 15 credits a channel, 60 for the user and three rivals):

- Size: subscribers, lifetime views, video count and age from `youtube_get_channel`.
- The current strategy, from the latest 20 videos: uploads a month over the last 90 days, the median views of the latest 10 (under-14-day videos out), the median as a share of subscribers, the length mix from `duration_s` (under 10 minutes, 10 to 30, over 30), and the formats from the titles (tutorial, comparison, review, customer story, webinar recording, podcast, product update).
- What has worked: the popular sort, with outliers marked against the recent median and dated. A popular list of five-year-old hits says what worked then.
- Engagement on the shortlist: `youtube_get_video` on the top five outliers and three recent videos.
- Why the winners work: transcripts of the top three outliers. Note the hook in roughly the first 80 words, the structure, and the call to action (a trial, a template, a demo).

**Deliver** one column per channel, the user's included: subscribers, videos, uploads a month, recent median, median over subscribers, length mix, main formats, top three topics, and the top five videos (title, URL, views, outlier ratio, likes and comments per 1,000 views). Under it, the hooks and calls to action that recur in the winners, and three gaps: topics or formats that win for a competitor and that the user has not covered.

**Judgment.**

- Judge a video against its own channel's median, never against YouTube at large, and compare channels only over the same window.
- A falling median across the latest videos means the audience is fading, whatever the subscriber count. Say so; it is an opening.
- Paid and recorded videos stay out of the medians ([channel data](channel-data.md#channel-health)). A product demo with a million views and a handful of comments was probably run as an ad: it shows ad spend, not demand.
- Since 24 August 2026 views count from the first frame ([platform](platform.md#what-a-view-is-now)); compare the competitor's recent videos with the user's recent videos, not with their 2025 ones.
- What a channel's Studio would show (CTR, retention, traffic sources) is invisible for competitors. Do not guess it.
- A weekly watch on a rival's uploads is a monitoring job: [competitor-intel](../../competitor-intel/SKILL.md). The rest of a rival's marketing is there too.

## One video that took off

**Inputs.** The video URL; optionally the user's own handle, to fit "what to repeat" to their numbers. About 16 credits.

**The baseline.** The channel's size, the recent median and the popular list, then `youtube_get_video` on three videos near the median for their usual likes and comments. Compare views per day since upload, since views are lifetime.

**The video.** Its multiple of the median views per day, views over subscribers, and its likes and comments per 1,000 against the channel's. If `is_ad` is true, say so first: its views may have been bought, and every factor below is weaker evidence.

**What was said.** The transcript of the video and of three videos near the median. The hook (the first sentence), the structure (set-up, turn, payoff, call to action), the topic, the length against the channel's usual, and what differs.

**What people reacted to.** Three pages of comments, most liked first. Count friends tagged (sharing), questions, disagreement, requests for a part two, and lines quoted back: a liked comment that quotes the video names the line that landed. Comments over likes well above the channel's median mean a debate.

**Timing.** The day and hour in the audience's time zone against the channel's usual, the gap since the previous upload, and the video's age (under 48 hours, still moving). Then a search on the topic with `since: "month"`, two pages: many videos on it before this one means it rode a trend; few means it may have started one.

**Deliver** a table: factor (hook, title, topic, format, length, timing, trend, what drove comments), this video, the channel's usual, verdict (likely cause, possible, not a cause) and the evidence (a quote, a number, a link). Then three lines: what to repeat, how the user would do it on their channel, and what cannot be copied.

**Judgment.**

- One video cannot prove a cause. Rank factors by the strength of their evidence and drop any the channel's ordinary videos share: it is not what made this one different.
- Views above the subscriber count mean YouTube showed it far beyond the channel's subscribers: the cause is in the video or its packaging, not the audience.
- The title and thumbnail can carry a video the content does not explain. The tools see the title, not the thumbnail; say so when the transcript shows nothing unusual.
- The biggest factors, watch time, satisfaction, CTR and where the views came from ([platform](platform.md#recommendations-are-pulled-per-viewer)), are not in the tools. For the user's own video, YouTube Studio has them; ask.
- A late surge on an old video is normal; YouTube says evergreen videos stay eligible. It often means the topic started trending or a search began sending traffic, so check the topic's recent uploads.
- Some breakouts have no cause the data shows. Say so rather than invent one.

## Both jobs

Deliver tables with a link to every video and channel cited. Never post, comment, subscribe or message. Write scripts or hooks only when the user asks, from what the table shows, and never pass off a creator's video or words as the user's.
