---
name: find-instagram-hooks
description: When the user wants to know how the best Instagram Reels and captions in a niche open. Pulls the first spoken line from the transcripts of the top reels and the first line of the captions of reels and image posts, keeps the reels that beat their author's own median, and sorts the hooks into patterns with real examples and links. Also use when the user mentions Reels hooks, Instagram hooks, Instagram caption hooks, how do the top reels open, first lines of viral reels, opening lines for Reels, hook ideas for our reels, or our Reels get skipped in the first second. TikTok hooks go to find-tiktok-hooks; LinkedIn hooks to find-linkedin-post-formats; what is trending on Reels to find-reels-trends; why one reel went viral to analyze-viral-reel; hooks from Instagram ads to research-meta-ads.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find Instagram hooks

How the niche's best reels open and how its best captions start: the first spoken line from the transcripts of the top reels, and the first line of each caption, sorted into patterns. It hands back a table of hook patterns, each with real examples and links, that the user can adapt.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `instagram_search_posts` and `instagram_get_transcript` (hosts often add a prefix, for example `mcp__manifold__instagram_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `instagram_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: this skill needs them.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the niche, the category and the problems it solves, the competitors' accounts, the brand voice) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Source**: niche hashtags (default: the category tag and two tags that recur in its captions), or a list of accounts the user wants to learn from (competitors, creators they admire).
- **Window**: `since: "month"` by default; `since: "year"` for a small niche.
- **Sample**: 20 reels by default, plus the captions of the top image posts from the same pull. Fewer than 12 reels is too few to see patterns.
- **Budget**: a default run costs about 3 x 2 + 10 + 20 = 36 credits for three hashtags. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the [Instagram notes](../create-instagram-plan/references/platforms/instagram.md)** before the first call. They say how to read Instagram's numbers and what each call costs.
2. **Collect the top posts.** From hashtags: `instagram_search_posts` for each tag with `since: "month"`, two pages each (1 credit a page). From accounts: `instagram_get_reels`, two pages each (1 credit a page), sorted by views yourself since the listing runs newest first, and `instagram_get_posts`, one page each (1 credit), for the image posts. Drop rows with `is_ad: true`.
3. **Keep the reels the opening lifted.** For up to 10 authors from the hashtag pull, `instagram_get_reels`, one page each (1 credit), for their median reel views. Keep the 20 reels with the highest multiple of their author's median, not the most views: a big account's ordinary reel says nothing about its hook. Drop reels whose engagement rate is under half their author's median (a rule of thumb). From an account source, the listing already gives the median. Also keep the 10 image posts with the most likes plus comments; their only readable hook is the caption.
4. **Read the opening.** `instagram_get_transcript` on each kept reel (1 credit). The first spoken sentence is the spoken hook; the first line of `text` is the caption hook, the part a viewer sees before the caption is cut. `NoData` means no speech: the hook is on screen, which the tools cannot read, so keep the caption line and mark it. A reel over two minutes comes back `InvalidTarget`: keep its caption line only.
5. **Sort into patterns.** Put each hook in one pattern: a question to the viewer, a bold or contrarian claim, a number or list ("5 mistakes..."), a callout of who it is for ("If you rent your flat..."), a story opening, the result first, a mistake or warning, a POV setup, a "save this for later" promise. Count the posts, and take the median views (reels) and the median multiple for each pattern.
6. **Deliver** a table: pattern, what it does in one line, posts using it, median views, median multiple of the author's median, two or three hooks quoted word for word each with the post link, author and views or likes, and whether the hook is spoken or in the caption. If the user asked, add three hooks for their own topic under each of the top three patterns.

## Judgment

- A hook is the first one to three seconds. Transcripts carry no timestamps, so the first sentence is the nearest reading; when it runs long, take its first clause.
- On Instagram, text on screen, the first frame and a carousel's first slide are invisible to the tools. If most top reels have no speech, say that the hooks in this niche are visual and the table shows only half of them.
- Image posts are ranked here by raw likes, which favours big accounts. Treat their caption hooks as weaker evidence than the reels'.
- Report patterns with three or more posts first. A pattern seen once is an anecdote.
- Adapt the pattern, never the words. Quoted hooks are evidence, not scripts to reuse, and never pass off a creator's words as the user's.
- For reels with transcripts in several languages, pass `language` to get the one the user needs.
- When the user also wants TikTok hooks, run [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md) and keep the tables apart: views compare only within one platform.
- Hooks from paid Instagram ads belong to [research-meta-ads](../research-meta-ads/SKILL.md).
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. A result this account already paid for is free while cached: searches and listings for 6 hours, transcripts for 30 days.
- **Handoff.** The deliverable is a table of evidence. Never post, comment or schedule. Write hooks for the user's own topic only when they ask, each on a pattern the table shows.

## Related skills

- What formats and topics are rising on Reels right now: [find-reels-trends](../find-reels-trends/SKILL.md). Why one reel beat its account's usual numbers: [analyze-viral-reel](../analyze-viral-reel/SKILL.md).
- Hooks on TikTok: [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md). LinkedIn hooks, structures and lengths: [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md). Ideas across channels: [find-content-ideas](../find-content-ideas/SKILL.md).
- One competitor's Instagram account and its top posts: [audit-instagram-account](../audit-instagram-account/SKILL.md). An Instagram plan: [create-instagram-plan](../create-instagram-plan/SKILL.md).
- Hooks from Instagram and Facebook ads: [research-meta-ads](../research-meta-ads/SKILL.md).
