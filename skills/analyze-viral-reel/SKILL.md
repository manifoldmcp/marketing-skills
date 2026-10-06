---
name: analyze-viral-reel
description: When the user wants to know why one Instagram reel went viral. Sets the reel against its account's own baseline, reads its transcript, its most liked comments and its timing, checks whether it rode a hashtag trend, and ranks the likely causes with the evidence for each and what the user can repeat. Also use when the user mentions why did this reel blow up, why did this Instagram reel go viral, break down this reel, can we replicate this reel, or our reel got far more views than usual. One TikTok goes to analyze-viral-tiktok, one YouTube video to analyze-viral-youtube-video; the hooks across a niche's top reels to find-instagram-hooks; what is rising on Reels to find-reels-trends; a whole Instagram account to audit-instagram-account; reusing the reel on other channels to repurpose-content.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Analyze a viral reel

Why one Instagram reel did far better than its account usually does. It sets the reel against the account's own baseline, reads its transcript and comments, and checks its timing. It hands back a table of likely causes with the evidence for each, and what the user can repeat.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `instagram_get_reels` (hosts often add a prefix, for example `mcp__manifold__instagram_get_reels`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `instagram_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: this skill needs them.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the user's own handle, the audience's time zone) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Reel**: the Instagram reel URL (required). The account comes from it.
- **Question**: why it worked, how the user could repeat it, or both. Default: both.
- **User's account**: the user's handle, if they want the "repeat it" part fitted to their own numbers. Optional.
- **Budget**: a default run costs about 22 credits (12 when the reel is already in the account's listing). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **The baseline.** The account's size and its usual numbers, read as the [Instagram notes](../create-instagram-plan/references/platforms/instagram.md#reading-the-numbers) say: `instagram_get_profile` (1 credit) for followers; `instagram_get_reels`, two pages (1 credit a page), for the median views, engagement rate, comments over likes and `duration_s` (Instagram does not publish shares). There is no popular sort: count the account's outliers in those pages instead, to see whether this is its one hit or one of several.
2. **The reel.** Take its row from the listing if it is there; if not, `instagram_get_post` (estimated 10 credits, 1 when the vendor does not fetch the media). Work out its multiple of the median views, views over followers, and its engagement ratios against the account's medians. If `is_ad: true`, the reel is an ad or a paid partnership and its views may have been bought: say so first, and treat every factor below as weaker evidence.
3. **What was said.** `instagram_get_transcript` on the reel (1 credit; reels up to two minutes, a longer one is `InvalidTarget`). Read the hook (the first sentence), the structure (setup, turn, payoff, call to action), the topic and the length against the account's median. Then the same tool on three of the account's reels near its median (1 credit each) and say what differs. `NoData` means no speech: the hook is on screen; mark it.
4. **What people reacted to.** `instagram_get_comments`, three pages (1 credit a page; leave `include_replies` off). Sort by likes. Count friends tagged (people sharing it), questions, disagreement, requests for a part 2, and any line people quote back. A liked comment that quotes the reel names the line that landed.
5. **Timing.** From `created_at`, converted from UTC to the audience's time zone: the day and hour against the account's usual, the gap since its previous reel, and the reel's age (under 48 hours, the numbers are still moving). Then `instagram_search_posts` on the reel's main hashtag with `since: "month"`, two pages (1 credit a page). If many posts on the topic came before it, it rode a trend; if few did, it may have started one.
6. **Deliver** a table: factor (hook, topic, format, length, timing, trend, what drove comments), this reel, the account's usual, verdict (likely cause, possible, not a cause), and the evidence (a quote, a number, a link). Below it, three lines: what to repeat, how the user would do it on their account, and what cannot be copied.

## Judgment

- **One reel proves little.** One reel cannot prove a cause. Rank the factors by the strength of their evidence, and drop any factor the account's ordinary reels share: it is not what made this one different.
- **Beyond the followers.** Views above the follower count mean Instagram showed the reel far beyond the account's followers. The cause is in the reel, not the audience.
- **Comments over likes.** Comments over likes well above the median mean a debate: it divides people.
- **What is not there.** The biggest factors in distribution, watch time, completion or retention, and where the views came from, are not in the tools, and neither are Instagram sends and saves. Say so. For the user's own reel, Instagram Insights has them.
- **Sound.** The rows carry no sound data. If the reel rides a song or a sound, the user has to check it in the app.
- **No cause the data shows.** Some breakouts have no cause the data shows. When the evidence is thin, say so rather than invent one.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Call `instagram_get_post` only when the reel is not in the listing.
- **Handoff.** The deliverable is a table of evidence with a link to every reel it cites. Never post, comment or schedule. Write a script or hooks only when the user asks, from what the table shows, and never pass off the creator's reel or words as the user's.

## Related skills

- The same breakdown for one TikTok or one YouTube video: [analyze-viral-tiktok](../analyze-viral-tiktok/SKILL.md), [analyze-viral-youtube-video](../analyze-viral-youtube-video/SKILL.md).
- How the niche's top reels open: [find-instagram-hooks](../find-instagram-hooks/SKILL.md). What is rising on Reels: [find-reels-trends](../find-reels-trends/SKILL.md).
- A whole account's cadence and top posts: [audit-instagram-account](../audit-instagram-account/SKILL.md). Everything said under the reel: [mine-instagram-comments](../mine-instagram-comments/SKILL.md).
- Turning the user's own winner into posts for other channels: [repurpose-content](../repurpose-content/SKILL.md).
- A plan for the user's Instagram: [create-instagram-plan](../create-instagram-plan/SKILL.md).
