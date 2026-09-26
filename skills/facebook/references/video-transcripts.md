# Facebook video transcripts

What a page's videos and reels say, as text: the hook, the problem named, the claims, the proof and the call to action, set beside how each video did. Use it on a competitor's page to learn what they say and which lines work, or on the user's own page to collect material to reuse. It ends in a table per video.

## Inputs to settle first

- **Source**: video or reel URLs, or a page (name or URL) to pick videos from.
- **How many**: default the 10 videos with the most views among the page's last 50 or so posts.
- **Language**: pass `language` when the videos are not in English.
- **Purpose**: learn a competitor's messaging, collect hooks, or reuse the user's own videos. It decides what step 3 looks for.
- **Budget**: a default run costs about 5 + 10 = 15 credits (five pages of posts, ten transcripts). With URLs instead of a page, about 2 credits per video. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the videos.** From a page: `facebook_get_posts` with `handle` or `url` (1 credit per page of results), paging with `meta.cursor` to about 50 posts. Keep the rows with `media: "video"` and rank them by `views`, or by `likes` plus `comments` where `views` is null. From URLs: `facebook_get_post` on each (1 credit) for the same numbers.
2. **Transcribe.** `facebook_get_transcript` with each video's `url` (1 credit, cached 30 days), and `language` where needed. `NoData` means the video carries no speech (music over captions, a silent demo): note it, it still cost 1 credit, and do not retry.
3. **Read each transcript.** Take the hook (the first sentence or two, what a scroller hears in the first seconds), the problem it names, the claims (numbers, promises, comparisons), the proof (a demo, a customer, data), the offer and the call to action. Note the length from `duration_s`.
4. **Compare.** Set the top half by views against the bottom half: the kind of hook (a question, a bold claim, a story, a demo), the length, the topic and the call to action. Name what the best videos share that the rest lack.
5. **Deliver** a table: video URL, date, length, views, likes, comments, hook (verbatim), main claim, proof, call to action, and why it worked or did not in one line. Below it, the patterns from step 4 in three to five lines, and the full transcripts of the top three if the user wants them.

## Judgment

- A transcript is speech only. On-screen text, captions burned into the video and visuals are not in it, and many Facebook videos rely on them. Say so when a top video's transcript is thin.
- The hook decides most of a video's reach. Compare first lines before anything else.
- Facebook counts a video view after a few seconds of play, so a high view count with few comments can mean people scrolled past. Read `views` together with `comments`.
- Use a competitor's transcript for patterns and claims to answer, not for lines to copy.
- Turning the user's own transcripts into posts, threads or a blog is the `content` group's [repurposing](../../content/references/repurposing.md).
- A competitor's video ads are in the ad library, not on the page: the `paid-ads` group's [competitor ads](../../paid-ads/references/competitor-ads.md).
