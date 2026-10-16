# Hooks and trends

How the best TikToks in a niche open, and what formats, topics, lengths and sounds are rising this week. Both start from the same move: find the videos that beat their own author's median, not the ones with the most views. A big account's ordinary video says nothing about its hook or about a trend.

## Collecting the sample

- **From niche terms:** `tiktok_search_videos` for three or four terms (the category, a problem the product solves, a use case), `sort: "popular"`, two pages each (30 rows a page, 1 credit). `since: "week"` for trends; `since: "month"` for hooks or a slow niche. A term with fewer than about 20 results in a week is too thin for a weekly read: widen to a month and say the window changed.
- **From accounts:** `tiktok_get_videos` with `sort: "latest"`, two pages each, which gives both the recent videos and the author's median.
- **Clean:** drop `is_ad` rows (in our "budget app" sample they held 87% of the views), duplicates across terms, captions in other languages, giveaways.
- **Speed:** rank by views per day since `created_at`. Search's popular sort is by likes and favours older videos.
- **Lift:** for the 10 fastest authors, one page of `tiktok_get_videos` each (1 credit) for their median. Keep videos at 3 times their author's median or more. For hooks, also drop videos whose engagement rate is under half their author's median: the opening won the view and the video did not hold it.

About 28 credits for a weekly trend read on four terms, about 40 for a hook table of 20 videos.

## Hooks

- **Read the opening.** `tiktok_get_transcript` on each kept video (1 credit). The first spoken sentence is the spoken hook (transcripts carry no timestamps; when it runs long, take its first clause); the first line of `text` is the caption hook. `NoData` means the hook is on screen or in the sound, which the tools cannot read: keep the caption line and mark it. Use `ai_fallback: true` (11 credits) only on videos under 2 minutes the table cannot do without. Pass `language` for videos with several transcripts.
- **Sort into patterns:** a question to the viewer; a bold or contrarian claim; a number or list; a callout of who it is for ("If you run a Shopify store..."); a story opening; the result first; a mistake or warning ("Stop doing..."); a POV or skit setup; a reply to a comment. Count videos and take the median views and median lift per pattern. Report patterns with three or more videos first.
- **Know what is invisible.** Text on screen, the first frame and the cut are often the hook on TikTok. If most kept videos have no speech, say the niche's hooks are visual and the table shows half of them.
- **Hooks work only if the video holds.** Median watch time on long videos is about 11 seconds (Buffer, 2025): a strong hook on a video that sags loses the For You signal that matters, finishing. Pair every hook with the payoff it promises.
- **Adapt the pattern, never the words.** Quoted hooks are evidence, not scripts. Write new ones only when asked, each on a pattern the table shows.

## Trends

- **A trend is three or more videos from three or more authors in the window, each beating its author's median.** One viral video is not a trend; a big account posting at its usual numbers is no evidence of one.
- **Name the format** from transcript and caption (talking head, voiceover, list, tutorial, before and after, reply, skit, carousel, text over music), the **topic** from recurring words and `hashtags`, the **length** from `duration_s` (under 15 seconds, 15 to 60, over 60) and the **sound** from `music` (see [content](content.md#music-and-sounds)).
- **Date it.** Trends on TikTok fade within one to three weeks; one whose oldest example is two weeks old is probably at its peak. Put the pull date on the table.
- **Check the audio before recommending it to a brand.** A trend that depends on a popular song is closed to a business account unless the track is in the Commercial Music Library, and labels sue brands over it.
- **The trend is the format or topic, never the video.** Advise making the brand's own version.
- **For a weekly read,** the host schedules the same run and keeps last week's table; the server keeps no state.

## Deliverables

- **Hooks:** pattern, what it does in a line, videos, median views, median lift, two or three hooks quoted word for word with link, author and views, spoken or caption.
- **Trends:** trend, what it is, videos, authors, median views per day, median lift, two example links with their first line, whether it needs a song, how the user could use it, pull date.
- Keep TikTok tables apart from Reels or Shorts tables: views compare only within one platform ([instagram](../../instagram/SKILL.md), [youtube](../../youtube/SKILL.md)). Hooks from paid TikTok ads belong to [paid-ads](../../paid-ads/SKILL.md).
