---
name: create-content-calendar
description: When the user wants a content calendar. Lays out four weeks of dated posts from an idea table, the channels and the team's hours, with pillars, one anchor piece a week and its cut-downs, the load per week and the evidence behind each slot, optionally checked against how often competitors post on LinkedIn, X, YouTube, TikTok or Instagram. Also use when the user mentions a content calendar, posting schedule, editorial calendar, plan our posts for October, what do we post each week, a posting cadence the team can keep, or turn these ideas into a schedule. Finding the ideas goes to find-content-ideas; turning one video or post into several to repurpose-content; a 90-day plan to grow one platform to create-tiktok-plan, create-instagram-plan, create-youtube-plan, create-linkedin-plan or create-facebook-plan. Posting and scheduling in a tool are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Content calendar

A calendar turns ideas into four weeks of dated posts the team can actually make. It takes an idea table, the channels and the team's hours, and lays out what goes where, when and by whom. It never schedules or posts: it hands back the calendar as a table, and the user, or their own scheduler, publishes.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` (which [find-content-ideas](../find-content-ideas/SKILL.md) needs when there is no idea table yet) and the listing tools of the channels, such as `linkedin_get_company_posts` or `youtube_get_videos` (hosts often add a prefix, for example `mcp__manifold__youtube_get_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If some tool groups are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, skip the cadence check for those channels, and mark them as not measured rather than weak.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the channels and accounts, the competitors' accounts, the brand voice, the goal) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Ideas**: the table from [find-content-ideas](../find-content-ideas/SKILL.md), or the user's own list. Without one, run find-content-ideas first and say it adds about 68 credits.
- **Channels**: one to three, from [pick-channels](../pick-channels/SKILL.md) or the user. More than three for a small team is the first thing to cut.
- **Capacity**: who makes content and how many hours a week. Default: one person, 4 hours a week, writing only.
- **Effort per format**: rough defaults to replace with the team's own times: a LinkedIn or X text post 45 minutes, a Reddit reply 20 minutes, a carousel 2 hours, a newsletter issue 2 hours, a short video 90 minutes, an article 5 hours, a YouTube video 8 hours.
- **Fixed dates**: a launch, an event, a holiday, days the team does not post. Default start: next Monday.
- **Budget**: about 8 credits for the optional cadence check (two competitors, up to four listings each, 1 credit a page). Reading `trend[12]` from the idea table costs nothing more. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Turn hours into slots.** Divide the weekly hours by the effort of each format on each channel. Plan to 80 percent of the hours: the rest absorbs slips and one reactive post.
2. **Check the cadence around the user (optional).** For one or two competitors per channel, one listing each (1 credit a page): `linkedin_get_company_posts`, `twitter_get_tweets`, `youtube_get_videos`, `tiktok_get_videos` or `instagram_get_posts`. Count posts per week from `created_at` over the page and note the days they post; LinkedIn dates are approximate, so count LinkedIn posts per month instead. It shows how often a channel's regulars post, which is a floor for being noticed, not a target to match.
3. **Set the pillars.** Group the ideas into two to four recurring themes (for example: how-to answers, opinions on the category, customer stories, product). Each week touches each pillar once where the slots allow.
4. **Place the anchors.** One anchor piece a week: the highest-ranked idea that fits the main channel. Its cut-downs go on the other channels in the days after it, mapped with [repurpose-content](../repurpose-content/SKILL.md). Ideas with a rising `trend[12]` or tied to a fixed date go in the first weeks.
5. **Fill and balance.** Fill the remaining slots by idea rank. Keep product posts to about one in four: people follow a channel for answers, and the other three earn the attention the fourth spends. Leave one open slot a week for news or a reply to something that takes off.
6. **Check the load.** Sum the effort per week. No week goes over the planned hours; if one does, move a slot or swap a format for a cheaper one, never add hours the team does not have.
7. **Deliver** a table: week, date, day, channel, format, idea (working title), pillar, evidence (the question and its number, from the idea table), repurposed from (the anchor, if any), owner, effort in hours, status (idea, drafting, ready, published). Under each week, one line: planned hours against capacity. Drafts only when the user asks; the host writes them from the evidence column.

## Judgment

- A calendar the team cannot keep is worse than a short one. Two posts a week for four weeks beat ten in the first week and none after.
- The tools cannot say when to post. `created_at` shows when accounts published, not when their audience was online. Do not name a best time; the user's own platform analytics have it.
- One anchor feeding four channels is cheaper than four separate ideas and keeps the message consistent. When capacity is under one anchor a week, drop to one channel.
- Keep the evidence column. Every slot names its evidence, or is marked as the user's call. When a post does well or badly, the team can see which question it answered and which source said people ask it.
- The host keeps the calendar (a doc, a sheet, its notes). The server schedules nothing and sends no reminders. If the host has a scheduling or social tool, offer to pass the slots to it; never post, schedule or publish.
- Never invent a number, a quote or a result the evidence does not hold in a draft.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. A result this account already paid for is free while cached: keywords and prompts for 7 days, listings for 6 hours, so a calendar built on this week's content ideas costs almost nothing.
- For the next calendar, run [find-content-ideas](../find-content-ideas/SKILL.md) again: results cached within 7 days are free. To review the past four weeks, list the user's own posts with the same listings as step 2 and compare each post with the channel's median; for X, [audit-x-account](../audit-x-account/SKILL.md) does this.

## Related skills

- The ideas the calendar places: [find-content-ideas](../find-content-ideas/SKILL.md). One anchor turned into cut-downs: [repurpose-content](../repurpose-content/SKILL.md).
- Which channels to be on at all: [pick-channels](../pick-channels/SKILL.md). A 90-day plan for one platform: [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-youtube-plan](../create-youtube-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md) or [create-facebook-plan](../create-facebook-plan/SKILL.md).
- Blog posts planned around keywords: [create-seo-content-plan](../create-seo-content-plan/SKILL.md).
- Content for a launch day: [create-launch-plan](../create-launch-plan/SKILL.md).
