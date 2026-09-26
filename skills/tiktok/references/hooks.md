# TikTok hooks

How the niche's best videos open: the first spoken line from their transcripts and the first line of their captions, sorted into patterns. It ends in a table of hook patterns, each with real examples and links, that the user can adapt.

## Inputs to settle first

- **Source**: niche terms (default: the category and two problems it solves), or a list of accounts the user wants to learn from (competitors, creators they admire).
- **Window**: `since: "month"` by default; `since: "year"` for a small niche, which on TikTok reaches back about six months.
- **Sample**: 20 videos by default. Fewer than 12 is too few to see patterns.
- **Budget**: a default run costs about 3 x 2 + 10 + 20 = 36 credits for three terms. Each `ai_fallback` transcript adds 10. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Collect the top videos.** From niche terms: `tiktok_search_videos` for each term with `since: "month"` and `sort: "popular"`, two pages each (1 credit a page). From accounts: `tiktok_get_videos` with `sort: "popular"`, one page each (1 credit). Drop rows with `is_ad: true` and videos under the engagement [floor](../SKILL.md#floors).
2. **Keep the videos the opening lifted.** For up to 10 authors, `tiktok_get_videos` with `sort: "latest"`, one page each (1 credit), for their median views. Keep the 20 videos with the highest multiple of their author's median, not the most views: a big account's ordinary video says nothing about its hook.
3. **Read the opening.** `tiktok_get_transcript` on each kept video (1 credit). The first spoken sentence is the spoken hook; the first line of `text` is the caption hook. `NoData` means no speech TikTok kept: the hook is on screen, which the tools cannot read, so keep the caption line and mark it. Use `ai_fallback: true` (11 credits) only on videos under 2 minutes that the table cannot do without.
4. **Sort into patterns.** Put each hook in one pattern: a question to the viewer, a bold or contrarian claim, a number or list ("3 things..."), a callout of who it is for ("If you run a Shopify store..."), a story opening ("I got fired and..."), the result first, a mistake or warning ("Stop doing..."), a POV or skit setup, a reply to a comment. Count the videos, and take the median views and the median multiple for each pattern.
5. **Deliver** a table: pattern, what it does in one line, videos using it, median views, median multiple of the author's median, two or three hooks quoted word for word each with the video link, author and views, and whether the hook is spoken or in the caption. If the user asked, add three hooks for their own topic under each of the top three patterns.

## Judgment

- A hook is the first one to three seconds. Transcripts carry no timestamps, so the first sentence is the nearest reading; when it runs long, take its first clause.
- The tools read words, not pictures. Text on screen, the first frame and the cut are invisible. If most top videos have no speech, say that the hooks in this niche are visual and the table shows only half of them.
- Report patterns with three or more videos first. A pattern seen once is an anecdote.
- For videos with transcripts in several languages, pass `language` to get the one the user needs.
- Adapt the pattern, never the words. Quoted hooks are evidence, not scripts to reuse.
- Hooks from paid TikTok ads belong to the [paid-ads](../../paid-ads/SKILL.md) group's [swipe file](../../paid-ads/references/swipe-file.md).
