---
name: audit-instagram-account
description: When the user wants to know what an Instagram account does and what works for it, their own or a competitor's. Reads each account's size, cadence, reels share, median reel views and engagement rates, transcribes its outlier reels, and checks its paid posts, paid partnerships and Instagram ads, side by side with rivals. Also use when the user mentions an Instagram or IG audit, a competitor's Instagram, how often a brand posts reels, Reels versus images, their best performing Instagram posts, benchmarking our IG against competitors, or Instagram engagement rate. TikTok goes to audit-tiktok-account, YouTube to audit-youtube-channel, Facebook to audit-facebook-page, LinkedIn to audit-linkedin-page, X to audit-x-account; a plan to grow on Instagram to create-instagram-plan; one viral reel to analyze-viral-reel; a rival beyond social to tear-down-competitor.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Audit an Instagram account

What rival brands do on Instagram and what works for them: how often they post, how much of it is reels, which posts beat their own median, how engaged their following is, and which posts are paid. Each account is judged against its own baseline and against the others. It ends in a side-by-side table, with a link to every post and account it cites, and a line per competitor on what the user should take from it.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `instagram_get_profile` (hosts often add a prefix, for example `mcp__manifold__instagram_get_profile`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `instagram_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop. Step 6 reads ads with the `ads_*` tools; without them, deliver the audit without the ads and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the user's own handle, the competitors and their accounts) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Competitors**: two to five Instagram handles. If the user names brands but not handles, ask, or read the `author` of the posts under the brand's own hashtag from `instagram_search_posts` (1 credit). If they do not know their competitors, [find-competitors](../find-competitors/SKILL.md) finds them first.
- **User's account**: the user's handle, to put their own numbers in the same table. Optional.
- **Window**: the last 90 days by default.
- **Budget**: a default run costs about 4 x (1 + 3 + 2 + 3 + 1) = 40 credits for three competitors and the user. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the platform notes** before the first call: [Instagram notes](../create-instagram-plan/references/platforms/instagram.md).
2. **Size.** `instagram_get_profile` on each handle (1 credit): followers, `posts_count`, bio, website, and `kind` (company for a business account). A handle that returns `NoData` is wrong; fix it before paying for the rest.
3. **Recent output.** `instagram_get_posts` on each account, paging until the window is covered (1 credit a page, about three pages for 90 days at a normal cadence). Order the rows by `created_at` yourself: pinned posts can sit first whatever their age. For each account: posts in the window, posts a week, the share of reels (`media: "video"`), the median image-post engagement rate over followers, and the days it posts (convert `created_at` from UTC to the audience's time zone). Read the numbers as the [Instagram notes](../create-instagram-plan/references/platforms/instagram.md#reading-the-numbers) says.
4. **Reels.** `instagram_get_reels`, two pages (1 credit a page): reels a week, median views, median reel engagement rate, median `duration_s`. Mark the outliers (3 times the account's median views or more). The listings run newest first, so rank the account's top posts yourself: reels by views, image posts by likes.
5. **What the top reels are.** `instagram_get_transcript` on the three strongest recent outlier reels per account (1 credit each), with the first line of each caption: the topic, the format, the hook and the call to action. Note any series or format the account repeats.
6. **Paid posts.** Count the rows with `is_ad: true`: ads and paid partnerships in the account's own feed, what they promote, and which creators appear in them. Then `ads_get_advertiser_ads` with `platform: "facebook"`, the brand's page name and `active_only: true` (1 credit), keeping rows whose `placements` include instagram. An ad whose `body` repeats an organic caption is a post the brand put money behind, the strongest sign it sells; an ad live for months (`first_shown`) is one that pays. A full teardown of the ads is the [research-meta-ads](../research-meta-ads/SKILL.md) skill's.
7. **Deliver** a table, one row per account: handle and link, followers, posts in the window, posts a week, reels a week, median reel views, median reel engagement rate, image-post engagement rate, paid posts, active ads and those that repeat an organic post, the top three recent posts (link, views or likes, multiple of the median, what each is in a few words). Below it, one line per competitor: what the user should take and what to avoid.

## Judgment

- **Own baseline.** Judge a post against its own account's median, never against Instagram at large; a rival against the user only over the same window.
- **Reel views, not followers.** Compare accounts on median reel views and engagement rates, not followers. Followers pile up over years; reel views show what Instagram gives the account now.
- **Paid apart.** Posts marked as ads or paid partnerships stay out of the organic medians and go in their own column: their reach may have been bought. An organic post that reappears in the brand's ads is a winner it put money behind.
- **Creators already paid.** Creators in a competitor's paid partnerships have already sold something like the user's product. They are a head start for [find-instagram-creators](../find-instagram-creators/SKILL.md).
- **Repeatable, not lucky.** The count of outliers in the window says more than the single biggest post: six mean a repeatable format, one means luck. Formats a brand keeps posting usually work for it; one it tried once and dropped probably did not. A rival whose cadence fell in the last month may have cut its effort: say so, it is an opening.
- **What is not there.** The tools see what is public. Saves, shares, reach, stories and follower growth over time are not there. For growth over time, the host re-runs this and keeps the table: the [monitor-competitors](../monitor-competitors/SKILL.md) skill.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Handoff.** The deliverable is a table with a link to every post and account it cites. Never post, comment, follow or message. A weekly watch of a rival's posts is the host's schedule, through [monitor-competitors](../monitor-competitors/SKILL.md).

## Related skills

- A plan for the user's own Instagram: [create-instagram-plan](../create-instagram-plan/SKILL.md).
- The hooks and Reels trends that win across the niche, not one account: [find-instagram-hooks](../find-instagram-hooks/SKILL.md), [find-reels-trends](../find-reels-trends/SKILL.md). Why one reel beat its account's usual numbers: [analyze-viral-reel](../analyze-viral-reel/SKILL.md).
- The same audit on other platforms: [audit-tiktok-account](../audit-tiktok-account/SKILL.md), [audit-youtube-channel](../audit-youtube-channel/SKILL.md), [audit-facebook-page](../audit-facebook-page/SKILL.md) (the same brand's Facebook page), [audit-linkedin-page](../audit-linkedin-page/SKILL.md), [audit-x-account](../audit-x-account/SKILL.md). Run each and set the results side by side, comparing accounts only within one platform.
- Everything about a competitor beyond Instagram (search, ads, messaging, pricing): [tear-down-competitor](../tear-down-competitor/SKILL.md). The Meta ads a rival runs: [research-meta-ads](../research-meta-ads/SKILL.md).
- What people say under a rival's posts: [mine-instagram-comments](../mine-instagram-comments/SKILL.md).
- Turning the user's own reels and posts into posts for other channels: [repurpose-content](../repurpose-content/SKILL.md).
