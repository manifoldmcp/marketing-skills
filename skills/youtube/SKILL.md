---
name: youtube
description: YouTube research with the manifold tools. Finds video ideas with proven views and search demand, breaks down competitor channels, finds video podcasts to guest on with a booking contact for each, mines the comments under videos, finds the YouTube videos ChatGPT, Perplexity and Google AI Overviews cite for the user's prompts, and finds YouTube creators in a niche sized by real views. Also plans a YouTube channel strategy. Use when the user asks for a YouTube strategy or channel plan, YouTube SEO, video ideas or video topics, what to make videos about, a competitor's YouTube channel or its best videos, podcasts to go on, podcast guesting or a podcast tour, what viewers say in YouTube comments, YouTube videos that AI answers or Google cite and how to get into them, YouTubers or YouTube influencers to sponsor, or YouTube creators for a review. The result is a table or a plan for the user; uploading, posting, commenting and sending pitches are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# YouTube

Every job here reads YouTube through the same few tools: search for what ranks, the channel for its size, the listing for what it publishes, one video for its likes and comments, the transcript for what is said. The playbooks differ in which videos they start from and what they judge.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `youtube_search_videos` and `youtube_get_channel` (hosts often add a prefix, for example `mcp__manifold__youtube_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `youtube_*` tools are not, the YouTube group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- Some playbooks also use other tool groups. Without `seo_*`, video ideas and the strategy lose Google demand: say so and rank on YouTube evidence alone. Without `aeo_*`, AI-cited videos can only read Google (`seo_get_serp`). Without `leads_*`, podcast guesting and creators deliver the contact the channel's own `bio` gives, unverified.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only says "YouTube" with no job, open the strategy.

| Job | The user says | Open |
|---|---|---|
| YouTube strategy: what the channel should publish, and a 90-day plan | "YouTube strategy", "should we start a YouTube channel", "grow our YouTube channel", "YouTube plan for next quarter", "B2B YouTube playbook" | [references/strategy.md](references/strategy.md) |
| Video ideas: topics with proven views and search demand | "video ideas for our niche", "what should we make videos about", "YouTube topics that get views", "YouTube keyword research", "what tutorials do people watch about X" | [references/video-ideas.md](references/video-ideas.md) |
| Competitor channels: what a rival's channel publishes and what works for it | "analyze a competitor's YouTube channel", "what videos does X post", "their best performing videos", "YouTube channel teardown", "compare our channel with theirs" | [references/competitor-channels.md](references/competitor-channels.md) |
| Podcasts to guest on: video podcasts on the topic, sized, with a booking contact | "podcasts I could guest on", "podcast guesting list", "get our founder on podcasts", "shows that interview SaaS founders", "podcast tour" | [references/podcast-guesting.md](references/podcast-guesting.md) |
| Comment mining: what viewers say under the videos that matter | "what do people say in the comments", "mine YouTube comments", "objections under review videos", "questions people ask under tutorials", "customer language from YouTube" | [references/comment-mining.md](references/comment-mining.md) |
| AI-cited videos: the YouTube videos AI engines and Google show for the user's prompts | "which YouTube videos does ChatGPT cite", "videos in Google's AI overview for our category", "YouTube videos Perplexity recommends", "get into the videos AI answers use", "which videos rank on Google for our keyword" | [references/ai-cited-videos.md](references/ai-cited-videos.md) |
| Find creators: YouTube channels in a niche, sized by real views | "find YouTubers to sponsor", "YouTube creators in our niche", "YouTube influencers for a product review", "channels that review tools like ours", "YouTube sponsorship list" | [references/find-creators.md](references/find-creators.md) |

## Shared rules

### Reading the data

- `youtube_search_videos` is YouTube's own ranked search: useful, never complete. `since` filters by upload date; `sort: "popular"` ranks by views. Leave `sort` at relevance otherwise, and use `since` for recency. It returns regular videos, not Shorts, so the tools cannot judge a Shorts strategy.
- A listing (search or `youtube_get_videos`) carries views and length (`duration_s`); likes and comments come only with `youtube_get_video`. Call that for the shortlist, not for every row.
- `created_at` on search rows and comments is approximate: YouTube shows an age ("3 weeks ago"), so a date is exact only to that unit. It is fine for "uploads in the last 90 days", not for a day-by-day timeline.
- Views on a row are lifetime views. To compare an old video with a new one, use views per month since upload.
- `youtube_get_channel`: `followers` is subscribers, `posts_count` is the video count, `views` is total views, `created_at` is when it joined. `website` is always null for a channel here; a site or a business address, when the channel gives one, is in the `bio`.
- The `author` of a row is the channel handle that `youtube_get_channel` and `youtube_get_videos` take. When it is null, or the lookup comes back `NoData`, skip the channel.
- There is no YouTube search volume. For demand, `seo_search_keywords` gives Google's monthly volume for the phrase, and a `video` in the `features` of `seo_get_serp` means Google shows videos for that query. Both are proxies for YouTube demand; say so every time a number comes from them.

### Channel health

- **Median, not mean.** Judge a channel by the median views of its latest 10 videos (`youtube_get_videos`, default sort). One viral video drags a mean far from what a new video will get.
- **Real audience.** A median under about 5% of subscribers means most subscribers no longer watch: size the channel by the median, not the subscriber count.
- **Active.** No upload in the last 90 days means the channel is paused. Skip it for sponsorships and guesting; keep it as evidence for topics.
- **Outlier.** A video with at least 3 times its channel's median views is an outlier: the topic or the title pulled viewers beyond the channel's own audience. Outliers are the best evidence of demand YouTube has.
- **Engagement.** Likes and comments per 1,000 views, from `youtube_get_video`. A video with high views and under about 5 likes per 1,000 views was often promoted with ads, whose viewers rarely like or comment. Mark it as likely, not proven.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Every `youtube_*` call is 1 credit, a page or a record. The expensive calls in this group are elsewhere: `aeo_run_ai_answers` (18 credits per prompt on the default five engines) and the contact steps (about 8 to 20 credits per domain).
- Transcripts are 1 credit but long. Read them for the shortlist only, to spare the context, not the credits.
- A result this account already paid for is free while cached: 6 hours for searches, listings, videos and comments, 24 hours for channels, 30 days for transcripts. A second pass the same day costs little.

### Handoff

- Never upload, post, comment, subscribe or send a pitch. The deliverable is a table or a plan. If the host has an email or sequencer tool, offer to pass the table to it; do not send.
- Write titles, scripts, comments or pitches only when the user asks, and then one per row, tied to the evidence in that row.
- The server keeps no state. If the user wants a channel or a topic watched every week, that is the `monitoring` group; the host schedules the run and keeps the previous results.

## Other groups

- Creators across TikTok, Instagram and YouTube at once, vetting one creator, UGC and a campaign brief: [influencers](../influencers/SKILL.md), which calls this group's find creators for YouTube.
- Turning a video into posts, threads or a newsletter, and content ideas across channels: [content](../content/SKILL.md), especially [repurposing](../content/references/repurposing.md).
- Pain points across Reddit, reviews and every platform's comments: [customers](../customers/references/pain-points.md), which uses this group's comment mining for YouTube.
- Everything about AI answers beyond YouTube (which prompts matter, every source type they cite): [ai-search](../ai-search/SKILL.md), starting from [visibility check](../ai-search/references/visibility-check.md) and [citation building](../ai-search/references/citation-building.md).
- A competitor's whole marketing (site, ads, pricing), not only its channel: [competitors](../competitors/SKILL.md).
- Blog content and Google rankings for pages: [seo](../seo/SKILL.md).
- Creators and press for a launch day: [launch](../launch/SKILL.md).
- Anything recurring ("every week", "alert me", "track"): [monitoring](../monitoring/SKILL.md).
