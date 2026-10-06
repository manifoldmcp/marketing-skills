---
name: repurpose-content
description: When the user wants to turn one piece of content into posts for other channels. Reads one YouTube video, TikTok, Instagram reel, Facebook video, LinkedIn post or X post through its transcript or text, checks its numbers against the account's median, picks the parts worth reusing and maps each to a channel and format, such as clips, LinkedIn posts, an X thread or a newsletter section. Also use when the user mentions repurposing, repurpose this video, turn our podcast into LinkedIn posts, make an X thread from this webinar, reuse this LinkedIn post on other channels, clips and posts from one YouTube video, or content atomization. Why a video went viral goes to analyze-viral-tiktok, analyze-viral-reel or analyze-viral-youtube-video; what a competitor's Facebook videos say to audit-facebook-page; placing the posts on dates to create-content-calendar. Posting and scheduling are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Content repurposing

One strong piece can feed a week of posts. This skill takes one source (a YouTube video, a TikTok, an Instagram reel, a Facebook video, a LinkedIn post or an X post), reads its words and its numbers, picks the parts worth reusing and maps each to a channel and a format. It ends in a map from the source to several posts; the host drafts them from the source's own words when the user asks.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for the reading tool of the source's platform, such as `youtube_get_transcript`, `tiktok_get_transcript` or `linkedin_get_post` (hosts often add a prefix, for example `mcp__manifold__youtube_get_transcript`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the source platform's tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop, or ask the user to paste the transcript. If only a target channel's tools are off, skip its angle check in step 4 and mark it as not measured.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the channels and handles the user posts on, the brand voice) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Source**: one URL. Video to post: YouTube, TikTok, an Instagram reel, a Facebook video or reel. Post to post: a LinkedIn post or an X post. A podcast or webinar works when it is on YouTube; otherwise the user pastes the transcript. A blog post works only if the user pastes it or the host can open the page: no manifold tool reads a page's body text.
- **Whose it is**: the user's own piece, or someone else's. Someone else's is quoted and credited, never passed off (see Judgment).
- **Target channels**: default the channels the user already posts on, from [pick-channels](../pick-channels/SKILL.md) if it ran.
- **Voice**: optional. For the user's own X account, `twitter_get_tweets` on their handle (1 credit); for LinkedIn, `linkedin_get_post` on two of their best posts (1 credit each). The host matches the drafts to it.
- **Budget**: about 1 + 1 + 1 + 3 = 6 credits for a YouTube video or a post; up to about 25 for a TikTok that needs `ai_fallback: true` and a media fetch. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the source.**
   - Video: `youtube_get_transcript`, `tiktok_get_transcript`, `instagram_get_transcript` or `facebook_get_transcript` (1 credit each). The text comes whole, without timestamps. `NoData` means no speech was kept: on TikTok, retry with `ai_fallback: true` (11 credits, videos up to 2 minutes); elsewhere, ask the user for a transcript. Instagram transcribes reels up to two minutes and refuses longer ones as `InvalidTarget`.
   - Post: `linkedin_get_post` or `twitter_get_tweet` (1 credit): the text and the engagement in one call. The text is cut at 2,000 characters; ask the user for the rest of a longer post.
2. **Read the numbers.** For a video, list the account's recent work with `youtube_get_videos`, `tiktok_get_videos`, `instagram_get_reels` or `facebook_get_posts` (1 credit a page). If the source is on that page, its row has the numbers; if not, `youtube_get_video` or `facebook_get_post` (1 credit), or `tiktok_get_video` or `instagram_get_post` (up to 10). For an X post, `twitter_get_tweets` on the author gives the comparison. Compare the source's views and engagement with the median of the page: a source above the account's median is worth repurposing widely. LinkedIn has no listing with engagement, so judge a LinkedIn post on its likes and comments alone. Read the numbers as the platform's notes say: [TikTok](../create-tiktok-plan/references/platforms/tiktok.md), [Instagram](../create-instagram-plan/references/platforms/instagram.md), [YouTube](../create-youtube-plan/references/platforms/youtube.md), [LinkedIn](../create-linkedin-plan/references/platforms/linkedin.md), [Facebook](../create-facebook-plan/references/platforms/facebook.md), [X](../audit-x-account/references/platforms/x.md).
3. **Pick the parts.** Read the text for the pieces that stand alone: the hook (the first line, or the first sentences spoken), each claim with a number, a list or a framework, a story with a result, a strong opinion, a question answered, and lines worth quoting. For clips, mark each part by its opening and closing words so an editor can find it; the transcript has no times.
4. **Check the angles (optional).** For the two or three strongest parts, one search on the target channel (1 credit a page): `linkedin_search_posts`, `tiktok_search_videos` or `youtube_search_videos` with the part's topic and `since: "month"`. Posts on that topic with engagement above the channel's usual confirm the angle and show the format that works there.
5. **Map to formats.** One row per derived post. A long video typically yields three to five short clips, two or three LinkedIn posts, one X thread and one newsletter section; a single post yields a version per channel and, if it did well, a short video script. A part that answers a searched question can become an article, which [write-seo-brief](../write-seo-brief/SKILL.md) plans.
6. **Deliver** a table: number, target channel, format (text post, thread, carousel outline, short video script, clip, newsletter section, article), the part of the source it uses (the quote, or its opening and closing words), the hook, the claim or number it carries, the evidence for the angle (a search row from step 4, if run), and a suggested day relative to the source. Hand the rows to [create-content-calendar](../create-content-calendar/SKILL.md) to place them. The host drafts each post from the source's own words only when the user asks.

## Judgment

- Repurpose winners. A source that did worse than the account's median rarely does better on another channel; say so and suggest a stronger source.
- Each channel gets a native version, not a paste. A LinkedIn post has to earn the click on "see more" in its first two lines; an X post fits in 280 characters or runs as a thread; a short video states its claim in the first sentence.
- Keep every claim to what the source says. The drafts quote the transcript's numbers and examples, and add none.
- Someone else's video or post is a reference, not material. Quote it with credit and a link, or take its question and answer it in the user's own words. To study why it worked, use [analyze-viral-tiktok](../analyze-viral-tiktok/SKILL.md), [analyze-viral-reel](../analyze-viral-reel/SKILL.md) or [analyze-viral-youtube-video](../analyze-viral-youtube-video/SKILL.md).
- A transcript is speech only. On-screen text, slides and demos are not in it, so a clip that depends on the visuals needs the user to check it.
- One source, spread over a week or two, beats the same post pasted on five channels on one day. The calendar spaces it.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. `tiktok_get_video` and `instagram_get_post` cost 10 when the vendor has to fetch the media; a listing row already carries the numbers, so call them only when the source is not on the page. Transcripts this account already paid for are free for 30 days.
- **Handoff.** The server does no content generation. Never post, schedule or publish; the host keeps the table, and if it has a scheduling or social tool, offers to pass the rows to it.

## Related skills

- Placing the derived posts on dates: [create-content-calendar](../create-content-calendar/SKILL.md). Fresh ideas rather than a source to reuse: [find-content-ideas](../find-content-ideas/SKILL.md).
- Why a video beat its account's usual numbers: [analyze-viral-tiktok](../analyze-viral-tiktok/SKILL.md), [analyze-viral-reel](../analyze-viral-reel/SKILL.md) or [analyze-viral-youtube-video](../analyze-viral-youtube-video/SKILL.md).
- What a competitor's Facebook videos say, video by video: [audit-facebook-page](../audit-facebook-page/SKILL.md).
- An article from a part that answers a searched question: [write-seo-brief](../write-seo-brief/SKILL.md).
