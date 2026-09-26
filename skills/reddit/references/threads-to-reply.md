# Threads to reply to

Recent threads where a helpful reply from the user fits: someone asks for a tool in the category, describes the problem the product solves, or compares competitors. Each thread is read before it makes the list, so the list holds only threads that are open, not already answered well, and in communities that allow the reply. It ends in a table the user works through from their own account.

## Inputs to settle first

- **Topic terms**: the problem in the buyer's words, category words, and competitor names. Up to 10 terms for `match`.
- **Communities**: from [find subreddits](find-subreddits.md) or the user. Default: the top 5 there; if there are none, run its first step (a few credits).
- **Window**: default the last 7 days, the most `reddit_get_new_posts` reaches.
- **Product facts**: what the product does and does not do, so each angle is honest.
- **Budget**: a default run costs about 5 + 4 + 20 x 2 + 5 = 54 credits (a check-in over 5 communities, 4 searches, 20 threads read with their comments, the rules of 5 communities). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Collect from the communities.** `reddit_get_new_posts` on the chosen subreddits in one call, with `since: "7d"` and `match` set to the topic terms (1 credit per subreddit per page). This is complete for the window, so nothing recent in those communities is missed. Read `coverage[]` and act on a cut window as the [router](../SKILL.md#search-and-watching) says.
2. **Collect from the rest of Reddit.** `reddit_search_posts` with three or four question phrasings ("best <category> for", "alternative to <competitor>", "how do I <job>", "recommend a <category>"), `sort: "relevance"` and `time_range: "month"` (1 credit each), then keep only rows whose `created_at` is inside the window. Do not sort by `new` across all of Reddit: the search then drops the query, as the [router](../SKILL.md#search-and-watching) says. Merge with step 1 and dedupe on `id`.
3. **Cut from the rows.** Without more calls, drop posts outside the window, `over_18` posts, vendor posts (launches, "I built", promotions: they are not asking), and posts that match a term but not the intent (a job ad, a meme, a news link). Keep posts that ask, compare or describe the problem. Cut to the 20 best, freshest first.
4. **Judge each thread.** `reddit_get_post` (1 credit): drop it if `locked` or `archived` is true, or the body is "[removed]" or "[deleted]"; read the full `body` for what the poster needs. `reddit_get_comments` (1 credit, one page): is there already a good answer (a top-level comment, `depth: 0`, with a high `score` that names a fitting tool or solves the problem); which products are recommended; has the poster replied to anyone. A thread with under 10 comments and no good answer is the best target; one with 200 comments buries a new reply.
5. **Check the rules.** For each community on the list, the verdict from [Subreddit rules](subreddit-rules.md) (`reddit_get_subreddit`, 1 credit per community, free if read this week). Where product mentions are not allowed, keep the thread only as "answer without the product", or drop it.
6. **Deliver** a table: thread title, URL, subreddit, age in hours, comments, what the poster needs (one line), answered already (no, partly, yes), products already recommended, rules verdict, and the angle: what a helpful reply would say, in one line, using a detail from the post. Freshest open unanswered threads first. Write no drafts unless the user asks; then follow the drafting rules in the [router](../SKILL.md#handoff).

## Judgment

- Speed matters more than polish. The poster and the voters read replies in the first day or two; after a week only search visitors do, unless the thread ranks on Google or AI engines cite it ([AI-cited threads](ai-cited-threads.md)).
- A thread answered well is not a target, unless the top answer is outdated or wrong. Then the angle is the correction.
- If the angle would fit any thread, it is spam. Every angle must use something only this post said.
- Mention the product only where it answers the question. A plain helpful answer with no product builds the history that makes later mentions credible.
- "Tools like <competitor>" and "alternative to <competitor>" posts have the highest intent. Put them first when they are fresh.
- Spread replies out. Many similar comments from one account in one day look like spam to Reddit's filters and to moderators.
- One run is a snapshot. To get this list every day or week, the host schedules it through the `monitoring` group's [brand mentions](../../monitoring/references/brand-mentions.md).
