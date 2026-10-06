---
name: audit-tiktok-account
description: When the user wants to know what a TikTok account does and what works for it, their own or a competitor's. Reads each account's size, recent and all-time top videos, cadence, formats, median views and engagement, transcribes its outlier videos, and checks its paid posts and TikTok ads, side by side with rivals. Also use when the user mentions a TikTok audit, what a competitor is doing on TikTok, how often a brand posts on TikTok, their best performing TikToks, benchmarking our TikTok against competitors, or TikTok engagement rate. Instagram goes to audit-instagram-account, YouTube to audit-youtube-channel, Facebook to audit-facebook-page, LinkedIn to audit-linkedin-page, X to audit-x-account; a plan to grow on TikTok to create-tiktok-plan; one viral TikTok to analyze-viral-tiktok; a rival beyond social to tear-down-competitor.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Audit a TikTok account

What rival brands do on TikTok and what works for them: how often they post, in which formats, which videos beat their own median, how engaged their audience is, and which posts are paid. Each account is judged against its own baseline and against the others. It ends in a side-by-side table, with a link to every video and account it cites, and a line per competitor on what the user should take from it.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_get_profile` (hosts often add a prefix, for example `mcp__manifold__tiktok_get_profile`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `tiktok_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop. Step 5 reads ads with the `ads_*` tools; without them, deliver the audit without the ads and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the user's own handle, the competitors and their accounts) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Competitors**: two to five TikTok handles. If the user names brands but not handles, find the handle in the author field of a `tiktok_search_videos` on the brand name (1 credit). If they do not know their competitors, [find-competitors](../find-competitors/SKILL.md) finds them first.
- **User's account**: the user's handle, to put their own numbers in the same table. Optional.
- **Window**: the last 90 days by default.
- **Budget**: a default run costs about 4 x (1 + 3 + 1 + 3 + 4) = 48 credits for three competitors and the user. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the platform notes** before the first call: [TikTok notes](../create-tiktok-plan/references/platforms/tiktok.md).
2. **Size.** `tiktok_get_profile` on each handle (1 credit): followers, `posts_count`, likes and the bio (with any link in it). A handle that returns `NoData` is wrong; fix it before paying for the rest.
3. **Recent output.** `tiktok_get_videos` with `sort: "latest"` on each account, paging until the window is covered (1 credit a page, usually three pages for 90 days). Order the rows by `created_at` yourself: a pinned video can sit first whatever its age. For each account: videos in the window, videos a week, median views, median engagement rate, median `duration_s`, the share of photo posts (`media: "image"`), and the days and hours it posts (convert `created_at` from UTC to the audience's time zone). Read the numbers as the [TikTok notes](../create-tiktok-plan/references/platforms/tiktok.md#reading-the-numbers) says.
4. **Top videos.** Mark the outliers in the window (3 times the account's median views or more). Then `tiktok_get_videos` with `sort: "popular"`, one page (1 credit), for the all-time top videos, and note how old they are.
5. **What the top videos are.** `tiktok_get_transcript` on the three strongest recent outliers per account (1 credit each), with the first line of each caption: the topic, the format, the hook and the call to action. Note any series or format the account repeats.
6. **Paid posts.** Count the rows with `is_ad: true`: ads and paid partnerships in the account's own feed, and what they promote. Then `ads_get_advertiser_ads` with `platform: "tiktok"` and the brand name (1 credit): the ads with a recent `last_shown` (`active` is null on TikTok). The rows carry no ad text, so `ads_get_ad` on the three newest (1 credit each) for the title; an ad whose title repeats an organic caption is a video the brand put money behind, the strongest sign it sells. A full teardown of the ads is the [research-tiktok-ads](../research-tiktok-ads/SKILL.md) skill's.
7. **Deliver** a table, one row per account: handle and link, followers, videos in the window, videos a week, median views, median engagement rate, median length, main formats, paid posts, live ads and those that repeat an organic video, the top three recent videos (link, views, multiple of the median, what each is in a few words). Below it, one line per competitor: what the user should take and what to avoid.

## Judgment

- **Own baseline.** Judge a video against its own account's median, never against TikTok at large; a rival against the user only over the same window.
- **Views, not followers.** Followers say little on TikTok, where most views come from people who do not follow. Compare accounts on median views.
- **Paid apart.** Videos marked as ads or paid partnerships stay out of the organic medians and go in their own column: their reach may have been bought. An organic video that reappears in the brand's ads is a winner it put money behind.
- **Repeatable, not lucky.** The count of outliers in the window says more than the single biggest video: six mean a repeatable format, one means luck. Formats a brand keeps posting usually work for it; one it tried once and dropped probably did not. A rival whose cadence fell in the last month may have cut its effort: say so, it is an opening.
- **What is not there.** The tools see what is public. Follower growth over time, ad spend and the account's own analytics are not there. For growth over time, the host re-runs this and keeps the table: the [monitor-competitors](../monitor-competitors/SKILL.md) skill.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Handoff.** The deliverable is a table with a link to every video and account it cites. Never post, comment, follow or message. A weekly watch of a rival's videos is the host's schedule, through [monitor-competitors](../monitor-competitors/SKILL.md).

## Related skills

- A plan for the user's own TikTok: [create-tiktok-plan](../create-tiktok-plan/SKILL.md).
- The hooks and trends that win across the niche, not one account: [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md), [find-tiktok-trends](../find-tiktok-trends/SKILL.md). Why one video beat its account's usual numbers: [analyze-viral-tiktok](../analyze-viral-tiktok/SKILL.md).
- The same audit on other platforms: [audit-instagram-account](../audit-instagram-account/SKILL.md), [audit-youtube-channel](../audit-youtube-channel/SKILL.md), [audit-facebook-page](../audit-facebook-page/SKILL.md), [audit-linkedin-page](../audit-linkedin-page/SKILL.md), [audit-x-account](../audit-x-account/SKILL.md). Run each and set the results side by side, comparing accounts only within one platform.
- Everything about a competitor beyond TikTok (search, ads, messaging, pricing): [tear-down-competitor](../tear-down-competitor/SKILL.md). The TikTok ads a rival runs: [research-tiktok-ads](../research-tiktok-ads/SKILL.md).
- What people say under a rival's videos: [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md).
- Turning the user's own videos into posts for other channels: [repurpose-content](../repurpose-content/SKILL.md).
