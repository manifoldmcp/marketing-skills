---
name: facebook
description: Facebook research with the manifold tools. Audits a Facebook business page, the user's own or a competitor's (followers, posting cadence, which posts earn reactions and comments, the ads it runs), mines public Facebook groups the user names for questions, pain points and recommendations, turns a page's videos and reels into transcripts with their hooks, and mines the comments under posts for objections and customer language. Use when the user asks for a Facebook page audit, a competitor's Facebook page, what a brand posts on Facebook, Facebook engagement, Facebook group research or listening, what people ask in a Facebook group, a Facebook video or reel transcript, what a competitor says in its Facebook videos, Facebook comments, or voice of customer from Facebook. There is no post or group search, so group mining starts from group links. The result is a table; posting, commenting, joining groups and messaging are out of scope, and Facebook ad teardowns belong to paid-ads.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Facebook

Facebook data here starts from something the user points at: a page, a group or a post. There is no keyword search of posts and no search for groups, so every job opens with a page name or a link and reads what is there.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `facebook_get_posts` and `facebook_get_group_posts` (hosts often add a prefix, for example `mcp__manifold__facebook_get_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `facebook_*` tools are not, the Facebook group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- The page audit also reads ads with `ads_get_advertiser_ads`, and group mining can look for groups with `seo_get_serp`. If the `ads_*` tools are off, deliver the audit without the ads; if the `seo_*` tools are off, group mining needs the group links from the user.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it names a page and nothing else, open the page audit.

| Job | The user says | Open |
|---|---|---|
| Page audit: a page's size, cadence, what earns engagement, and the ads it runs | "audit our Facebook page", "review a competitor's Facebook page", "what does Gymshark post on Facebook", "is our Facebook page working", "benchmark our Facebook page against rivals" | [references/page-audit.md](references/page-audit.md) |
| Group mining: what members ask, complain about and recommend in groups the user names | "what are people asking in this Facebook group", "mine these Facebook groups", "Facebook group research", "what do members of this group complain about" | [references/group-mining.md](references/group-mining.md) |
| Video transcripts: what a page's videos and reels say, with the hook and the claims | "transcribe this Facebook video", "what does our competitor say in its Facebook reels", "get the scripts of their Facebook videos", "pull the hooks from these Facebook videos" | [references/video-transcripts.md](references/video-transcripts.md) |
| Comment mining: objections, questions and language in the comments under posts | "what do people say in the comments on our Facebook posts", "mine the comments on a competitor's Facebook page", "Facebook comment analysis", "objections in our Facebook comments" | [references/comment-mining.md](references/comment-mining.md) |

## Shared rules

### What the tools can read

- A page by `handle` or `url`: `facebook_get_profile` for the record, `facebook_get_posts` for its posts, newest first. A group by its `url`: `facebook_get_group_posts`, newest first. A post by its `url`: `facebook_get_post`, `facebook_get_comments` and, for a video or reel, `facebook_get_transcript`. Pages are businesses (`kind` is company); personal profiles are not covered.
- No keyword post search, no group search, and no comments across posts. Each list is one page per call with `meta.cursor` for the next; there is no complete window since a date, so page back until `created_at` passes the window and dedupe on `id`.
- Only public groups can be read. A private group returns nothing; the user can read it as a member, but the tools cannot.
- A number the platform does not publish is null, not zero. Facebook publishes no share count on page posts, so `shares` there is always null; group posts can carry one. `views` is often null on posts that are not videos.

### Engagement

- Engagement on a post is `likes` (reactions) plus `comments`, plus `shares` where published. Compare pages per 1,000 followers, not in raw counts, and use medians: one viral post drags a mean.
- Page posts reach few followers organically, and engagement well under 1% of followers per post is normal. Compare a page with its peers read the same way, not with a benchmark from elsewhere.
- Under about 8 posts in the window, a median says little. Say so rather than score it.

### Evidence

- Quote verbatim, with the post or comment link.
- Group members and commenters are private individuals. Leave their names out of the table, and never build a list of people from a group for outreach.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Every `facebook_*` call is 1 credit, per page of results for the lists, and `ads_get_advertiser_ads` is 1 credit per page. A `NoData` answer (a video with no speech, a private group) still costs 1; do not retry it.
- Caches: a page's record 24 hours, posts and comments 6 hours, transcripts 30 days, ads 24 hours. A result this account already paid for is free while cached.

### Handoff

- Never post, comment, react, join a group or send messages. The deliverable is a table.
- Draft a post, a reply or a comment only when the user asks, and then in the page's own voice, answering what the comment or post asked.
- The server keeps no state. To watch a page or a group every week, the host schedules the calls and keeps the ids already seen: the `monitoring` group's job.

## Other groups

- A competitor's Facebook and Instagram ads in depth (the Meta ad library, offers, creatives, how long each runs): the [paid-ads](../paid-ads/references/competitor-ads.md) group's competitor ads, and its [swipe file](../paid-ads/references/swipe-file.md).
- The same brand on Instagram: the [instagram](../instagram/SKILL.md) group.
- Pain points across Facebook, Reddit, reviews and other platforms' comments: [customers pain points](../customers/references/pain-points.md), which uses this group's comment mining.
- A competitor across every channel: the [competitors](../competitors/references/teardown.md) group's teardown.
- Posting a launch in Facebook groups, subreddits and other communities: [launch communities](../launch/references/communities.md), which uses group mining.
- Turning a video transcript into posts, threads or a blog: the [content](../content/references/repurposing.md) group's repurposing.
- A weekly watch on a page, a group or brand mentions: the [monitoring](../monitoring/SKILL.md) group.
- Subreddits rather than Facebook groups: the [reddit](../reddit/SKILL.md) group.
