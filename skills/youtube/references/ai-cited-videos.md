# AI-cited videos

The YouTube videos that AI engines cite, and that Google shows, when buyers ask the user's questions. For each one: who made it, whether it names the user, and the play: get featured in it, make a better one, or leave it. It is the YouTube part of citation building.

## Inputs to settle first

- **Answers**: a visibility check run from this conversation or an earlier one (its `task_id`; results are kept 30 days). If there is none, run one first: open the `ai-search` group's [visibility check](../../ai-search/references/visibility-check.md) and do its steps 1 and 2. Choosing the prompts lives there; do not rebuild it here.
- **Brand and competitors**: the same `brands` as that run.
- **Google queries**: the short search twin of each prompt ("best crm for agencies" for "what's the best CRM for a small agency?"). Default: built from the prompts.
- **What the user can offer a creator**: a trial, data, a sponsorship budget, or nothing yet. The play depends on it.
- **Budget**: with an existing run, about 10 x 2 + 15 x 3 + 5 = 70 credits for ten Google queries and 15 cited videos. Without one, add the visibility check (about 270). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the answers.** `get_task` with the visibility check's `task_id` (free). From every row, keep the `citations[]` whose `domain` is youtube.com (with or without www or m) or youtu.be. For each video URL, count the engines and the prompts that cite it, and note the brands mentioned in those rows (`mentions[]`) and whether the user is one of them.
2. **Check Google.** `seo_get_serp` with `ai_overview: true` on each Google query (2 credits each). Keep the organic `results` whose `domain` is youtube.com, the `ai_overview` references on youtube.com, and whether `features` includes `video`. The video block's own row carries no video URLs, so which videos fill it is unknown; `youtube_search_videos` on the same query (1 credit) shows the likely ones. Mark those as likely, not proven.
3. **Read each video.** For the cited and ranking videos, up to 15: `youtube_get_video` (1 credit) for views, likes, comments, age and channel; `youtube_get_channel` on its `author` (1 credit) for subscribers and the `bio`; `youtube_get_transcript` (1 credit) to see whether it names the user and which competitors, what kind of video it is (review, comparison, tutorial, list) and how current its facts are.
4. **Choose the play for each video.**
   - **Already in**: the video names the user fairly. Note it; nothing to do but keep the facts current for the creator.
   - **Get featured**: an active creator (the [channel health](../SKILL.md#channel-health) rules), a review, comparison or list format, and the user missing. The ask is an update, a pinned correction, or a place in the next video, sponsored if the user has budget. Size the creator with [find creators](find-creators.md) and vet with the `influencers` group's [vet a creator](../../influencers/references/vet-creator.md). The contact is in the channel `bio`, or through the [contact steps](../../link-building/SKILL.md#contact-steps) on the creator's own site.
   - **Make a better video**: the cited video is old, from a paused or tiny channel, or a competitor's own. The user makes the video that answers the prompt better; [video ideas](video-ideas.md) gives the angle.
5. **Deliver** a table: video URL, title, channel, subscribers, views, age, cited by (engines, prompts), on Google (organic rank, AI overview reference, likely in the video block), names the user (yes, no), competitors named, play (already in, get featured, make a better video), and the next step.

## Judgment

- AI answers are live and non-deterministic. A video cited by two or more engines, or for two or more prompts, is a target; one cited once is noise unless it is the only source for a prompt that matters.
- A competitor's own video cited for a category prompt cannot be joined. The only play is a better video of the user's own.
- A creator who reviews tools for a living may ask for a fee. A paid mention must be disclosed as a paid promotion on YouTube; never suggest hiding it.
- Google's AI overview and AI Mode follow Google's rankings, so a video that ranks in `seo_get_serp` for the query is likelier to be cited there too.
- For every other source type in those answers (articles, lists, Reddit threads, review sites), use the `ai-search` group's [citation building](../../ai-search/references/citation-building.md). To re-check citations on a schedule, the `monitoring` group's [AI visibility tracking](../../monitoring/references/ai-visibility-tracking.md).
