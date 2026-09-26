# Reddit strategy

A Reddit strategy picks the few communities where buyers ask for help, decides what the user can bring to them, and chooses which of this group's playbooks to run each week. It ends in a 90-day plan, not a list of threads: the list comes from the playbooks the plan names.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: signups from Reddit, customer research, or being the answer people recommend (which also feeds Google and AI answers that show Reddit threads). Default: be recommended in the three communities where buyers ask.
- **Stage**: pre-launch, just launched or established. Before launch there is nothing to mention, so the plan is research and participation only.
- **ICP**: who buys, in the words they would use on Reddit, so the searches find them.
- **Budget**: credits for the research (this playbook costs about 33, or about 87 with the AI check in step 3) and hours a week on Reddit. Default: 500 credits and 3 hours a week.
- **Team**: who posts, from which account, and whether the founder can post as themselves. Account age and karma matter: many communities filter new accounts automatically. Default: the founder, from a personal account with some history.
- **Competitors**: two or three. Default: the ones Reddit names most in step 2.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 23 credits.
   - Where the topic lives: `reddit_search_subreddits` with three phrasings of the buyer's problem and `time_range: "year"` (1 credit each). Merge, then `reddit_get_subreddit` on the top 8 (1 credit each) for `subscribers`, `weekly_contributions` and the `rules` text.
   - How busy it is: `reddit_get_new_posts` on those 8 with `since: "7d"` and `match` set to the problem terms and competitor names (8 credits). The count per community is complete for the window if `coverage[]` says so; see [the router](../SKILL.md#search-and-watching).
   - Share of voice: `reddit_search_comments` with the user's brand and with each competitor, `time_range: "year"` (1 credit each, 4 for four brands). Note how often each is named and in what tone. It is a sample; compare the brands with each other, not with a total.
3. **Gaps.** About 10 credits more, or about 64 with the AI check.
   - Unanswered demand: from the step 2 check-in, the posts that ask for help or a tool and have fewer than 5 `comments`. That is where a helpful member stands out.
   - Why rivals win: `reddit_get_comments` on 5 threads where a competitor is recommended (1 credit each). Who recommends it (users, or the vendor's own staff) and for what reason.
   - Google and AI: `seo_get_serp` for 5 category keywords (1 credit each at the default depth). Rows with `type: "discussions_and_forums_element"` show Google puts Reddit threads on the page. If it does, optionally run `aeo_run_ai_answers` on 3 category prompts (18 credits each, 54 in all) and count citations on reddit.com.
   - Rules: the communities where the user's intended action (a product mention, a link, a launch post) is not allowed, from the rules read in step 2.
4. **Tactics.** Choose two to four from this group, and say why each fits the numbers:
   - [Subreddit rules](subreddit-rules.md) always, before the first post: one verdict per community and action.
   - [Threads to reply](threads-to-reply.md) as the weekly core when the check-in shows 5 or more relevant posts a week across the chosen communities.
   - [Find subreddits](find-subreddits.md) when the baseline found fewer than three communities above the [floors](../SKILL.md#floors), or the user wants a wider list.
   - [Pain points](pain-points.md) when the user is pre-launch or needs messaging and content ideas.
   - [Competitor complaints](competitor-complaints.md) when competitors are named often and "alternative to X" threads exist.
   - [AI-cited threads](ai-cited-threads.md) when Google or AI engines show Reddit threads for the category.
   - A daily or weekly watch of the chosen communities runs through the `monitoring` group's [brand mentions](../../monitoring/references/brand-mentions.md), which the host schedules.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - By day 30: rules read for three to five communities; account history built with plain helpful comments and no product mentions; a set number of replies a week from the threads-to-reply list, set by the team's hours.
   - By day 60: product mentions where a thread asks and the rules allow; one useful post per community where allowed (a guide, data, a lesson learned, never an ad); a competitor complaints pass.
   - By day 90: the AI-cited threads; an AMA or founder story where the moderators approve it; more of whatever earned the best comment scores.
   - KPIs the tools can measure again later: brand mentions in `reddit_search_comments` and `reddit_search_posts` with the same queries and `time_range: "month"` (a sample, so compare like with like); brand mentions in the chosen communities from `reddit_get_new_posts` with `match` on the brand (complete per window, if the host runs it weekly); the `score` of the user's own replies (`reddit_get_comments` on each thread replied in); and Reddit threads naming the user among AI citations (`aeo_run_ai_answers`) and in Google's discussions block (`seo_get_serp`). Signups from Reddit sit in the user's analytics, not in these tools: ask for UTM links on any link the user posts.
   - **Deliver** one document: the inputs with defaults marked, a baseline table (per community: subscribers, weekly contributions, relevant posts in 7 days, rules stance; per brand: mentions in the sample and tone), the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and the three first actions.

## Judgment

- Reddit punishes marketing that looks like marketing. The 30-day KPI is activity and comment score, never signups; the 90-day KPI is being named by other people.
- Set reply volume from the team's hours, not the thread count. Three hours a week is about ten careful replies; twenty rushed ones read as spam and trip the spam filters.
- Three to five communities, not twenty. Moderators and regulars notice a newcomer who shows up everywhere with the same message.
- A community whose rules ban any product mention still counts for research and plain helpful answers. Say so rather than drop it.
- If competitors are recommended mostly by their own staff, the space is open: a genuine user recommendation beats a vendor's.
- Paid Reddit ads are not covered here: no manifold tool reads Reddit's ad library.
