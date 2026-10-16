# Repurposing

Turning one strong piece (a YouTube video, a TikTok, a reel, a Facebook video, a LinkedIn or X post, a podcast or webinar on YouTube) into native posts for other channels. It ends in a map from the source to several posts; the host drafts them from the source's own words only when asked. Checked October 2026.

## What the platforms penalise

- **Meta (14 July 2025):** accounts that repeatedly reuse others' videos, photos or text without crediting them lose monetisation for a time and get lower reach on all their posts; detected duplicate videos get less distribution. Using others' material is allowed with meaningful change (commentary, voiceover, real editing), and Meta says stitching clips together or adding a watermark does not count. Sharing the same content across one's own Pages is allowed.
- **YouTube (15 July 2025):** the Partner Program's "repetitious content" became "inauthentic content": mass-produced, templated or repetitive uploads (slideshows, scrolling text with little narrative) cannot be monetised. Reused content without significant original commentary or change was already excluded, and permission from the original creator does not change that.
- **Instagram (April 2024):** ranking favours original content and, when it finds an identical repost, recommends the original instead (Instagram for Creators, "Helping Creators Find New Audiences").

So: repurposing your own material into a native version per platform is safe and cheap. Posting the same exported file everywhere, with another platform's watermark, is the version that gets demoted. Someone else's piece is a reference, never material: quote it with credit and a link, or take its question and answer it in the user's own words.

## Repurpose winners

A source that did worse than the account's median rarely does better elsewhere; say so and suggest a stronger source. Compare the source with a page of the account's recent posts (`youtube_get_videos`, `tiktok_get_videos`, `instagram_get_reels`, `facebook_get_posts` or `twitter_get_tweets`, 1 a page). LinkedIn has no listing with engagement, so judge a LinkedIn post on its own likes and comments. `tiktok_get_video` and `instagram_get_post` cost up to 10 when the media has to be fetched; a listing row already carries the numbers.

## Reading the source

- **Video:** `youtube_get_transcript`, `tiktok_get_transcript`, `instagram_get_transcript` or `facebook_get_transcript` (1 each, cached 30 days). The text comes whole, without timestamps, so mark clips by their opening and closing words. `NoData` means no speech was kept: on TikTok retry with `ai_fallback: true` (11 credits, videos up to 2 minutes); elsewhere ask for a transcript. Instagram transcribes reels up to two minutes and refuses longer ones.
- **Post:** `linkedin_get_post` or `twitter_get_tweet` (1): text and engagement together, text cut at 2,000 characters.
- **A blog post** only if the user pastes it or the host can open the page; no Manifold tool reads a page's body text.
- A transcript is speech only. On-screen text, slides and demos are missing, so a clip that depends on the visuals needs the user's check.

## The map

- **Pick the parts that stand alone:** the hook (the first line or first sentences spoken), each claim with a number, a list or framework, a story with a result, a strong opinion, a question answered, quotable lines.
- **Check the strongest two or three angles (optional):** one search on the target channel with the part's topic and `since: "month"` (`linkedin_search_posts`, `tiktok_search_videos` or `youtube_search_videos`, 1 a page). Posts on that topic with engagement above the channel's usual confirm the angle and show the format that works there.
- **Typical yield:** a long video gives three to five short clips, two or three LinkedIn posts, an X thread and a newsletter section; a single post gives a version per channel and, if it did well, a short video script. A part that answers a searched question can become an article ([seo](../../seo/SKILL.md)).
- **Native, not pasted:** a LinkedIn post earns the "see more" click in its first two lines; an X post fits 280 characters or runs as a thread; a short video states its claim in the first sentence.
- **Keep every claim to the source:** the drafts quote its numbers and examples and add none.
- **Space it:** one source over one or two weeks beats the same post on five channels on one day. The [calendar](content.md#laying-out-the-calendar) places the rows.

**The table:** number, target channel, format, the part used (the quote, or its opening and closing words), the hook, the claim or number it carries, the evidence for the angle, and a suggested day relative to the source. About 6 credits for a YouTube video or a post, up to about 25 for a TikTok that needs `ai_fallback` and a media fetch. Never post, schedule or publish.
