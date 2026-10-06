---
name: audit-facebook-page
description: When the user wants to know what a Facebook page does and what works for it, their own or a competitor's. Reads each page's size, posting cadence, video share, which posts earn reactions, comments and shares against the page's own median, and the ads it runs, side by side with competitor pages, and transcribes a page's videos and reels into hooks, claims and calls to action. Also use when the user mentions a Facebook page audit, a competitor's Facebook page, what a brand posts on Facebook, is our Facebook page working, Facebook engagement, or their Facebook video scripts. TikTok goes to audit-tiktok-account, Instagram to audit-instagram-account, YouTube to audit-youtube-channel, LinkedIn to audit-linkedin-page, X to audit-x-account; a plan to grow on Facebook to create-facebook-plan; Facebook groups to mine-facebook-groups; a rival beyond social to tear-down-competitor.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Audit a Facebook page

A read on one Facebook page, the user's own or a competitor's: how big it is, how often it posts, which formats and topics earn reactions and comments, and what it pays to run. With competitor pages beside it, the audit becomes a benchmark. It ends in a scorecard per page, the posts that worked with a link to each, and three recommendations tied to the numbers. When the user wants only what a page's videos say, run the [video transcripts](#video-transcripts) part on its own.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `facebook_get_profile` (hosts often add a prefix, for example `mcp__manifold__facebook_get_profile`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `facebook_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop. Step 6 reads ads with the `ads_*` tools; without them, deliver the audit without the ads and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the user's own page, the competitors and their pages) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Pages**: the page to audit, by name or URL, and up to three competitor pages to compare. Check each is the brand's real page in step 2; brands often have regional and fan pages.
- **Window**: default the last 90 days, capped at 5 pages of posts for a page that posts often.
- **What the page is for**: community, traffic to the site, sales or support. It decides which number matters most: comments for community, link posts for traffic.
- **Budget**: about 1 + 5 + 3 + 1 = 10 credits a page (the record, five pages of posts, three posts in full, one page of ads), so about 40 for the user's page and three competitors. The video transcripts part alone costs about 15. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the platform notes** before the first call: [Facebook notes](../create-facebook-plan/references/platforms/facebook.md).
2. **Read the page.** `facebook_get_profile` with `handle` or `url` (1 credit, cached 24 hours): `followers`, `likes`, `bio`, `website`, `industry`, `created_at`. If `website` is not the brand's domain, or `followers` is far below the brand's other accounts, ask the user before going on: it may be a fan or regional page.
3. **Pull the posts.** `facebook_get_posts` with the same `handle` or `url` (1 credit per page of results), paging with `meta.cursor` until `created_at` passes 90 days back or 5 pages are read. For each post keep `created_at`, `media`, `likes`, `comments`, `views` and the first line of `text`. `media` is only `video` or `text` (an image post reads as text), and the listing carries no shares, length or paid flag, as the [Facebook notes](../create-facebook-plan/references/platforms/facebook.md#what-the-tools-can-read) say.
4. **Score it.** Posts per week; the share of video posts; the median of `likes` plus `comments` per post and per 1,000 followers; the median `views` on videos. Then rank posts by engagement against the page's own median, and read the top ten and bottom ten: topic, first line, video or not, a link or not, a question or not.
5. **Read the best posts in full.** `facebook_get_post` on the top three (1 credit each) for their `shares`, which the listing lacks, and the full text where it was cut. What people said under them is [mine-facebook-comments](../mine-facebook-comments/SKILL.md); what the top videos say is the [video transcripts](#video-transcripts) part below. Offer those rather than run them.
6. **Read the ads.** `ads_get_advertiser_ads` with `platform: "facebook"`, `advertiser` set to the page name or page id, and `active_only: true` (1 credit per page of results). Count the active ads, their formats and offers, and the oldest `first_shown` among them: an ad that has run for months is one that pays. An ad whose `body` repeats the first line of an organic post means the page boosted that post: mark it and take it out of the organic medians from step 4. A teardown of the ads themselves is the [research-meta-ads](../research-meta-ads/SKILL.md) skill's.
7. **Compare.** Run steps 2 to 6 for each competitor page, and set the pages side by side per 1,000 followers.
8. **Deliver** a scorecard table with one row per page: followers, posts per week, video share, median engagement per post, median engagement per 1,000 followers, median video views, active ads, and the page's best format. Then a table of the top posts: page, link, date, video or not, first line, likes, comments, shares, views, and why it worked in one line. End with three recommendations, each tied to a number in the tables.

## Judgment

- **Own baseline.** Judge a post against its own page's median, never against Facebook at large; a rival against the user only over the same window.
- **Engagement, not followers.** Followers pile up over years and include people who never see a post. Engagement per post is the live number; judge the page on it. A page with 200,000 followers and 15 reactions a post is not a working channel, whatever its size. Say so plainly.
- **Paid apart.** Boosted posts stay out of the organic medians and go in their own column: their reach was bought. An organic post that reappears in the page's ads is a winner it put money behind. If the competitor's page is quiet but its ads are many, its Facebook presence is paid. Say so; the organic comparison then says little.
- **Cadence.** Posting more is not the answer when engagement per post is falling. Compare the last 30 days with the 60 before them before recommending cadence. A rival whose cadence fell in the last month may have cut its effort: say so, it is an opening.
- **Repeatable, not lucky.** The count of outliers in the window says more than the single biggest post: six mean a repeatable format, one means luck. Formats a page keeps posting usually work for it; one it tried once and dropped probably did not.
- **What is not there.** The tools see what a visitor sees: no reach, no clicks, no audience demographics. For the user's own page those sit in Meta Business Suite; ask for an export if they matter.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Handoff.** The deliverable is a table with a link to every post and page it cites. Never post, comment, follow or message. A weekly watch of a rival's posts is the host's schedule, through [monitor-competitors](../monitor-competitors/SKILL.md).

## Video transcripts

What a page's videos and reels say, as text: the hook, the problem named, the claims, the proof and the call to action, set beside how each video did. Use it on a competitor's page to learn what they say and which lines work, or on the user's own page to collect material to reuse. It runs on its own when that is all the user wants, and ends in a table per video.

### Inputs

- **Source**: video or reel URLs, or a page (name or URL) to pick videos from.
- **How many**: default the 10 videos with the most views among the page's last 50 or so posts.
- **Language**: pass `language` when the videos are not in English.
- **Purpose**: learn a competitor's messaging, collect hooks, or reuse the user's own videos. It decides what step 3 looks for.
- **Budget**: a default run costs about 5 + 10 = 15 credits (five pages of posts, ten transcripts). With URLs instead of a page, about 2 credits per video. Say so before starting; pass `max_credits` if the user gave a budget.

### Transcript steps

1. **Pick the videos.** From a page: `facebook_get_posts` with `handle` or `url` (1 credit per page of results), paging with `meta.cursor` to about 50 posts. Keep the rows with `media: "video"` and rank them by `views`, or by `likes` plus `comments` where `views` is null. From URLs: `facebook_get_post` on each (1 credit) for the same numbers.
2. **Transcribe.** `facebook_get_transcript` with each video's `url` (1 credit, cached 30 days), and `language` where needed. `NoData` means the video carries no speech (music over captions, a silent demo): note it, it still cost 1 credit, and do not retry.
3. **Read each transcript.** Take the hook (the first sentence or two, what a scroller hears in the first seconds), the problem it names, the claims (numbers, promises, comparisons), the proof (a demo, a customer, data), the offer and the call to action.
4. **Compare.** Set the top half by views against the bottom half: the kind of hook (a question, a bold claim, a story, a demo), the transcript's length in words, the topic and the call to action. Name what the best videos share that the rest lack.
5. **Deliver** a table: video URL, date, views, likes, comments, hook (verbatim), main claim, proof, call to action, and why it worked or did not in one line. Below it, the patterns from step 4 in three to five lines, and the full transcripts of the top three if the user wants them.

### Transcript judgment

- Facebook rows carry no video length (`duration_s` is null); the transcript's word count is the nearest reading.
- A transcript is speech only. On-screen text, captions burned into the video and visuals are not in it, and many Facebook videos rely on them. Say so when a top video's transcript is thin.
- The hook decides most of a video's reach. Compare first lines before anything else.
- Facebook counts a video view after a few seconds of play, so a high view count with few comments can mean people scrolled past. Read `views` together with `comments`.
- Use a competitor's transcript for patterns and claims to answer, not for lines to copy.
- Turning the user's own transcripts into posts, threads or a blog is [repurpose-content](../repurpose-content/SKILL.md).
- A competitor's video ads are in the ad library, not on the page: the [research-meta-ads](../research-meta-ads/SKILL.md) skill.

## Related skills

- A plan for the user's own page: [create-facebook-plan](../create-facebook-plan/SKILL.md).
- The same audit on other platforms: [audit-instagram-account](../audit-instagram-account/SKILL.md) (the same brand's Instagram account), [audit-tiktok-account](../audit-tiktok-account/SKILL.md), [audit-youtube-channel](../audit-youtube-channel/SKILL.md), [audit-linkedin-page](../audit-linkedin-page/SKILL.md), [audit-x-account](../audit-x-account/SKILL.md). Run each and set the results side by side, comparing pages only within one platform.
- What people say under a page's posts: [mine-facebook-comments](../mine-facebook-comments/SKILL.md). Questions and pain points in public Facebook groups: [mine-facebook-groups](../mine-facebook-groups/SKILL.md).
- Everything about a competitor beyond Facebook (search, ads, messaging, pricing): [tear-down-competitor](../tear-down-competitor/SKILL.md). The Meta ads a rival runs: [research-meta-ads](../research-meta-ads/SKILL.md).
- Turning the user's own videos and posts into posts for other channels: [repurpose-content](../repurpose-content/SKILL.md).
