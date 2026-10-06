---
name: find-tiktok-hooks
description: When the user wants to know how the best TikToks in a niche open. Pulls the first spoken line from the transcripts of the top TikToks and the first line of their captions, keeps the videos that beat their author's own median, and sorts the hooks into patterns with real examples and links. Also use when the user mentions TikTok hooks, hooks that work on TikTok, how do the top TikToks open, first lines of viral TikToks, opening lines for TikTok, hook ideas for our TikToks, or our TikToks lose people in the first seconds. Reels and Instagram caption hooks go to find-instagram-hooks; LinkedIn hooks to find-linkedin-post-formats; what is trending on TikTok to find-tiktok-trends; why one TikTok went viral to analyze-viral-tiktok; hooks from TikTok ads to research-tiktok-ads.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find TikTok hooks

How the niche's best TikToks open: the first spoken line from their transcripts and the first line of their captions, sorted into patterns. It hands back a table of hook patterns, each with real examples and links, that the user can adapt.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos` and `tiktok_get_transcript` (hosts often add a prefix, for example `mcp__manifold__tiktok_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `tiktok_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: this skill needs them.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the niche, the category and the problems it solves, the competitors' accounts, the brand voice) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Source**: niche terms (default: the category and two problems it solves), or a list of accounts the user wants to learn from (competitors, creators they admire).
- **Window**: `since: "month"` by default; `since: "year"` for a small niche, which on TikTok reaches back about six months.
- **Sample**: 20 videos by default. Fewer than 12 is too few to see patterns.
- **Budget**: a default run costs about 3 x 2 + 10 + 20 = 36 credits for three terms. Each `ai_fallback` transcript adds 10. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the [TikTok notes](../create-tiktok-plan/references/platforms/tiktok.md)** before the first call. They say how to read TikTok's numbers and what each call costs.
2. **Collect the top videos.** From niche terms: `tiktok_search_videos` for each term with `since: "month"` and `sort: "popular"`, two pages each (1 credit a page). From accounts: `tiktok_get_videos` with `sort: "latest"`, two pages each (1 credit a page), which gives both the recent videos and the account's median; the popular sort reaches back years. Drop rows with `is_ad: true`.
3. **Keep the videos the opening lifted.** For up to 10 authors, `tiktok_get_videos` with `sort: "latest"`, one page each (1 credit), for their median views. Keep the 20 videos with the highest multiple of their author's median, not the most views: a big account's ordinary video says nothing about its hook. Drop videos whose engagement rate is under half their author's median (a rule of thumb): views the opening won but the video did not hold.
4. **Read the opening.** `tiktok_get_transcript` on each kept video (1 credit). The first spoken sentence is the spoken hook; the first line of `text` is the caption hook. `NoData` means no speech TikTok kept: the hook is on screen, which the tools cannot read, so keep the caption line and mark it. Use `ai_fallback: true` (11 credits) only on videos under 2 minutes that the table cannot do without.
5. **Sort into patterns.** Put each hook in one pattern: a question to the viewer, a bold or contrarian claim, a number or list ("3 things..."), a callout of who it is for ("If you run a Shopify store..."), a story opening ("I got fired and..."), the result first, a mistake or warning ("Stop doing..."), a POV or skit setup, a reply to a comment. Count the videos, and take the median views and the median multiple for each pattern.
6. **Deliver** a table: pattern, what it does in one line, videos using it, median views, median multiple of the author's median, two or three hooks quoted word for word each with the video link, author and views, and whether the hook is spoken or in the caption. If the user asked, add three hooks for their own topic under each of the top three patterns.

## Judgment

- A hook is the first one to three seconds. Transcripts carry no timestamps, so the first sentence is the nearest reading; when it runs long, take its first clause.
- On TikTok, text on screen, the first frame and the cut are invisible to the tools. If most top videos have no speech, say that the hooks in this niche are visual and the table shows only half of them.
- Report patterns with three or more videos first. A pattern seen once is an anecdote.
- Adapt the pattern, never the words. Quoted hooks are evidence, not scripts to reuse, and never pass off a creator's words as the user's.
- For videos with transcripts in several languages, pass `language` to get the one the user needs.
- When the user also wants Reels hooks, run [find-instagram-hooks](../find-instagram-hooks/SKILL.md) and keep the tables apart: views compare only within one platform.
- Hooks from paid TikTok ads belong to [research-tiktok-ads](../research-tiktok-ads/SKILL.md).
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. A result this account already paid for is free while cached: searches and listings for 6 hours, transcripts for 30 days.
- **Handoff.** The deliverable is a table of evidence. Never post, comment or schedule. Write hooks for the user's own topic only when they ask, each on a pattern the table shows.

## Related skills

- What formats and topics are rising on TikTok right now: [find-tiktok-trends](../find-tiktok-trends/SKILL.md). Why one TikTok beat its account's usual numbers: [analyze-viral-tiktok](../analyze-viral-tiktok/SKILL.md).
- Hooks on Reels: [find-instagram-hooks](../find-instagram-hooks/SKILL.md). LinkedIn hooks, structures and lengths: [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md). Ideas across channels: [find-content-ideas](../find-content-ideas/SKILL.md).
- One competitor's TikTok account and its top videos: [audit-tiktok-account](../audit-tiktok-account/SKILL.md). A TikTok plan: [create-tiktok-plan](../create-tiktok-plan/SKILL.md).
- Hooks from TikTok ads: [research-tiktok-ads](../research-tiktok-ads/SKILL.md).
