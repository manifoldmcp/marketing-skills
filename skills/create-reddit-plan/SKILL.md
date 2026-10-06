---
name: create-reddit-plan
description: When the user wants a plan for Reddit. Picks the few subreddits where buyers ask for help, measures the brand's and competitors' share of voice there, finds the gaps, chooses which Reddit skills to run each week and ends in a 30-60-90 day plan with KPIs. Also use when the user mentions a Reddit strategy, Reddit marketing plan, how do we grow on Reddit, should we be on Reddit, community-led growth on Reddit, or how the founder should show up on Reddit without getting banned. A ranked list of subreddits or their rules goes to find-subreddits, threads to answer this week to find-reddit-threads, what people complain about to find-reddit-pain-points, Reddit threads AI answers cite to build-ai-citations. The user posts from their own account; posting, replying, voting and messaging are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Reddit strategy

Communities reward people who help and ban people who advertise. A Reddit strategy picks the few communities where buyers ask for help, decides what the user can bring to them, and chooses which Reddit skills to run each week. It ends in a 90-day plan, not a list of threads: the lists come from the skills the plan names.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_posts` and `reddit_get_subreddit` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `reddit_*` tools are not, the Reddit tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- Step 3 also uses `seo_get_serp` and `aeo_run_ai_answers`. If the `aeo_*` tools are off, read Google alone and say AI engines were not checked; if the `seo_*` tools are off, AI answers alone.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the problem the product solves, the ICP, the competitors, the goal, customer language) from it; ask only for what it lacks. Use its customer language as the search phrasings and `match` terms. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first. After delivering, offer to write new customer language into `.agents/product-marketing.md` with [create-product-context](../create-product-context/SKILL.md).

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: signups from Reddit, customer research, or being the answer people recommend (which also feeds Google and AI answers that show Reddit threads). Default: be recommended in the three communities where buyers ask.
- **Stage**: pre-launch, just launched or established. Before launch there is nothing to mention, so the plan is research and participation only.
- **ICP**: who buys, in the words they would use on Reddit, so the searches find them.
- **Budget**: credits for the research (this skill costs about 33, or about 87 with the AI check in step 3) and hours a week on Reddit. Default: 500 credits and 3 hours a week.
- **Team**: who posts, from which account, and whether the founder can post as themselves. Account age and karma matter: many communities filter new accounts automatically. Default: the founder, from a personal account with some history.
- **Competitors**: two or three. Default: the ones Reddit names most in step 2.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 23 credits.
   - Where the topic lives: `reddit_search_subreddits` with three phrasings of the buyer's problem and `time_range: "year"` (1 credit each). Merge, then `reddit_get_subreddit` on the top 8 (1 credit each) for `subscribers`, `weekly_contributions` and the `rules` text.
   - How busy it is: `reddit_get_new_posts` on those 8 with `since: "7d"` and `match` set to the problem terms, the user's brand and the competitor names (8 credits, up to 10 terms). The count per community is complete for the window if `coverage[]` says so; see the [Reddit notes](references/platforms/reddit.md#search-and-watching). The brand and competitor matches are the share of voice inside the chosen communities, at no extra cost.
   - Share of voice across Reddit: `reddit_search_comments` with the user's brand and with each competitor and `time_range: "all"` (1 credit each, 4 for four brands), keeping the past year by `created_at`. Note how often each is named and in what tone. It is a sample; compare the brands with each other, not with a total.
3. **Gaps.** About 10 credits more, or about 64 with the AI check.
   - Unanswered demand: from the step 2 check-in, the posts that ask for help or a tool and have fewer than 5 `comments`. That is where a helpful member stands out.
   - Why rivals win: `reddit_get_comments` on 5 threads where a competitor is recommended (1 credit each). Who recommends it (users, or the vendor's own staff) and for what reason.
   - Google and AI: `seo_get_serp` for 5 category keywords (1 credit each at the default depth). Rows with `type: "discussions_and_forums_element"` show Google puts Reddit threads on the page. If it does, optionally run `aeo_run_ai_answers` on 3 category prompts (18 credits each, 54 in all) and count citations on reddit.com.
   - Rules: the communities where the user's intended action (a product mention, a link, a launch post) is not allowed, from the rules read in step 2.
4. **Tactics.** Choose two to four of these skills, and say why each fits the numbers:
   - The rules check in [find-subreddits](../find-subreddits/SKILL.md) always, before the first post: one verdict per community and action.
   - [find-reddit-threads](../find-reddit-threads/SKILL.md) as the weekly core when the check-in shows 5 or more relevant posts a week across the chosen communities.
   - [find-subreddits](../find-subreddits/SKILL.md) in full when the baseline found fewer than three communities above the [floors](references/platforms/reddit.md#floors), or the user wants a wider list.
   - [find-reddit-pain-points](../find-reddit-pain-points/SKILL.md), when the user is pre-launch or needs messaging and content ideas.
   - [find-competitor-complaints](../find-competitor-complaints/SKILL.md) when competitors are named often and "alternative to X" threads exist.
   - [build-ai-citations](../build-ai-citations/SKILL.md), for the Reddit threads AI answers cite, when Google or AI engines show Reddit threads for the category.
   - A daily or weekly watch of the chosen communities runs through [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md), which the host schedules.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - By day 30: rules read for three to five communities; account history built with plain helpful comments and no product mentions; a set number of replies a week from the find-reddit-threads list, set by the team's hours.
   - By day 60: product mentions where a thread asks and the rules allow; one useful post per community where allowed (a guide, data, a lesson learned, never an ad); a competitor complaints pass.
   - By day 90: the Reddit threads AI engines cite; an AMA or founder story where the moderators approve it; more of whatever earned the best comment scores.
   - KPIs the tools can measure again later: brand mentions in `reddit_search_comments` and `reddit_search_posts` with the same queries, counting the rows from the last month by `created_at` (a sample, so compare like with like); brand mentions in the chosen communities from `reddit_get_new_posts` with `match` on the brand (complete per window, if the host runs it weekly); the `score` of the user's own replies (`reddit_get_comments` on each thread replied in); and Reddit threads naming the user among AI citations (`aeo_run_ai_answers`) and in Google's discussions block (`seo_get_serp`). Signups from Reddit sit in the user's analytics, not in these tools: ask for UTM links on any link the user posts.
6. **Deliver** one document: the inputs with defaults marked, a baseline table (per community: subscribers, weekly contributions, relevant posts in 7 days, rules stance; per brand: mentions in the sample and tone), the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions.

## Judgment

- Every Reddit call follows the [Reddit notes](references/platforms/reddit.md): [search and watching](references/platforms/reddit.md#search-and-watching), the [floors](references/platforms/reddit.md#floors), the [evidence](references/platforms/reddit.md#evidence), the [credits](references/platforms/reddit.md#credits) and the [handoff](references/platforms/reddit.md#handoff). Never post, reply, vote or message for the user.
- Reddit punishes marketing that looks like marketing. The 30-day KPI is activity and comment score, never signups; the 90-day KPI is being named by other people.
- Set reply volume from the team's hours, not the thread count. Three hours a week is about ten careful replies; twenty rushed ones read as spam and trip the spam filters.
- Three to five communities, not twenty. Moderators and regulars notice a newcomer who shows up everywhere with the same message.
- A community whose rules ban any product mention still counts for research and plain helpful answers. Say so rather than drop it.
- If competitors are recommended mostly by their own staff, the space is open: a genuine user recommendation beats a vendor's.
- Paid Reddit ads are not covered here: no manifold tool reads Reddit's ad library.

## Related skills

- The lists this plan runs each week: [find-subreddits](../find-subreddits/SKILL.md) for communities and their rules, [find-reddit-threads](../find-reddit-threads/SKILL.md) for threads to answer.
- What people complain about on Reddit and elsewhere: [find-reddit-pain-points](../find-reddit-pain-points/SKILL.md) and [find-pain-points](../find-pain-points/SKILL.md). Complaints about named competitors: [find-competitor-complaints](../find-competitor-complaints/SKILL.md).
- Reddit threads that ChatGPT, Perplexity and Google show for the category: [build-ai-citations](../build-ai-citations/SKILL.md).
- The same daily engagement on LinkedIn: [find-linkedin-posts-to-comment](../find-linkedin-posts-to-comment/SKILL.md).
- Communities for a launch day: [create-launch-plan](../create-launch-plan/SKILL.md). Mentions of the brand on a schedule: [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
