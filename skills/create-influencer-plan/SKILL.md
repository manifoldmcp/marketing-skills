---
name: create-influencer-plan
description: When the user wants a plan for working with influencers and creators. Baselines where the niche's creators are on TikTok, Instagram and YouTube, which creators competitors pay and re-hire, what creator content competitors run as ads, and the brand's own unpaid footprint, then picks the deal types (gifting, paid posts, affiliate codes, UGC for ads) and ends in a 30-60-90 day plan. Also use when the user mentions an influencer marketing strategy, a creator program, how should we work with influencers, gifting or paid posts or affiliates, or an influencer plan for next quarter. A list of creators goes to find-creators, vetting one creator to vet-creator, the brief to write-creator-brief, and creators timed to a launch to create-launch-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Influencer strategy

An influencer strategy settles where the niche's creators are, what competitors already get from creators, and which kind of deal fits the goal and the budget: gifting, paid posts, affiliate codes or content for ads. It ends in a 90-day plan the creator skills can execute, not a creator list.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos`, `instagram_search_posts` and `youtube_search_videos` (hosts often add a prefix, for example `mcp__manifold__tiktok_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If one platform's tools are missing (all `tiktok_*`, `instagram_*` or `youtube_*`), that group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and plan on the platforms that are on. Without the `ads_*` tools, skip the competitor ads step and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the goal, the stage, the ICP and what they watch, the niche, the competitors, the countries that matter and the budget) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: awareness, sales through codes and links, content to run as ads, or buzz for a moment. Default: sales through codes, because the user can measure them.
- **Stage**: pre-launch, early or established, and whether the user has worked with creators before. Default: early, no creators yet.
- **ICP**: who buys and what they watch. It picks the platforms.
- **Budget**: cash for creator fees and product for gifting, and credits for the research (this skill costs about 40). Default: $3,000 and 30 units of product for the quarter, 500 credits.
- **Team**: who manages creators, who approves drafts, and whether product can be shipped. Default: the founder, a few hours a week.
- **Competitors**: two or three. Default: the brands that show up in the niche's sponsored posts in step 2.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 34 credits.
   - Where the niche lives: `tiktok_search_videos` and `youtube_search_videos` with the niche keyword (`since: "year"` on YouTube), and `instagram_search_posts` with the niche hashtag, two pages each (6 credits). Per platform, note how many distinct creators post and the median views of their posts.
   - What competitors get from creators: the same three searches with each competitor's name, one page each (9 credits for three). Keep the rows with `is_ad: true` or a sponsored caption: those are the creators competitors pay.
   - What competitors amplify: `ads_get_advertiser_ads` on each competitor with `platform: "facebook"` and `active_only: true`, and with `platform: "tiktok"` (1 credit a page; 6 for three). An ad whose creative is a creator's video, or whose `body` names one, is creator content the competitor pays to run as a partnership ad or a Spark Ad. Only Facebook reports `active`; on TikTok read `last_shown`.
   - The user's own footprint: the same three searches with the brand name (3 credits). Creators already posting about the brand unpaid are the brand fans list in [find-creators](../find-creators/SKILL.md).
   - Size: `tiktok_get_profile`, `instagram_get_profile` or `youtube_get_channel` on the ten authors who appear most (10 credits), for the size bands the niche runs on.
3. **Gaps.** About 5 credits more. `tiktok_get_videos`, `instagram_get_posts` or `youtube_get_videos` on five creators competitors paid (1 credit each).
   - Re-hires: a competitor paying the same creator twice is the best public sign a deal paid off. A creator's video still running as an ad a month after its `first_shown` is the other. Name those creators and the size band they sit in.
   - Deal types: discount codes in captions mean affiliate deals, "gifted" means seeding, `is_ad` without a code means a flat fee, and a creator's video in the ad library means the competitor also bought usage rights.
   - Platforms: the platform where the niche has many creators and competitors have few is the opening; the one where every competitor is present is table stakes.
