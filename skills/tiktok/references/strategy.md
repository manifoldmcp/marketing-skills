# TikTok strategy

A TikTok strategy answers three questions before anyone films: what the account does now, what wins in its niche and for its competitors, and which of this group's playbooks closes the gap for the hours the team has. It ends in a 90-day plan, not a list of video ideas.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: awareness, followers, traffic and signups, or sales. Default: awareness, measured in median views per video.
- **Stage**: no account yet, a new account (under about 1,000 followers), or an established one. `tiktok_get_profile` in step 2 answers it if the user gives the handle.
- **ICP**: who buys, so the niche terms and the creators match what those people watch.
- **Niche terms**: two or three words people search for the category or the problem ("meal prep", "budgeting app"). Default: the category in the user's own words.
- **Budget**: credits for the research (this playbook costs about 31), hours a week for filming and editing, and any money for creators or ads. Default: 300 credits, 4 hours a week, no paid budget.
- **Team**: who can be on camera, who edits. Default: the founder on camera, filmed on a phone.
- **Competitors**: two or three TikTok handles. Default: the three brand accounts that appear most often in the niche search in step 3.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 16 credits for the user's account and three competitors. For each account, `tiktok_get_profile` (1 credit) for followers, `posts_count` and bio; `tiktok_get_videos` with `sort: "latest"`, two pages (1 credit a page), for the cadence (videos a week from `created_at`), median views, median engagement rate, median `duration_s`, the share of photo posts (`media: "image"`) and the paid posts (`is_ad`); and `tiktok_get_videos` with `sort: "popular"`, one page (1 credit), for the all-time top videos and their age. Read the numbers as the [router](../SKILL.md#reading-the-numbers) says. If the user has no account yet, baseline the competitors only. If they named no competitors, run the niche search from step 3 first and take the brand accounts from it.
3. **Gaps.** About 15 credits more.
   - The niche: `tiktok_search_videos` for each niche term with `since: "month"` and `sort: "popular"`, two pages each (6 credits). Note which accounts win (creators, brands, the user), which formats and lengths, and which topics recur. Topics the niche rewards that the user never posts about are the first gap.
   - Against competitors: cadence, median views, formats and topics side by side. A competitor's outliers (3 times its median) show what works for a brand like the user's.
   - How winners open: `tiktok_get_transcript` on the six top outliers across the niche and the competitors (1 credit each). Read the first spoken line of each.
   - What the audience asks: `tiktok_get_comments`, one page on each of the three most commented niche videos (1 credit each). Count the questions.
4. **Tactics.** Choose two or three from this group and say why each fits the numbers:
   - [Trends](trends.md) when several authors in the niche search repeat a format in the last month: the cheapest views while the account is small.
   - [Hooks](hooks.md) when the user's median views trail the competitors' on the same topics: the opening is usually what differs.
   - [Viral breakdown](viral-breakdown.md) when a competitor, or the user, has an outlier worth repeating.
   - [Competitor accounts](competitor-accounts.md) for the full audit when one competitor clearly beats the user on cadence or median views.
   - [Comment mining](comment-mining.md) when the niche's comments are full of questions: each question is a video to make.
   - [Find creators](find-creators.md) and an [audience check](audience-check.md) when nobody on the team can be on camera, or the goal needs reach faster than an account can grow.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week. By day 30, a cadence the team can hold (default three videos a week) testing three formats and three hook patterns from step 3. By day 60, keep the format with the best median views, drop the rest, and answer the top comment questions with videos. By day 90, a first creator partnership from the creator playbooks, or a paid test through the [paid-ads](../../paid-ads/SKILL.md) group if there is budget.
   - KPIs the tools can measure again later: followers and `posts_count` (`tiktok_get_profile`), median views, median engagement rate and the count of outliers over the last 20 videos (`tiktok_get_videos`), and whether the account appears in the top results for the niche terms (`tiktok_search_videos`).
   - **Deliver** one document: the inputs with defaults marked, a baseline table (the user against each competitor: followers, videos a week, median views, median engagement rate, median length, paid posts), the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and the three first actions.

## Judgment

- TikTok shows each video to people who do not follow the account, so a new account can win from its first video. Judge progress on median views, not followers.
- Set the cadence from the team's hours. One video a week the team can hold beats five rushed ones that stop in week three.
- A competitor's all-time top videos can be years old and made for a different TikTok. Weight the last 90 days.
- The tools cannot see the account's own analytics: watch time, completion rate, where views came from, profile visits and link clicks live in TikTok Studio. Ask the user to read them at each review.
- If the goal is signups or sales, the tools cannot see clicks from TikTok. The KPI for that is the user's own analytics on the bio link.
- The rows carry no sound data, and TikTok Shop and LIVE are outside the tools. Say so if the plan leans on them.
