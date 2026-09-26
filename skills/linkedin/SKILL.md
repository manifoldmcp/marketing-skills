---
name: linkedin
description: LinkedIn research with the manifold tools. Finds who leads the conversation on a topic, which post formats, hooks and lengths get engagement in a niche, recent posts by leaders or buyers worth commenting on, and people posting about the problem the user solves, and audits a company page, the user's or a competitor's. Also plans a LinkedIn strategy for a founder's personal brand or a company page. Use when the user asks for a LinkedIn strategy, founder-led content, personal branding or thought leadership on LinkedIn, LinkedIn influencers or top voices in a niche, what to post on LinkedIn, post formats or hooks that work, how long a LinkedIn post should be, posts to comment on or a commenting routine, social selling, a LinkedIn company page audit, a competitor's LinkedIn page, or people on LinkedIn asking for a tool or complaining about a problem. The result is a table or a plan for the user; posting, commenting, liking, connecting and messaging on LinkedIn are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# LinkedIn

Every job here reads LinkedIn through posts: search for what is said on a topic, one post for its numbers, a company's own feed, and a profile or a company page for who is behind them. The playbooks differ in whose posts they look for and what they judge.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_search_posts` and `linkedin_get_post` (hosts often add a prefix, for example `mcp__manifold__linkedin_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `linkedin_*` tools are not, the LinkedIn group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- These playbooks use only the `linkedin_*` tools. Email addresses for the people they find come from the `leads` group, which needs the `leads_*` tools; without them, deliver names and profile links.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only says "LinkedIn" with no job, open the strategy.

| Job | The user says | Open |
|---|---|---|
| LinkedIn strategy: founder-led, company page, or both, with a 90-day plan | "LinkedIn strategy", "founder-led content plan", "grow my personal brand on LinkedIn", "what should our company page post", "LinkedIn plan for next quarter" | [references/strategy.md](references/strategy.md) |
| Topic leaders: who posts most and best on a topic | "top LinkedIn voices on X", "LinkedIn influencers in our niche", "who posts about X on LinkedIn", "thought leaders to follow and learn from", "who owns this topic on LinkedIn" | [references/topic-leaders.md](references/topic-leaders.md) |
| Post formats: which structures, hooks and lengths get engagement in the niche | "what kind of LinkedIn posts work in X", "how long should a LinkedIn post be", "hooks that work on LinkedIn", "analyze the top posts in our niche", "why do their posts get more engagement" | [references/post-formats.md](references/post-formats.md) |
| Posts to comment on: recent posts by leaders or buyers where a comment adds value | "posts I should comment on today", "commenting strategy", "where to engage on LinkedIn this week", "recent posts by our buyers", "social selling on LinkedIn" | [references/posts-to-comment.md](references/posts-to-comment.md) |
| Company page audit: the user's or a competitor's page and its posts | "audit our LinkedIn company page", "review a competitor's LinkedIn page", "how often does X post on LinkedIn", "what's working on their company page", "benchmark our page against competitors" | [references/company-page-audit.md](references/company-page-audit.md) |
| Problem posts: people posting about the problem the user solves | "people on LinkedIn complaining about X", "LinkedIn posts asking for a tool like ours", "who is looking for a X recommendation on LinkedIn", "people struggling with X on LinkedIn", "LinkedIn posts about switching from a competitor" | [references/problem-posts.md](references/problem-posts.md) |

## Shared rules

### What LinkedIn shows

- `linkedin_search_posts` is LinkedIn's own ranked search: never complete, 1 credit a page, filtered by `since` (day, week, month, year or all) and with no other sort. Its ranking tends to favour posts that already have engagement.
- A search row carries the author's name (`author_name`) but no handle. A post URL of the form `linkedin.com/posts/<handle>_...` starts with the author's handle, the part before the first underscore: pass it as `handle` to `linkedin_get_profile` (1 credit) for `followers`, `location` and the `bio`, or to `linkedin_get_company` when the author is a company. A URL without that form cannot be traced to a profile; keep the name only.
- Engagement is `likes` and `comments`. LinkedIn publishes no view count (`views` is null). Search rows usually carry both counts; when they are null, `linkedin_get_post` (1 credit) has them. `linkedin_get_company_posts` carries no engagement at all, so every post judged from it needs `linkedin_get_post`.
- The post type is not visible: every row reads `media: "text"`, so a carousel, an image, a video or a poll looks like a text post. The data judges the words (length, hook, structure); to see the media, the host opens the post URL if it has a browser, or the user checks the top posts by eye.
- `text` is cut at 2,000 characters, and `created_at` is approximate: LinkedIn shows an age ("3d", "2w"), exact only to that unit.
- No comments. The tools cannot read what people replied, or who liked or commented. Judge a conversation by its counts, and let the user read the thread.
- No feed per person. A person's own posts cannot be listed. Find them through search on the topics they post about and keep the rows under their name. For the user's own posts, ask for the URLs and read each with `linkedin_get_post`.
- `linkedin_get_company` carries no follower count (`followers` is null); compare pages on posting and engagement, not audience size.

### Thresholds

- **Sample.** Judge a format, a hook or a leader on at least 30 posts in total and 10 per group compared. A pattern in five posts is an anecdote.
- **Size matters.** Compare engagement per 1,000 followers when the authors differ in size. 200 likes from a 5,000-follower author is a stronger post than 2,000 from 500,000.
- **Median, not mean.** One viral post drags a mean; report medians.
- **Giveaways.** A post that asks readers to "comment X" for a guide gets comments by design. Count it apart, or it inflates every comment figure.
- **Fresh.** Most of a post's reach comes in its first days. For commenting, only posts from the last few days are worth it; for judging formats, a month or a year is fine.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Every `linkedin_*` call is 1 credit, a page or a record, so a run costs tens of credits. Profiles and single posts are where the count grows: fetch them for the shortlist only.
- A result this account already paid for is free while cached: 6 hours for searches, posts and company feeds, 24 hours for profiles and company pages.

### Handoff

- Never post, comment, like, connect or message. The deliverable is a table or a plan; the user writes and posts in their own voice.
- Draft a post or a comment only when the user asks, one per row, tied to the evidence in that row, and marked as a draft to edit.
- The server keeps no state. A daily commenting list or a weekly watch of a topic is the `monitoring` group's job; the host schedules the run and keeps the posts already seen.

## Other groups

- LinkedIn ads, a competitor's ads in LinkedIn's ad library: [paid-ads](../paid-ads/SKILL.md), especially [competitor ads](../paid-ads/references/competitor-ads.md).
- People by title at target companies, their emails, and buying signals across sources: [leads](../leads/SKILL.md), especially [buying intent](../leads/references/buying-intent.md), which uses this group's problem posts for LinkedIn.
- Content across channels, and turning one post into others: [content](../content/SKILL.md), especially [repurposing](../content/references/repurposing.md).
- A competitor's whole marketing, not only its page: [competitors](../competitors/references/teardown.md).
- Pain points across Reddit, reviews and platforms: [customers](../customers/references/pain-points.md).
- Creators to sponsor on TikTok, Instagram and YouTube: [influencers](../influencers/SKILL.md).
- Reporters found through their posts, for press: [link-building](../link-building/references/journalists.md).
- Anything recurring ("every morning", "alert me", "track"): [monitoring](../monitoring/SKILL.md), such as [brand mentions](../monitoring/references/brand-mentions.md).