4. **Tactics.** Choose and say why each fits:
   - [find-creators](../find-creators/SKILL.md), always: the shortlist across platforms, sized to the budget. Its brand fans search when step 2 found people posting about the brand unpaid: the cheapest first gifting list. Its UGC search when the goal is content to run as ads or to use on the product page, not reach.
   - [vet-creator](../vet-creator/SKILL.md) before any flat fee above a few hundred dollars. Skip it for product-only gifting to nano creators, where the check would cost more than the risk.
   - [write-creator-brief](../write-creator-brief/SKILL.md) before the first creator posts, always.
   - For creators timed to a launch day, [create-launch-plan](../create-launch-plan/SKILL.md).
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - The shape: by day 30, a shortlist of 30, the top 10 vetted, product sent to 15 to 20 nano and micro creators, and the brief written. By day 60, three to five paid posts from vetted micro creators, each with its own code. By day 90, re-hire the two creators whose posts beat their own median, turn the best posts into ads with [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md), and drop the rest.
   - KPIs the tools can measure again later: views, likes and comments on each campaign post (`tiktok_get_video`, `instagram_get_post`, `youtube_get_video`), each post's views against the creator's own median, questions about the product in its comments (`tiktok_get_comments`, `instagram_get_comments`, `youtube_get_comments`), and the count of posts naming the brand in the platform searches from step 2. Code redemptions and sales live in the user's store; ask for them at each checkpoint.
6. **Deliver** one document: the inputs with defaults marked, a baseline table (platform against creators in the niche, median views, competitors present, the user's footprint), the creators competitors pay and re-hire, the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and three first actions for this week.

## Judgment

- Twenty gifted nano creators teach more in the first month than one paid macro creator, for the same money. Pay only after the gifting shows which creators and angles move people.
- A code per creator is the only clean attribution. Without codes or tagged links, the user cannot tell which creator sold.
- Creators whose sponsored posts do as well as their own posts are the ones to re-hire; the audience trusts them. Sponsored posts at half the creator's median are a warning, whatever the follower count.
- Competitors' creators are proven for the niche, but a creator mid-contract with a competitor may be bound by exclusivity. Flag them; do not exclude them.
- The tools see public posts and numbers. They do not see fees, contracts or sales, and see a post's paid boost only when it shows in an ad library. Keep the KPIs to what can be measured again.
- Size bands: nano under 10K followers, micro 10K to 100K, mid 100K to 500K, macro 500K to 1M, mega above. Engagement per follower falls as accounts grow while fees rise with followers.
- Paid and gifted posts must be disclosed: the platform's paid partnership label and a plain "ad" the viewer cannot miss. That duty sits with the user and the creator; [write-creator-brief](../write-creator-brief/SKILL.md) spells it out.
- Credits: if the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Profiles are cached 24 hours, listings and searches 6 hours.
- Never follow, message, email, contract or pay a creator, and never ship product. The server keeps no state: the host keeps the baseline for the checkpoints, and [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md) can rerun the brand searches every week.

## Related skills

- The tactics in step 4: [find-creators](../find-creators/SKILL.md), [vet-creator](../vet-creator/SKILL.md) and [write-creator-brief](../write-creator-brief/SKILL.md).
- Creators timed with a launch: [create-launch-plan](../create-launch-plan/SKILL.md).
- Creator content run as ads: [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md) and [write-ad-brief](../write-ad-brief/SKILL.md).
- The user's own posts: [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md) or [create-youtube-plan](../create-youtube-plan/SKILL.md). Hooks and trends: [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md), [find-instagram-hooks](../find-instagram-hooks/SKILL.md), [find-tiktok-trends](../find-tiktok-trends/SKILL.md) and [find-reels-trends](../find-reels-trends/SKILL.md).
- A plan by goal across every channel: [create-growth-plan](../create-growth-plan/SKILL.md).
