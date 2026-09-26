---
name: instagram
description: Instagram research with the manifold tools. Reads hashtag search, accounts, posts, reels, reel transcripts and comments to plan an Instagram strategy, spot the Reels formats and topics rising in a niche, pull the hooks of the top reels and captions, audit competitor accounts, find Instagram creators and influencers, and mine comments for questions and objections. Use when the user asks about Instagram or IG, Instagram marketing or growth, an Instagram content strategy, Reels trends, what is working on Reels, Reels hooks, opening lines or caption hooks, a competitor's Instagram account, how often a brand posts reels, Instagram engagement rate, Instagram influencers or creators in a niche, or what people ask in Instagram comments. Instagram search here is by hashtag only, and Instagram publishes no audience split. Requests that span several platforms go to influencers, content or customers. The result is a table for the user; posting, commenting, DMs and scheduling are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Instagram

Every job here reads what Instagram already shows: an account's posts and reels, their numbers, what is said in a reel, and what people reply. The playbooks differ in which posts they pick and what they compare them with.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `instagram_search_posts` and `instagram_get_profile` (hosts often add a prefix, for example `mcp__manifold__instagram_get_profile`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `instagram_*` tools are not, the instagram group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: every playbook here needs it.
- Two steps reach outside Instagram: a creator's address at their own website (the `leads_*` tools, through the link-building contact steps) and a competitor's ads in Meta's library (the `ads_*` tools, in the paid-ads group). If those groups are off, skip those steps and say so.

## Menu

Pick one job from the request and open its playbook; if the request fits two jobs, ask one question, and if it only asks what to post, open Reels trends.

| Job | The user says | Open |
|---|---|---|
| Instagram strategy: where the account stands, what wins in the niche, and a 90-day plan | "Instagram strategy", "how do we grow our Instagram", "Instagram content plan for our brand", "plan our next 90 days on IG", "Instagram marketing plan" | [references/strategy.md](references/strategy.md) |
| Reels trends: the formats and topics rising on Reels in a niche this week or month | "what's trending on Reels", "Reels trends in home decor", "which Reels formats are taking off", "trending Instagram topics this month" | [references/reels-trends.md](references/reels-trends.md) |
| Hooks: how the niche's top reels open and how the best captions start | "Reels hooks that work", "how do the top reels open", "first lines of viral reels", "Instagram caption hooks" | [references/hooks.md](references/hooks.md) |
| Competitor accounts: cadence, formats, top posts, engagement and paid posts of rival accounts | "what is our competitor doing on Instagram", "audit these Instagram accounts", "how often do they post reels", "which of their posts get engagement" | [references/competitor-accounts.md](references/competitor-accounts.md) |
| Find creators: Instagram creators in a niche, sized, active and engaged | "find Instagram influencers for skincare", "Instagram creators in our niche", "micro influencers on Instagram", "IG creators who post about meal prep" | [references/find-creators.md](references/find-creators.md) |
| Comment mining: questions, objections and wording from comments on the niche's top posts | "what are people asking in Instagram comments", "mine Instagram comments", "objections under our competitor's Instagram posts", "how do people talk about this on Instagram" | [references/comment-mining.md](references/comment-mining.md) |

## Shared rules

### Reading the numbers

