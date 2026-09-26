# Instagram strategy

An Instagram strategy answers three questions before anyone shoots: what the account does now, what wins in its niche and for its competitors, and which of this group's playbooks closes the gap for the hours the team has. It ends in a 90-day plan, not a list of post ideas.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: awareness, followers, traffic and signups, or sales. Default: awareness, measured in median reel views.
- **Stage**: no account yet, a new account (under about 1,000 followers), or an established one. `instagram_get_profile` in step 2 answers it if the user gives the handle.
- **ICP**: who buys, so the hashtags and the creators match what those people follow.
- **Niche hashtags**: two or three tags the niche actually uses ("mealprep", "homeoffice"). Default: the category tag, then the tags that recur in the captions of its first results.
- **Budget**: credits for the research (this playbook costs about 35), hours a week for shooting and editing, and any money for creators or ads. Default: 300 credits, 4 hours a week, no paid budget.
- **Team**: who can be on camera, who edits, who designs carousels. Default: the founder on camera, filmed on a phone, no designer.
- **Competitors**: two or three Instagram handles. Default: the three brand accounts that appear most often in the hashtag search in step 3.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 20 credits for the user's account and three competitors. For each account, `instagram_get_profile` (1 credit) for followers, `posts_count`, bio and whether it is a business account; `instagram_get_posts`, two pages (1 credit a page), for the overall cadence from `created_at`, the mix of reels and image posts (`media`), the median image-post engagement rate over followers, and the paid posts (`is_ad`); and `instagram_get_reels`, two pages (1 credit a page), for the reel cadence, median reel views, median reel engagement rate and median `duration_s`. Read the numbers as the [router](../SKILL.md#reading-the-numbers) says. If the user has no account yet, baseline the competitors only. If they named no competitors, run the hashtag search from step 3 first and take the brand accounts from it.
3. **Gaps.** About 15 credits more.
   - The niche: `instagram_search_posts` for each hashtag with `since: "month"`, two pages each (6 credits). Note which accounts win (creators, brands, the user), the share of reels, and which topics recur. Topics the niche rewards that the user never posts about are the first gap.
   - Against competitors: cadence, median reel views, format mix and topics side by side. A competitor's outlier reels (3 times its median views) show what works for a brand like the user's.
   - How winners open: `instagram_get_transcript` on the six top outlier reels across the niche and the competitors (1 credit each). Read the first spoken line of each, and the first line of each caption.
   - What the audience asks: `instagram_get_comments`, one page on each of the three most commented niche posts (1 credit each). Count the questions.
4. **Tactics.** Choose two or three from this group and say why each fits the numbers:
   - [Reels trends](reels-trends.md) when several accounts in the hashtag search repeat a reel format in the last month: the cheapest reach while the account is small.
   - [Hooks](hooks.md) when the user's median reel views trail the competitors' on the same topics: the opening is usually what differs.
   - [Competitor accounts](competitor-accounts.md) for the full audit when one competitor clearly beats the user on cadence or median reel views.
   - [Comment mining](comment-mining.md) when the niche's comments are full of questions: each question is a reel or a carousel to make.
   - [Find creators](find-creators.md) when nobody on the team can be on camera, or the goal needs reach faster than an account can grow.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week. By day 30, a cadence the team can hold (default three reels and one carousel a week) testing three reel formats and three hook patterns from step 3. By day 60, keep the reel format with the best median views, drop the rest, and answer the top comment questions with posts. By day 90, a first creator partnership from [find creators](find-creators.md), or a paid test through the [paid-ads](../../paid-ads/SKILL.md) group if there is budget.
   - KPIs the tools can measure again later: followers and `posts_count` (`instagram_get_profile`), median reel views, median reel engagement rate and the count of outlier reels over the last 20 reels (`instagram_get_reels`), the median image-post engagement rate (`instagram_get_posts`), and whether the account appears in the top results for the niche hashtags (`instagram_search_posts`).
   - **Deliver** one document: the inputs with defaults marked, a baseline table (the user against each competitor: followers, posts a week, reels a week, median reel views, median reel engagement rate, image-post engagement rate, paid posts), the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and the three first actions.

## Judgment

- Reels are what Instagram shows to people who do not follow the account; image posts and carousels mostly reach existing followers. A plan for growth leads with reels, and uses carousels to keep followers engaged.
- Set the cadence from the team's hours. Two reels a week the team can hold beat daily reels that stop in week three.
- Judge progress on median reel views and engagement, not followers: followers lag both by weeks.
- The tools cannot see saves, shares, reach, profile visits, link clicks or stories. They live in the account's Instagram Insights; ask the user to read them at each review.
- If the goal is signups or sales, the tools cannot see clicks from Instagram. The KPI for that is the user's own analytics on the bio link.
- The rows carry no audio data. If the plan leans on trending audio, the user checks it in the app.
