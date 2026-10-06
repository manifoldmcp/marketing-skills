# YouTube videos

The YouTube videos that AI engines cite, and that Google shows, when buyers ask the user's questions. For each one: who made it, whether it names the user, and the play: get featured in it, make a better one, or leave it.

## Inputs to settle first

- **Answers**: an [check-ai-visibility](../../check-ai-visibility/SKILL.md) run from this conversation or an earlier one (its `task_id`; results are kept 30 days). If there is none, run its steps 1 and 2 first. Choosing the prompts lives there; do not rebuild it here.
- **Brand and competitors**: the same `brands` as that run.
- **Google queries**: the short search twin of each prompt ("best crm for agencies" for "what's the best CRM for a small agency?"). Default: built from the prompts.
- **What the user can offer a creator**: a trial, data, a sponsorship budget, or nothing yet. The play depends on it.
- **Budget**: with an existing run, about 10 x 2 + 10 + 15 x 3 = 75 credits for ten Google queries, their video searches and 15 cited videos. Without one, add the check-ai-visibility check (about 270).

## Steps

1. **Read the answers.** `get_task` with the run's `task_id` (free). From every row, keep the `citations[]` whose `domain` is youtube.com (with or without www or m) or youtu.be. For each video URL, count the engines and the prompts that cite it, and note the brands mentioned in those rows (`mentions[]`) and whether the user is one of them. If no row cites YouTube, say so and carry on with step 2 alone.
2. **Check Google.** `seo_get_serp` with `ai_overview: true` on each Google query (2 credits each). Keep the organic `results` whose `domain` is youtube.com, the `ai_overview` references on youtube.com, and whether `features` includes `video`. The video block's own row carries no video URLs, so which videos fill it is unknown; `youtube_search_videos` on the same query (1 credit) shows the likely ones. Mark those as likely, not proven. If no engine cites a video and no query shows `video` or a youtube.com result, stop: video is not a source for these answers, and the [skill](../SKILL.md) covers what is.
3. **Read each video.** For the cited and ranking videos, up to 15: `youtube_get_video` (1 credit) for views, likes, comments, age and channel; `youtube_get_channel` on its `author` (1 credit) for subscribers and the `bio`; `youtube_get_transcript` (1 credit) to see whether it names the user and which competitors, what kind of video it is (review, comparison, tutorial, list) and how current its facts are.
4. **Choose the play for each video.**
   - **Already in**: the video names the user fairly. Note it; nothing to do but keep the facts current for the creator.
   - **Get featured**: an active creator (the [channel health](../../create-youtube-plan/references/platforms/youtube.md#channel-health) rules), a review, comparison or list format, and the user missing. The ask is an update, a pinned correction, or a place in the next video, sponsored if the user has budget. Size the creator with [find-creators](../../find-creators/SKILL.md) and vet with [vet-creator](../../vet-creator/SKILL.md). The contact is in the channel `bio`, or through the [contact steps](../../create-link-building-plan/references/outreach.md#contact-steps) on the creator's own site.
   - **Make a better video**: the cited video is old, from a paused or tiny channel, or a competitor's own. The user makes the video that answers the prompt better; [find-youtube-video-ideas](../../find-youtube-video-ideas/SKILL.md) gives the angle.
5. **Deliver** a table: video URL, title, channel, subscribers, views, age, cited by (engines, prompts), on Google (organic rank, AI overview reference, likely in the video block), names the user (yes, no), competitors named, play (already in, get featured, make a better video), and the next step.

## Judgment

- A video cited by two or more engines, or for two or more prompts, is a target; one cited once is noise unless it is the only source for a prompt that matters.
- A competitor's own video cited for a category prompt cannot be joined. The only play is a better video of the user's own.
- A creator who reviews tools for a living may ask for a fee. A paid mention must be disclosed as a paid promotion on YouTube; never suggest hiding it.
- Google's AI overview and AI Mode follow Google's rankings, so a video that ranks in `seo_get_serp` for the query is likelier to be cited there too.