- **Engagement rate.** For a reel, likes plus comments over views. For an image or carousel post, `views` is null, so use likes plus comments over `followers` from `instagram_get_profile`. Never mix the two bases in one median: report reels and image posts apart.
- **What is missing.** `shares` is always null on Instagram here, and saves, reach and stories are not in the tools at all. A null number is one Instagram did not publish, such as views on an image post: leave it out of the median.
- **Baseline.** Compare a post with its own account, never with Instagram at large: the account's median over its recent reels (`instagram_get_reels`) and, apart, its recent image posts (`instagram_get_posts`), paged until you have at least 20 of each kind it posts. Use the median, not the mean: one viral reel drags a mean up for months.
- **Outlier.** A reel with views at least 3 times the account's median, or an image post with likes at least 3 times its median, is an outlier. Less than that is ordinary spread.
- **Paid posts.** `is_ad: true` marks ads and paid partnerships. Keep them out of the organic medians and report them in their own column: their reach may have been bought.
- **Listings run newest first.** `instagram_get_posts` and `instagram_get_reels` have no popular sort. To find an account's top posts, page back through the window and sort the rows yourself. Order by `created_at`: a pinned post can come first whatever its age. `media` says only video or image; a carousel reads as an image post.
- **Search is by hashtag only.** `instagram_search_posts` takes one hashtag and `since`, with no sort and no free text, and its results are ranked by Instagram and never complete. Use the hashtags the niche actually uses (read them off the captions of the first results); a broad tag like #love is noise. Keep reels with `media: "video"` when the job is about Reels.
- **Transcripts** cover reels up to two minutes. A longer reel comes back `InvalidTarget`: skip it. `NoData` means no speech: the reel runs on music and text on screen.
- **No audience data.** Instagram publishes no audience split and the tools have no follower list. For a creator also on TikTok, the TikTok [audience check](../tiktok/references/audience-check.md) on that handle is the nearest proxy.

### Floors

- **Active.** No post in the last 30 days means the account is inactive: drop it from creator lists and flag it in competitor lists.
- **Engagement.** A median reel engagement rate under about 2% of views, or a median image-post engagement rate under about 1% of followers, means the following no longer responds. Skip such creators unless they fit the niche exactly.
- **Views against followers.** Median reel views under about 5% of followers means most followers no longer watch: bought, inactive or grown on an old format.
- **Enough posts.** Judge an account on 10 or more posts of a kind. With fewer, report the numbers and say the median is weak.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Most calls cost 1 credit: a page of hashtag search, a page of posts or reels, a page of comments, a profile, a transcript. Two cost more: `instagram_get_post` is estimated at 10 (1 when the vendor does not fetch the media), and `include_replies: true` on comments costs 15 a page, charged even when no reply comes back.
- Skip `instagram_get_post` when a row from a search or a listing already has the post: the row carries the same numbers and caption. Call it only for a URL you have nothing else on.
- Leave `include_replies` off unless a comment the job depends on shows many `replies`. Transcripts go on the shortlist only.
- A result this account already paid for is free while cached: 6 hours for searches, listings and comments, 24 hours for profiles, 30 days for transcripts.

### Handoff

- Never post, comment, like, follow, send a DM or schedule anything. The deliverable is a table with a link to every post and account it cites.
- Write new hooks, captions or scripts only when the user asks, and base each on a pattern the table shows. Never pass off a creator's post or words as the user's.
- The server keeps no state. If the user wants the same check every week, the host schedules it and keeps the table: that is the [monitoring](../monitoring/SKILL.md) group.

## Other groups

- Creators across Instagram, TikTok and YouTube, or with no platform named: [influencers](../influencers/SKILL.md), whose [find creators](../influencers/references/find-creators.md) calls this group's playbook and whose [vet a creator](../influencers/references/vet-creator.md) vets across platforms. Campaign briefs for creators are there too.
- TikTok, including the only audience split by country: [tiktok](../tiktok/SKILL.md). Shorts and YouTube channels: [youtube](../youtube/SKILL.md). Facebook pages and groups: [facebook](../facebook/SKILL.md).
- Instagram ads, which sit in Meta's ad library, and ad creative: [paid-ads](../paid-ads/SKILL.md), starting with [competitor ads](../paid-ads/references/competitor-ads.md).
- Turning a reel into posts for other channels, or a calendar across channels: [content](../content/SKILL.md), starting with [repurposing](../content/references/repurposing.md).
- Pain points across Reddit, reviews and several platforms: [customers](../customers/references/pain-points.md), which calls this group's comment mining.
- Everything about a competitor beyond Instagram: [competitors](../competitors/SKILL.md).
- Recurring checks ("every week", "alert me when a competitor posts"): [monitoring](../monitoring/SKILL.md).
- Creators for a launch day: [launch](../launch/references/creators.md). Work for an agency's client: [agency](../agency/SKILL.md).
