---
name: create-instagram-plan
description: When the user wants a plan to grow on Instagram. Writes an Instagram strategy from where the account stands, what wins in the niche hashtags and for competitors, and which tactics close the gap for the hours the team has, ending in a 30-60-90 day plan with KPIs the tools can measure again. Also use when the user mentions an Instagram strategy, an Instagram content plan, how do we grow on Instagram or IG, reels or carousels for our brand, how often to post reels, or a 90-day Instagram plan. A plan for TikTok goes to create-tiktok-plan, YouTube to create-youtube-plan, LinkedIn to create-linkedin-plan, Facebook to create-facebook-plan; which platforms to be on at all to pick-channels; Reels trends to find-reels-trends, hooks to find-instagram-hooks, a competitor's Instagram account to audit-instagram-account.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Instagram plan

An Instagram plan answers three questions before anyone shoots: what the account does now, what wins in its niche and for its competitors, and which Instagram tactics close the gap for the hours the team has. It pulls in other skills as tactics (Reels trends, hooks, competitor accounts, creators) and ends in a 30-60-90 day plan, not a list of post ideas.

This skill also holds the [Instagram notes](references/platforms/instagram.md) that every skill reading Instagram follows.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `instagram_search_posts` and `instagram_get_profile` (hosts often add a prefix, for example `mcp__manifold__instagram_get_profile`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `instagram_*` tools are not, the Instagram tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- The competitor ads check uses `ads_get_advertiser_ads`. If the `ads_*` tools are off, skip it and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the account handle, the ICP, the competitors and their accounts, the brand voice, the goal) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it. If the user has not chosen Instagram yet, or asks which platforms to be on, run [pick-channels](../pick-channels/SKILL.md) first.

- **Goal**: awareness, followers, traffic and signups, or sales. Default: awareness, measured in median reel views.
- **Stage**: no account yet, a new account (under about 1,000 followers), or an established one. `instagram_get_profile` in the baseline answers it if the user gives the handle.
- **ICP**: who buys, so the hashtags and the creators match what those people follow.
- **Niche hashtags**: two or three tags the niche actually uses ("mealprep", "homeoffice"). Default: the category tag, then the tags that recur in the captions of its first results.
- **Budget**: credits for the research (this plan costs about 44), hours a week for shooting and editing, and any money for creators or ads. Default: 300 credits, 4 hours a week, no paid budget.
- **Team**: who can be on camera, who edits, who designs carousels. Default: the founder on camera, filmed on a phone, no designer.
- **Competitors**: two or three Instagram handles. Default: the three brand accounts that appear most often in the hashtag search in the gaps step.
- **Horizon**: default 90 days.

## Steps

1. **Read the Instagram notes.** Before the first call, read the [Instagram notes](references/platforms/instagram.md): how to read the numbers, the floors, what each call costs and the handoff. They hold for every skill this plan pulls in.
2. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
3. **Baseline.** Say the cost first: about 20 credits for the user's account and three competitors. For each account, `instagram_get_profile` (1 credit) for followers, `posts_count`, bio and whether it is a business account; `instagram_get_posts`, two pages (1 credit a page), for the overall cadence from `created_at`, the mix of reels and image posts (`media`), the median image-post engagement rate over followers, and the paid posts (`is_ad`); and `instagram_get_reels`, two pages (1 credit a page), for the reel cadence, median reel views, median reel engagement rate and median `duration_s`. Read the numbers as the [Instagram notes](references/platforms/instagram.md#reading-the-numbers) say. If the user has no account yet, baseline the competitors only. If they named no competitors, run the hashtag search from step 4 first and take the brand accounts from it.
4. **Gaps.** About 24 credits more.
   - The niche: `instagram_search_posts` for each hashtag with `since: "month"`, two pages each (6 credits). Note which accounts win (creators, brands, the user), the share of reels, and which topics recur. Topics the niche rewards that the user never posts about are the first gap.
   - Against competitors: cadence, median reel views, format mix and topics side by side. A competitor's outlier reels (3 times its median views) show what works for a brand like the user's. Then `ads_get_advertiser_ads` with `platform: "facebook"`, the brand's page name and `active_only: true` (1 credit each), keeping rows whose `placements` include instagram: an ad whose `body` repeats an organic caption is a post the competitor put money behind, the strongest sign it sells. The full read of their ads is the [research-meta-ads](../research-meta-ads/SKILL.md) skill's.
   - How winners open: `instagram_get_reels`, one page, on the six niche authors with the most views (1 credit each) for their medians; then `instagram_get_transcript` on the six reels with the highest multiple of their author's median, the competitors' outliers included (1 credit each). Read the first spoken line of each, and the first line of each caption.
   - What the audience asks: `instagram_get_comments`, one page on each of the three most commented niche posts (1 credit each). Count the questions.
