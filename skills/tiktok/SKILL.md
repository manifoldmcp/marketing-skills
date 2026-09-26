---
name: tiktok
description: TikTok research with the manifold tools. Reads TikTok search, accounts, videos, transcripts, comments, followers and audience country splits to plan a TikTok strategy, spot the formats and topics rising in a niche, pull the hooks of the top videos, break down why one video went viral, audit competitor accounts, find TikTok creators, check a creator's audience before paying, and mine comments for questions and objections. Use when the user asks about TikTok, TikTok marketing or growth, a TikTok content strategy, what is trending on TikTok, viral TikToks, TikTok hooks or opening lines, why a video blew up, a competitor's TikTok account, TikTok creators or TikTokers in a niche, vetting a TikTok creator, fake followers, which countries a creator's audience is in, or what people say in TikTok comments. Requests that span several platforms go to influencers, content or customers. The result is a table for the user; posting, commenting, messaging creators and scheduling are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# TikTok

Every job here reads what TikTok already shows: videos, their numbers, what is said in them and what people reply. The playbooks differ in which videos they pick and what they compare them with.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos` and `tiktok_get_profile` (hosts often add a prefix, for example `mcp__manifold__tiktok_get_profile`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `tiktok_*` tools are not, the tiktok group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: every playbook here needs it.
- Two steps reach outside TikTok: a creator's address at their own website (the `leads_*` tools, through the link-building contact steps) and a competitor's ads (the `ads_*` tools, in the paid-ads group). If those groups are off, skip those steps and say so.

## Menu

Pick one job from the request and open its playbook; if the request fits two jobs, ask one question, and if it gives a video URL and asks why it worked, open viral breakdown.

| Job | The user says | Open |
|---|---|---|
| TikTok strategy: where the account stands, what wins in the niche, and a 90-day plan | "TikTok strategy", "how do we grow on TikTok", "TikTok content plan for our brand", "plan our first 90 days on TikTok", "TikTok marketing plan" | [references/strategy.md](references/strategy.md) |
| Trends: the formats and topics rising in a niche this week or month | "what's trending on TikTok in skincare", "TikTok trends in our niche", "which formats are blowing up right now", "trending TikTok topics this week" | [references/trends.md](references/trends.md) |
| Hooks: how the niche's top videos open, as patterns with examples | "TikTok hooks that work", "how do the top videos open", "first lines of viral TikToks", "hook ideas for our TikToks" | [references/hooks.md](references/hooks.md) |
| Viral breakdown: why one video beat the account's usual numbers | "why did this TikTok go viral", "break down this video", "what made this blow up", "can we replicate this TikTok" | [references/viral-breakdown.md](references/viral-breakdown.md) |
| Competitor accounts: cadence, formats, top videos, engagement and paid posts of rival accounts | "what is our competitor doing on TikTok", "audit these TikTok accounts", "how often do they post on TikTok", "which of their TikToks work" | [references/competitor-accounts.md](references/competitor-accounts.md) |
| Find creators: TikTok creators in a niche, sized, active and engaged | "find TikTok creators for fitness", "TikTokers who make videos about budgeting", "micro creators on TikTok", "TikTok influencers in our niche" | [references/find-creators.md](references/find-creators.md) |
| Audience check: whether a TikTok creator's audience is real and in the right country | "is this TikToker's audience in the US", "check @handle before we pay", "fake followers on TikTok", "where are her TikTok followers" | [references/audience-check.md](references/audience-check.md) |
| Comment mining: questions, objections and wording from comments on the niche's top videos | "what are people asking in TikTok comments", "mine TikTok comments", "objections in the comments on TikTok", "how do people talk about this on TikTok" | [references/comment-mining.md](references/comment-mining.md) |

## Shared rules

### Reading the numbers

- **Engagement rate** is likes plus comments over views, from the row a search or a listing returns. Where `views` is null, use `followers` from `tiktok_get_profile` as the base and say so. Shares over views is a separate signal: above the account's median, the video spreads by people sending it.
- **Baseline.** Compare a video with its own account, never with TikTok at large: the account's median views and median engagement rate over its recent videos (`tiktok_get_videos` with `sort: "latest"`, paged until you have at least 20). Use the median, not the mean: one viral video drags a mean up for months.
- **Outlier.** A video with views at least 3 times the account's median is an outlier. TikTok views swing widely from one video to the next, so less than that is ordinary spread.
- **Paid posts.** `is_ad: true` marks ads and paid partnerships. Keep them out of the organic medians and report them in their own column: their views may have been bought.
- **Null is not zero.** A null number is one TikTok did not publish. Leave it out of the median.
- **Search is a sample.** `tiktok_search_videos` is ranked and never complete. Say "in the top results for X", never "on TikTok". In search, `sort: "popular"` ranks by likes (in `tiktok_get_videos` it ranks by views), and `since: "year"` reaches back about six months. Search has no country filter: read the language of the captions and transcripts.

### Floors

- **Active.** No video in the last 30 days means the account is inactive: drop it from creator lists and flag it in competitor lists.
- **Engagement.** A median engagement rate under about 3% of views means the audience watches but does not respond. Skip such creators unless they fit the niche exactly.
- **Views against followers.** Median views under about 5% of followers means most followers no longer watch: bought, inactive or grown on an old format. It is the first sign to check with the [audience check](references/audience-check.md).
- **Enough videos.** Judge an account on 10 or more videos. With fewer, report the numbers and say the median is weak.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Most calls cost 1 credit: a page of search, a page of an account's videos, a page of comments or followers, a profile, a transcript. Three cost more: `tiktok_get_audience` is 26, `tiktok_get_video` is estimated at 10 (1 when the vendor does not fetch the media), and `ai_fallback: true` on a transcript adds 10.
- Skip `tiktok_get_video` when a row from a search or a listing already has the video: the row carries the same numbers and caption. Call it only for a URL you have nothing else on.
- Transcripts go on the shortlist, and the audience split on the finalists only. A transcript that comes back `NoData` still costs 1 credit and means the video has no speech TikTok kept; use `ai_fallback: true` only on videos under 2 minutes that matter.
- A result this account already paid for is free while cached: 6 hours for searches, listings and comments, 24 hours for profiles, 7 days for followers and audience splits, 30 days for transcripts.

### Handoff

- Never post, comment, like, follow, message a creator or schedule anything. The deliverable is a table with a link to every video and account it cites.
- Write new hooks, scripts or captions only when the user asks, and base each on a pattern the table shows. Never pass off a creator's video or words as the user's.
- The server keeps no state. If the user wants the same check every week, the host schedules it and keeps the table: that is the [monitoring](../monitoring/SKILL.md) group.

## Other groups

- Creators across TikTok, Instagram and YouTube, or with no platform named: [influencers](../influencers/SKILL.md), whose [find creators](../influencers/references/find-creators.md) calls this group's playbook and whose [vet a creator](../influencers/references/vet-creator.md) uses the audience check. Campaign briefs for creators are there too.
- Reels and Instagram accounts: [instagram](../instagram/SKILL.md). Shorts and YouTube channels: [youtube](../youtube/SKILL.md).
- TikTok ads in the ad library, ad angles and creative briefs: [paid-ads](../paid-ads/SKILL.md), starting with [competitor ads](../paid-ads/references/competitor-ads.md).
- Turning a TikTok into posts for other channels, or a calendar across channels: [content](../content/SKILL.md), starting with [repurposing](../content/references/repurposing.md).
- Pain points across Reddit, reviews and several platforms: [customers](../customers/references/pain-points.md), which calls this group's comment mining.
- Everything about a competitor beyond TikTok: [competitors](../competitors/SKILL.md).
- Recurring checks ("every week", "alert me when a competitor posts"): [monitoring](../monitoring/SKILL.md).
- Creators for a launch day: [launch](../launch/references/creators.md). Work for an agency's client: [agency](../agency/SKILL.md).
