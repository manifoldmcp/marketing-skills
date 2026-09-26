---
name: launch
description: Product launch planning with the manifold tools. Builds a launch plan from a month before to a month after, finds the communities to launch in (subreddits and their self-promotion rules, Facebook groups, others the user names), the creators to seed, and the press list with its timing, then reports what people said after launch on Reddit, TikTok, YouTube and LinkedIn. Use when the user asks for a launch plan, launch strategy, launch checklist or timeline, launch day, a Product Hunt or Hacker News launch, Show HN, a beta, waitlist or feature launch, a relaunch, where to post or announce a launch, launch communities, creators or influencers to seed a launch, early access for creators, launch press, an embargo, press timing for a launch, a post-launch report, launch reactions, launch sentiment, or feedback since launch day. Product Hunt and Hacker News are planned for as venues, but no tool reads them. Posting, submitting, sending and scheduling stay with the user.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Launch

A launch is a date that concentrates attention. Every job here serves that date: where to show up, who else posts about it, who writes about it, and what people said afterwards. The platform and channel groups do the research; this group sets it on one timeline.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_posts` and `tiktok_search_videos` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The playbooks read Reddit, the social platforms (`tiktok_*`, `youtube_*`, `linkedin_*`, `instagram_*`, `facebook_*`), `seo_*` and, for press contacts, `leads_*`. If some of these tools are there and others are not, the missing tool groups are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, carry on with the rest, and list the platforms that went unchecked.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only says "launch" or asks for a plan, open the launch plan.

| Job | The user says | Open |
|---|---|---|
| Launch plan: before, launch day and after, from T-30 to T+30 | "launch plan", "plan our Product Hunt launch", "what do we do before and after launch day", "launch timeline", "we launch in three weeks, what's the plan" | [references/launch-plan.md](references/launch-plan.md) |
| Communities: where to launch, what each allows, and what to post where | "where should we post our launch", "which subreddits allow launch posts", "Facebook groups to announce in", "launch communities", "how do we launch without getting banned" | [references/communities.md](references/communities.md) |
| Creators: who to seed before launch day, and when | "creators to seed our launch", "influencers for launch day", "who gets early access", "send product to creators before launch", "launch influencer list" | [references/creators.md](references/creators.md) |
| Press: the launch press list, tiers and embargo timing | "launch press list", "who to pitch for our launch", "embargo timing", "press for launch day", "launch PR" | [references/press.md](references/press.md) |
| Reaction report: what people said after launch, with sentiment and quotes | "what are people saying since launch", "post-launch report", "launch reactions", "how did the launch land", "launch feedback so far" | [references/reaction-report.md](references/reaction-report.md) |

## Shared rules

### Timeline

- Every playbook dates its rows in days from launch day, so their outputs drop into one plan. The plan's milestones are T-30 (a month out), T-7, launch day, T+7 and T+30; a playbook adds a day between them where its work needs one (T-14 for a press exclusive, T-21 to ship product to creators).
- The launch date comes first. A playbook run without one asks for it; with fewer than 30 days left, it says which T-30 work no longer fits.

### Venues without tools

- Product Hunt, Hacker News, Indie Hackers, Slack and Discord communities and newsletters are common launch venues, and no manifold tool reads them. Name them in a plan with the user's own notes, and have the user read each venue's current rules and timing themselves.
- Never report votes, rankings, comments or any number from them. If the user pastes comments from one, the reaction report includes them, marked as pasted.

### Self-promotion

- Read a community's rules before planning a post there, and quote the line that allows or forbids it. Not stated is not permission.
- One post per community, written for its readers. The same text in ten places on one day reads as spam and gets accounts flagged.
- Stagger the posts so the founder can answer every comment in the first hours.
- The user posts from their own accounts. Nothing here posts, submits, votes or messages.

### Credits

- A launch plan costs about 30 credits; communities about 10 plus the Reddit and Facebook playbooks it runs (about 95 in all); creators about 20 plus find creators and the brief (about 190 in all); press about 3 plus the journalists run (about 370 in all); a reaction report about 50 per pass, run at T+1, T+7 and T+30.
- Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.

### Handoff

- The deliverable is a plan or a table. Posting, emailing, scheduling and submitting stay with the user or the host's own tools; offer to pass the table on.
- The server keeps no state. The host keeps the launch date, the plan and the launch post URLs for the reaction report, and after T+30 moves the watch to [brand mentions](../monitoring/references/brand-mentions.md).

## Other groups

- A go-to-market plan for a new product, of which the launch is one step: [go-to-market](../growth-plan/references/go-to-market.md).
- Subreddits, their rules and threads outside a launch: [reddit](../reddit/SKILL.md). Facebook groups outside a launch: [facebook](../facebook/SKILL.md).
- Creators for an ongoing program rather than a launch date: [influencers](../influencers/SKILL.md).
- A media list for a story that is not a launch: [journalists](../link-building/references/journalists.md), and a PR plan beyond launch week: [PR strategy](../link-building/references/pr-strategy.md).
- Mentions after the launch window, every day or week: [monitoring](../monitoring/SKILL.md).
- Launch ads: [paid-ads](../paid-ads/SKILL.md). The launch content calendar across channels: [content](../content/SKILL.md).