5. **Tactics.** From the gaps, choose two or three of these skills and say why each fits the numbers:
   - [find-reels-trends](../find-reels-trends/SKILL.md) when several accounts in the hashtag search repeat a reel format in the last month: the cheapest reach while the account is small.
   - [find-instagram-hooks](../find-instagram-hooks/SKILL.md) when the user's median reel views trail the competitors' on the same topics: the opening is usually what differs.
   - [analyze-viral-reel](../analyze-viral-reel/SKILL.md) when a competitor, or the user, has an outlier reel worth repeating.
   - [audit-instagram-account](../audit-instagram-account/SKILL.md) for the full audit of competitor accounts when one competitor clearly beats the user on cadence or median reel views.
   - [mine-instagram-comments](../mine-instagram-comments/SKILL.md) when the niche's comments are full of questions: each question is a reel or a carousel to make.
   - [find-instagram-creators](../find-instagram-creators/SKILL.md) when nobody on the team can be on camera, or the goal needs reach faster than an account can grow.
6. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week. By day 30, a cadence the team can hold (default three reels and one carousel a week) testing three reel formats and three hook patterns from step 4, each first as a Trial Reel (shown to non-followers only) so a weak test does not cost the grid. By day 60, keep the reel format with the best median views, drop the rest, and answer the top comment questions with posts. By day 90, a first creator partnership from [find-instagram-creators](../find-instagram-creators/SKILL.md), or a paid test through the [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md) skill if there is budget.
   - KPIs the tools can measure again later: followers and `posts_count` (`instagram_get_profile`), median reel views, median reel engagement rate and the count of outlier reels over the last 20 reels (`instagram_get_reels`), the median image-post engagement rate (`instagram_get_posts`), and whether the account appears in the top results for the niche hashtags (`instagram_search_posts`).
7. **Deliver** one document: the inputs with defaults marked, a baseline table (the user against each competitor: followers, posts a week, reels a week, median reel views, median reel engagement rate, image-post engagement rate, paid posts, active ads), the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions.

## Judgment

- **Instagram notes first.** They hold for every skill the plan pulls in.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Plans, not posts.** The deliverable is a plan with a link to every account and post it cites. Never post, comment, follow, send a DM or schedule anything. Write hooks, captions or scripts only when the user asks, and base each on a pattern the evidence shows.
- **Measure again.** Every KPI in the plan is one the tools can read again later. A weekly check is the host's schedule, as in [monitor-competitors](../monitor-competitors/SKILL.md).
- Reels reach the most people who do not follow the account, but Instagram recommends carousels too, and they earn the saves and sends it ranks on; for how-to and B2B topics a carousel often out-reaches a reel. Lead growth with reels and give the topics people save to carousels.
- Set the cadence from the team's hours. Two reels a week the team can hold beat daily reels that stop in week three.
- Judge progress on median reel views and engagement, not followers: followers lag both by weeks.
- The tools cannot see saves, sends, reach, profile visits, link clicks or stories. They live in the account's Instagram Insights; ask the user to read them at each review.
- If the goal is signups or sales, the tools cannot see clicks from Instagram. The KPI for that is the user's own analytics on the bio link.
- The rows carry no audio data. If the plan leans on trending audio, the user checks it in the app.

## Related skills

- Which platforms to be on at all: [pick-channels](../pick-channels/SKILL.md). The whole growth plan: [create-growth-plan](../create-growth-plan/SKILL.md).
- The same plan on another platform: [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-youtube-plan](../create-youtube-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md), [create-facebook-plan](../create-facebook-plan/SKILL.md) (the same buyers often use both), [create-reddit-plan](../create-reddit-plan/SKILL.md).
- Reels trends, hooks and viral breakdowns: [find-reels-trends](../find-reels-trends/SKILL.md), [find-instagram-hooks](../find-instagram-hooks/SKILL.md), [analyze-viral-reel](../analyze-viral-reel/SKILL.md). A content calendar and repurposing: [create-content-calendar](../create-content-calendar/SKILL.md), [repurpose-content](../repurpose-content/SKILL.md).
- A competitor's Instagram account: [audit-instagram-account](../audit-instagram-account/SKILL.md). What people ask in Instagram comments: [mine-instagram-comments](../mine-instagram-comments/SKILL.md).
- Creators to work with: [find-instagram-creators](../find-instagram-creators/SKILL.md), [find-creators](../find-creators/SKILL.md) across platforms, [create-influencer-plan](../create-influencer-plan/SKILL.md).
- Ads on Instagram: [research-meta-ads](../research-meta-ads/SKILL.md), [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
