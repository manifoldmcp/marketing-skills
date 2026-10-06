---
name: create-tiktok-plan
description: When the user wants a plan to grow on TikTok. Writes a TikTok strategy from where the account stands, what wins in the niche and for competitors, and which tactics close the gap for the hours the team has, ending in a 30-60-90 day plan with KPIs the tools can measure again. Also use when the user mentions a TikTok strategy, how do we grow on TikTok, starting a TikTok account for a brand, a TikTok content plan, how often to post on TikTok, or a 90-day TikTok plan. A plan for Instagram goes to create-instagram-plan, YouTube to create-youtube-plan, LinkedIn to create-linkedin-plan, Facebook to create-facebook-plan; which platforms to be on at all to pick-channels; TikTok trends to find-tiktok-trends, hooks to find-tiktok-hooks, a competitor's TikTok account to audit-tiktok-account.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# TikTok plan

A TikTok plan answers three questions before anyone films: what the account does now, what wins in its niche and for its competitors, and which TikTok tactics close the gap for the hours the team has. It pulls in other skills as tactics (trends, hooks, competitor accounts, creators) and ends in a 30-60-90 day plan, not a list of video ideas.

This skill also holds the [TikTok notes](references/platforms/tiktok.md) that every skill reading TikTok follows.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos` and `tiktok_get_profile` (hosts often add a prefix, for example `mcp__manifold__tiktok_get_profile`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `tiktok_*` tools are not, the TikTok tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- The competitor ads check uses `ads_get_advertiser_ads`. If the `ads_*` tools are off, skip it and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the account handle, the ICP, the competitors and their accounts, the brand voice, the goal) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it. If the user has not chosen TikTok yet, or asks which platforms to be on, run [pick-channels](../pick-channels/SKILL.md) first.

- **Goal**: awareness, followers, traffic and signups, or sales. Default: awareness, measured in median views per video.
- **Stage**: no account yet, a new account (under about 1,000 followers), or an established one. `tiktok_get_profile` in the baseline answers it if the user gives the handle.
- **ICP**: who buys, so the niche terms and the creators match what those people watch.
- **Niche terms**: two or three words people search for the category or the problem ("meal prep", "budgeting app"). Default: the category in the user's own words.
- **Budget**: credits for the research (this plan costs about 40, or 66 with the audience check), hours a week for filming and editing, and any money for creators or ads. Default: 300 credits, 4 hours a week, no paid budget.
- **Team**: who can be on camera, who edits. Default: the founder on camera, filmed on a phone.
- **Competitors**: two or three TikTok handles. Default: the three brand accounts that appear most often in the niche search in the gaps step.
- **Horizon**: default 90 days.

## Steps

1. **Read the TikTok notes.** Before the first call, read the [TikTok notes](references/platforms/tiktok.md): how to read the numbers, the floors, what each call costs and the handoff. They hold for every skill this plan pulls in.
2. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
3. **Baseline.** Say the cost first: about 16 credits for the user's account and three competitors. For each account, `tiktok_get_profile` (1 credit) for followers, `posts_count` and bio; `tiktok_get_videos` with `sort: "latest"`, two pages (1 credit a page), for the cadence (videos a week from `created_at`), median views, median engagement rate, median `duration_s`, the share of photo posts (`media: "image"`) and the paid posts (`is_ad`); and `tiktok_get_videos` with `sort: "popular"`, one page (1 credit), for the all-time top videos and their age. Read the numbers as the [TikTok notes](references/platforms/tiktok.md#reading-the-numbers) say. If the user sells in one or a few countries, add `tiktok_get_audience` on their own account (26 credits): views from outside the market are reach that cannot buy. If the user has no account yet, baseline the competitors only. If they named no competitors, run the niche search from step 4 first and take the brand accounts from it.
4. **Gaps.** About 24 credits more.
   - The niche: `tiktok_search_videos` for each niche term with `since: "month"` and `sort: "popular"`, two pages each (6 credits). Note which accounts win (creators, brands, the user), which formats and lengths, and which topics recur. Topics the niche rewards that the user never posts about are the first gap.
   - Against competitors: cadence, median views, formats and topics side by side. A competitor's outliers (3 times its median) show what works for a brand like the user's. Then `ads_get_advertiser_ads` with `platform: "tiktok"` and the brand name (1 credit each): ads with a recent `last_shown` show what the competitor pays to push (`active` is null on TikTok). The full read of their ads is the [research-tiktok-ads](../research-tiktok-ads/SKILL.md) skill's.
   - How winners open: `tiktok_get_videos` with `sort: "latest"`, one page, on the six niche authors with the most views (1 credit each) for their medians; then `tiktok_get_transcript` on the six videos with the highest multiple of their author's median, the competitors' outliers included (1 credit each). Read the first spoken line of each.
   - What the audience asks: `tiktok_get_comments`, one page on each of the three most commented niche videos (1 credit each). Count the questions.
5. **Tactics.** From the gaps, choose two or three of these skills and say why each fits the numbers:
   - [find-tiktok-trends](../find-tiktok-trends/SKILL.md) when several authors in the niche search repeat a format in the last month: the cheapest views while the account is small.
   - [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md) when the user's median views trail the competitors' on the same topics: the opening is usually what differs.
   - [analyze-viral-tiktok](../analyze-viral-tiktok/SKILL.md) when a competitor, or the user, has an outlier worth repeating.
   - [audit-tiktok-account](../audit-tiktok-account/SKILL.md) for the full audit of competitor accounts when one competitor clearly beats the user on cadence or median views.
   - [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md) when the niche's comments are full of questions: each question is a video to make.
   - [find-tiktok-creators](../find-tiktok-creators/SKILL.md) and an audience check with [vet-creator](../vet-creator/SKILL.md) when nobody on the team can be on camera, or the goal needs reach faster than an account can grow.
6. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week. By day 30, a cadence the team can hold (default three videos a week) testing three formats and three hook patterns from step 4. By day 60, keep the format with the best median views, drop the rest, and answer the top comment questions with videos. By day 90, a first creator partnership from [find-tiktok-creators](../find-tiktok-creators/SKILL.md), or a paid test through the [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md) skill if there is budget.
   - KPIs the tools can measure again later: followers and `posts_count` (`tiktok_get_profile`), median views, median engagement rate and the count of outliers over the last 20 videos (`tiktok_get_videos`), and whether the account appears in the top results for the niche terms (`tiktok_search_videos`).
7. **Deliver** one document: the inputs with defaults marked, a baseline table (the user against each competitor: followers, videos a week, median views, median engagement rate, median length, paid posts, live ads), the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions.

## Judgment

- **TikTok notes first.** They hold for every skill the plan pulls in.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Plans, not posts.** The deliverable is a plan with a link to every account and video it cites. Never post, comment, follow, send a message or schedule anything. Write hooks or scripts only when the user asks, and base each on a pattern the evidence shows.
- **Measure again.** Every KPI in the plan is one the tools can read again later. A weekly check is the host's schedule, as in [monitor-competitors](../monitor-competitors/SKILL.md).
- TikTok shows each video to people who do not follow the account, so a new account can win from its first video. Judge progress on median views, not followers.
- Set the cadence from the team's hours. One video a week the team can hold beats five rushed ones that stop in week three.
- A competitor's all-time top videos can be years old and made for a different TikTok. Weight the last 90 days.
- The tools cannot see the account's own analytics: watch time, completion rate, where views came from, profile visits and link clicks live in TikTok Studio. Ask the user to read them at each review.
- If the goal is signups or sales, the tools cannot see clicks from TikTok. The KPI for that is the user's own analytics on the bio link.
- The rows carry no sound data, and TikTok Shop and LIVE are outside the tools. Say so if the plan leans on them.

## Related skills

- Which platforms to be on at all: [pick-channels](../pick-channels/SKILL.md). The whole growth plan: [create-growth-plan](../create-growth-plan/SKILL.md).
- The same plan on another platform: [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-youtube-plan](../create-youtube-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md), [create-facebook-plan](../create-facebook-plan/SKILL.md), [create-reddit-plan](../create-reddit-plan/SKILL.md).
- Trends, hooks and viral breakdowns on TikTok: [find-tiktok-trends](../find-tiktok-trends/SKILL.md), [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md), [analyze-viral-tiktok](../analyze-viral-tiktok/SKILL.md). A content calendar and repurposing: [create-content-calendar](../create-content-calendar/SKILL.md), [repurpose-content](../repurpose-content/SKILL.md).
- A competitor's TikTok account: [audit-tiktok-account](../audit-tiktok-account/SKILL.md). What people ask in TikTok comments: [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md).
- Creators to work with: [find-tiktok-creators](../find-tiktok-creators/SKILL.md), [find-creators](../find-creators/SKILL.md) across platforms, [create-influencer-plan](../create-influencer-plan/SKILL.md).
- Ads on TikTok: [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
