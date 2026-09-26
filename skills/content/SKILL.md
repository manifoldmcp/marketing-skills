---
name: content
description: Cross-channel content with the manifold tools. Finds which channels to use from where the audience and demand are (Google, AI engine prompts, Reddit, YouTube, TikTok, LinkedIn) and where competitors post, turns real questions into content ideas with the channel and format each fits, lays out a four-week content calendar, repurposes one video or post into posts for other channels, and audits an X (Twitter) account against a competitor. Use when the user asks which channels to use, a channel mix, where their audience hangs out, whether to be on TikTok or LinkedIn, content ideas, what to post, content pillars, a content calendar, posting schedule or editorial calendar, repurposing a video, podcast or webinar, turning a YouTube video into LinkedIn posts or a thread, content atomization, or an X or Twitter audit. Blog posts and SEO articles belong to seo, one platform's content to that platform's group. The server writes no posts; posting and scheduling are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Content

Content here spans channels: where to show up, what to say there, when, and how one piece becomes several. Every job reads evidence (what people search, ask, discuss and watch) and ends in a table. The host writes the posts from that table when the user asks.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `youtube_search_videos` (hosts often add a prefix, for example `mcp__manifold__seo_search_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The playbooks cross tool groups: `seo_*`, `aeo_*`, `reddit_*` and the platform groups (`tiktok_*`, `youtube_*`, `linkedin_*`, `instagram_*`, `facebook_*`, `twitter_*`). If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run the steps the rest allow, and mark those channels as not measured rather than weak. The X account audit needs `twitter_get_tweets`; without it, stop and say so.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only says "content", open content ideas.

| Job | The user says | Open |
|---|---|---|
| Channels: which channels to use, ranked by audience, demand, competitor presence and effort | "which channels should we be on", "where does our audience hang out", "TikTok or LinkedIn for us", "what channel mix for a B2B startup", "where should we post" | [references/channels.md](references/channels.md) |
| Content ideas: ideas from real questions and demand, each with the channel and format it fits | "content ideas", "what should we post about", "topics our buyers care about", "content pillars", "we ran out of ideas" | [references/content-ideas.md](references/content-ideas.md) |
| Calendar: four weeks of dated posts from the ideas and the team's hours | "content calendar", "posting schedule for next month", "editorial calendar", "plan our posts for October", "what do we post each week" | [references/calendar.md](references/calendar.md) |
| Repurposing: one video or post turned into posts for other channels | "repurpose this video", "turn our podcast into LinkedIn posts", "make an X thread from this webinar", "reuse this LinkedIn post on other channels", "clips and posts from one YouTube video" | [references/repurposing.md](references/repurposing.md) |
| X account audit: one X account's cadence, topics and engagement against a competitor's | "audit our Twitter", "X account audit", "how is our X account doing", "compare our X to @competitor", "what works on our Twitter feed" | [references/x-account-audit.md](references/x-account-audit.md) |

## Shared rules

### Evidence

- Every row (a channel, an idea, a calendar slot, a derived post) names its evidence: the keyword and its `volume`, the prompt as asked, the thread URL and its comment count, the video URL and its views. A row with no evidence is marked as the user's call, not dropped silently.
- The sources count in different units, so each has its own floor:
  - Google: a keyword with `volume` of about 50 a month or more. Below that, even the first position brings a handful of visits; use the keyword as a phrasing, not as a topic to plan around.
  - AI engines: any prompt `aeo_search_prompts` returns, since the index holds only prompts it has seen answered. Rank by `ai_search_volume`, but never quote it as searches: it is a People Also Ask proxy.
  - Reddit: a thread with 10 or more comments in the last year, or a community with 3 or more posts in the `reddit_search_subreddits` sample.
  - Video and LinkedIn: compare views and likes only within one platform and one query. The median of the top 10 results is the bar a new post has to clear there.
- A question that shows up in two or more sources beats a bigger number in one. The overlap is the strongest signal these tools give.
- The tools see what people search, ask and watch, not what converts. Say so when the user asks which idea or channel will bring customers; their own analytics answer that.

### Platform numbers

- A number the platform does not publish is null, not zero. `created_at` on YouTube search and comment rows and on LinkedIn post rows is approximate ("3 weeks ago"), exact only to that unit.
- `linkedin_get_company_posts` rows carry no engagement; `linkedin_get_post` has likes and comments for one post (1 credit). LinkedIn exposes no comments.
- X has no search and no replies here. `twitter_get_tweets` returns one page of an account's recent posts with a null cursor, and its rows do not say whether a post carried an image or a video.
- Instagram search is by hashtag only. Facebook has no keyword search; group posts need the group URLs from the user.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Run the cheap calls first. Platform searches, listings, profiles, Reddit calls and transcripts cost 1 credit a page or a call. `seo_search_keywords` costs 10 for 100 rows. `aeo_search_prompts` is the costly one at 30 plus 30 per 100 rows (36 with `limit: 20`, 45 at the default 50): run it once per topic, never per idea.
- `tiktok_get_video` and `instagram_get_post` cost 10 when the vendor has to fetch the media. Search and listing rows already carry the numbers; call those two only for one post that needs it.
- A result this account already paid for is free while cached: keywords and prompts for 7 days, platform searches and listings for 6 hours, transcripts for 30 days. A calendar built on this week's content ideas costs almost nothing.

### Handoff

- The server does no content generation. Every playbook ends in a table of evidence. When the user asks for drafts, the host writes them from that table: the question in the audience's own words, the claims the transcript actually makes. Never invent a number, a quote or a result the evidence does not hold.
- Never post, schedule or publish. The host keeps the table (a doc, a sheet, its own notes). If it has a scheduling or social tool, offer to pass the calendar to it; do not post.
- The server keeps no state. A request to repeat a job every week, or to watch what competitors post, belongs to `monitoring`.

## Other groups

- Blog posts, SEO articles and a keyword-led content plan: [seo content plan](../seo/references/content-plan.md); one article's brief: [seo brief](../seo/references/brief.md). The content ideas here hand Google-bound ideas to those two.
- One platform's content: [tiktok](../tiktok/SKILL.md) (trends, hooks, viral breakdown), [instagram](../instagram/SKILL.md) (Reels trends, hooks), [youtube](../youtube/SKILL.md) (video ideas), [linkedin](../linkedin/SKILL.md) (post formats, founder and company strategy), [reddit](../reddit/SKILL.md) (threads to reply to), [facebook](../facebook/SKILL.md).
- One platform's competitor accounts: [tiktok competitor accounts](../tiktok/references/competitor-accounts.md), [instagram competitor accounts](../instagram/references/competitor-accounts.md), [youtube competitor channels](../youtube/references/competitor-channels.md), [linkedin company page audit](../linkedin/references/company-page-audit.md). The X audit lives here because X has no group.
- "Where do I start" or "more signups" with no channel in mind: [growth-plan](../growth-plan/SKILL.md). Paid channels: [paid-ads](../paid-ads/SKILL.md). Content for a launch day: [launch](../launch/SKILL.md).
- Pain points and personas behind the content: [customers](../customers/SKILL.md). Creators to make content with: [influencers](../influencers/SKILL.md).
- Watching mentions or competitors' posts every week: [monitoring](../monitoring/SKILL.md).
