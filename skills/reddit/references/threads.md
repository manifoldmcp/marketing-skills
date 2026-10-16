# Threads and replies

Recent threads where a helpful reply from the user fits, and how to write one that survives. Ends in a table the user works through from their own account. Checked October 2026.

## Which threads

- **Fresh beats old.** A reply in the first 24 to 48 hours is read by the poster and the voters; after a week only search visitors read it, except threads Google or AI engines show, which stay read for months ([Google and AI answers](search-and-ai.md)).
- **Highest intent first:** "alternative to <competitor>", "tools like <competitor>", "best <category> for <situation>", "what do you use for <job>", then posts that describe the problem without asking for a tool.
- **Open and unanswered.** A thread qualifies only when `reddit_get_post` shows neither `locked` nor `archived` and the body is not "[removed]" or "[deleted]". Under about 10 comments with no good answer is the best target; 200 comments bury a new reply. A thread answered well is not a target unless the top answer is outdated or wrong; then the angle is the correction.
- **Not vendor posts.** Launches, "I built", "roast my product" and disguised research posts ("we're the team behind X, curious how you...") are not asking. In our week of r/CRM they were half the new posts.
- **Watch for seeded threads.** A "question" that names two little-known vendors as options "that have come up on Reddit", from a new account, is often planted. Answer the real question or skip it; never join a seeded thread on the vendor's side.

## Finding them

1. **The chosen communities, completely:** `reddit_get_new_posts` on up to 10 subreddits from [communities](communities.md), `since: "7d"` (the most it reaches), `match` set to up to 10 topic terms and competitor names. Read `coverage[]`: a busy community comes back cut to hours, so for those run it daily with `since: "24h"` and `pages: 3`, or shorten the window.
2. **The rest of Reddit, as a sample:** `reddit_search_posts` with three or four question phrasings and `time_range: "month"`, default relevance sort, keeping rows whose `created_at` is in the window. Sorted `new`, search loosens the query and returns posts that merely contain a word. Merge and dedupe on `id`.
3. **Cut without calls:** outside the window, `over_18`, vendor posts, posts that match a term but not the intent (a job ad, news, a meme). Keep the 20 best, freshest first.
4. **Read each:** `reddit_get_post` for `locked`, `archived` and the full body; `reddit_get_comments` (one page, 25 replies) for whether a good answer exists (a top-level comment, `depth: 0`, with a high `score` that solves it), which products are already recommended and by whom, and whether the poster is replying.
5. **Rules:** the verdict per community from [rules](rules.md#the-rules-check). Where the product may not be mentioned, keep the thread only as "answer without the product", or drop it.
6. **Deliver** a table: title, URL, subreddit, age in hours, comments, what the poster needs in one line, answered already (no, partly, yes), products already recommended, rules verdict, and the angle: what a helpful reply would say, using a detail only this post contains. Freshest open unanswered threads first. Default cost about 54 credits (a check-in over 5 communities, 4 searches, 20 threads read with comments, 5 rule reads).

## Writing the reply

Draft only when the user asks. Then:

- **Answer the question in the thread on its own.** The reply must be worth reading if the product line were deleted. Use a detail from the post ("for 20 leads a day from WhatsApp..."). If the angle would fit any thread, it is spam.
- **Ask a question back when the post lacks a fact that decides the answer.** In the busiest "which CRM" thread we read, the poster replied to the comment that asked where the leads came from, and to a method with no product; a dozen product names with links got no reply.
- **Disclose, then mention, only where allowed:** "I work on X, so weigh this" before the product, one mention, at most one link, and only where the rules allow it and the asker would want it. Name competitors fairly; a reply that runs down a competitor reads as a pitch.
- **Write it yourself.** Most of the communities buyers use remove AI-written replies, and moderators read for it: generic openers, symmetric bullet lists, "it depends" without a recommendation, no first-hand detail. A short reply with one real number from the user's experience beats a long polished one.
- **No DMs.** "DM me" and "check my profile" are banned in many communities and read as solicitation everywhere.
- **Then stay.** Answer follow-ups in the thread for a day or two. A reply that leaves after the link looks like what it was.

## Posts, launches and AMAs

- **A post of the user's own** works where the rules allow it and it stands without the product: data the community cannot get elsewhere, a lesson with numbers, a teardown. "How we did X" with the product in the last line is the format moderators remove most.
- **Launch and "I built this" posts** belong in the set-aside threads or the communities made for them (r/SideProject, r/EntrepreneurRideAlong publish few or no rules on it). Expect little from them in buyer communities.
- **AMAs** need moderator approval in most communities that allow them, often weeks ahead, and usually a host with a history in that community. Pitch the moderators through modmail, from the user's account.

## Cadence

- Set reply volume from the team's hours, not the thread count: three hours a week is about ten careful replies.
- One run is a snapshot; the server keeps no state. To get this list daily or weekly, the host schedules `reddit_get_new_posts`, stores `next_since` to send back as `since`, and keeps the ids already seen, as [brand-monitoring](../../brand-monitoring/SKILL.md) does.
