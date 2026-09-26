# Repurposing

One strong piece can feed a week of posts. This playbook takes one source (a YouTube video, a TikTok, an Instagram reel, a Facebook video, a LinkedIn post or an X post), reads its words and its numbers, picks the parts worth reusing and maps each to a channel and a format. It ends in a map from the source to several posts; the host drafts them from the source's own words when the user asks.

## Inputs to settle first

- **Source**: one URL. Video to post: YouTube, TikTok, an Instagram reel, a Facebook video or reel. Post to post: a LinkedIn post or an X post. A podcast or webinar works when it is on YouTube; otherwise the user pastes the transcript. A blog post works only if the user pastes it or the host can open the page: no manifold tool reads a page's body text.
- **Whose it is**: the user's own piece, or someone else's. Someone else's is quoted and credited, never passed off (see Judgment).
- **Target channels**: default the channels the user already posts on, from [channels](channels.md) if it ran.
- **Voice**: optional. For the user's own X account, `twitter_get_tweets` on their handle (1 credit); for LinkedIn, `linkedin_get_post` on two of their best posts (1 credit each). The host matches the drafts to it.
- **Budget**: about 1 + 1 + 1 + 3 = 6 credits for a YouTube video or a post; up to about 25 for a TikTok that needs `ai_fallback: true` and a media fetch. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the source.**
   - Video: `youtube_get_transcript`, `tiktok_get_transcript`, `instagram_get_transcript` or `facebook_get_transcript` (1 credit each). The text comes whole, without timestamps. `NoData` means no speech was kept: on TikTok, retry with `ai_fallback: true` (11 credits, videos up to 2 minutes); elsewhere, ask the user for a transcript. Instagram transcribes reels up to two minutes and refuses longer ones as `InvalidTarget`.
   - Post: `linkedin_get_post` or `twitter_get_tweet` (1 credit): the text and the engagement in one call. The text is cut at 2,000 characters; ask the user for the rest of a longer post.
2. **Read the numbers.** For a video, list the account's recent work with `youtube_get_videos`, `tiktok_get_videos`, `instagram_get_reels` or `facebook_get_posts` (1 credit a page). If the source is on that page, its row has the numbers; if not, `youtube_get_video` or `facebook_get_post` (1 credit), or `tiktok_get_video` or `instagram_get_post` (up to 10). For an X post, `twitter_get_tweets` on the author gives the comparison. Compare the source's views and engagement with the median of the page: a source above the account's median is worth repurposing widely. LinkedIn has no listing with engagement, so judge a LinkedIn post on its likes and comments alone.
3. **Pick the parts.** Read the text for the pieces that stand alone: the hook (the first line, or the first sentences spoken), each claim with a number, a list or a framework, a story with a result, a strong opinion, a question answered, and lines worth quoting. For clips, mark each part by its opening and closing words so an editor can find it; the transcript has no times.
4. **Check the angles (optional).** For the two or three strongest parts, one search on the target channel (1 credit a page): `linkedin_search_posts`, `tiktok_search_videos` or `youtube_search_videos` with the part's topic and `since: "month"`. Posts on that topic with engagement above the channel's usual confirm the angle and show the format that works there.
5. **Map to formats.** One row per derived post. A long video typically yields three to five short clips, two or three LinkedIn posts, one X thread and one newsletter section; a single post yields a version per channel and, if it did well, a short video script. A part that answers a searched question can become an article, which the `seo` group's brief plans.
6. **Deliver** a table: number, target channel, format (text post, thread, carousel outline, short video script, clip, newsletter section, article), the part of the source it uses (the quote, or its opening and closing words), the hook, the claim or number it carries, the evidence for the angle (a search row from step 4, if run), and a suggested day relative to the source. Hand the rows to [calendar](calendar.md) to place them. The host drafts each post from the source's own words only when the user asks.

## Judgment

- Repurpose winners. A source that did worse than the account's median rarely does better on another channel; say so and suggest a stronger source.
- Each channel gets a native version, not a paste. A LinkedIn post has to earn the click on "see more" in its first two lines; an X post fits in 280 characters or runs as a thread; a short video states its claim in the first sentence.
- Keep every claim to what the source says. The drafts quote the transcript's numbers and examples, and add none.
- Someone else's video or post is a reference, not material. Quote it with credit and a link, or take its question and answer it in the user's own words. To study why it worked, use the `tiktok` group's [viral breakdown](../../tiktok/references/viral-breakdown.md).
- A transcript is speech only. On-screen text, slides and demos are not in it, so a clip that depends on the visuals needs the user to check it.
- One source, spread over a week or two, beats the same post pasted on five channels on one day. The calendar spaces it.
