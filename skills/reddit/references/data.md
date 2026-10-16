# Reading the Reddit data

How Manifold's `reddit_*` tools behave, what their numbers mean, and where they mislead. Read it before counting anything. Behaviour checked against live calls on 15 October 2026 ([field notes](field-notes.md)).

## Where the data comes from

The tools run on a vendor that reads Reddit, not on Reddit's own API. Reddit has been closing outside access (reportedly blocking unauthenticated JSON around May 2026, suing scrapers, licensing its data to Google and OpenAI), so fields can change or empty without warning. Every response names `meta.provider`. When a field the work depends on comes back empty, say so rather than fill the gap.

## Search is a sample, watching is complete

- **Search** (`reddit_search_posts`, `reddit_search_comments`) answers "what has Reddit said about X". One ranked page per call, six to nine rows, never complete; `meta.cursor` pages on. A count from search is a count in the sample: compare like with like (same query, sort, time range) and never report it as a total.
- **Watching** (`reddit_get_new_posts`) answers "what appeared in these communities since I looked". Every post in up to 10 subreddits since `since` (at most 7 days), oldest first, with `match` keeping posts whose title or body holds a term (exact, case-insensitive, applied after the fetch). Complete only for the window each `coverage[]` entry reports: `complete: false`, or a `covered_from` later than `since`, means a busy community was cut. One page reached back about 8 hours on r/SaaS and r/smallbusiness and 6 days on r/CRM; three pages (the maximum) cover roughly a day of the busiest business communities. For those, check in daily with `since: "24h"` and `pages: 3`, or pass `meta.cursor`. `next_since` overlaps by two minutes, so dedupe on `id`.
- **There is no way to watch all of Reddit for a keyword.** Find the communities, then watch them.

## Search quirks

- **Use the relevance sort.** Sorted `new`, post search loosens the query: rows contain a query word but are mostly off topic (1 of 6 rows on topic for "hubspot alternative", against 7 of 7 sorted by relevance).
- **Comment search applies `time_range` itself.** The vendor ignores it ("all" and "week" returned the same rows, and "year" with `sort: "top"` returned comments from 2023), so the tool drops the rows outside the window from the page it got, and a page can come back short or `NoData`. The default sort leans to the last three weeks; `sort: "top"` with `time_range: "all"` reaches older comments.
- **Check that rows are on topic.** If most titles and bodies lack the query's words, the search failed; do not count the rows. Retry with the relevance sort and `time_range: "all"`, filtering by `created_at`.
- **Comment search returns a lot of self-promotion.** In one query, 5 of 9 comment rows were vendors placing their own product and 1 was off topic. Read before quoting.
- A comment row names its thread (`post_title`) and the comment it answers (`parent_id`), so a hit can be read in context before paying for `reddit_get_comments`. Its `depth` is null for replies.
- `reddit_search_subreddits` counts the communities behind one page of about seven posts: three to six communities a call. It answers "where is this discussed", never "every subreddit about X".

## Fields to distrust

- There is no member count: Reddit replaced it, and the tools no longer carry the always-empty field. Use `weekly_active_users` (weekly visitors) and `weekly_contributions` from `reddit_get_subreddit`.
- `submit_text` was empty on all 20 communities read; posting guidelines shown on submit are not available. `rules` was empty on 2 of 20, which may mean no rules or rules kept elsewhere: read the community on Reddit before assuming none.
- `upvote_ratio` is empty on search rows and filled on `reddit_get_new_posts` rows and `reddit_get_post`.
- `score` a day after posting barely separates replies: the 25 first replies in a 78-comment thread scored 1 or 2. Judge reply quality on whether the poster answered, not on score, in the first days.
- `body` is cut at 2,000 characters in search, check-in and comment rows; `reddit_get_post` returns the whole post.
- `reddit_get_comments` returns 25 comments a page in reading order (`depth` 0 is top level); `meta.cursor` loads more.

## Credits and caching

- Every `reddit_*` call is 1 credit (per subreddit per page on `reddit_get_new_posts`), so most Reddit jobs cost 20 to 60 credits. The expensive call in Reddit work is `aeo_run_ai_answers`, 18 credits a prompt on its five default engines.
- Search, thread and comment results are cached 1 hour, a community's record 7 days; a result this account already paid for is free while cached. `reddit_get_new_posts` is never cached.
- Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop at `BudgetExceeded`. `dry_run: true` prices any call for free.
- The server keeps no history and has no scheduler. A weekly watch or a before-and-after needs the host to store the queries, the ids seen, `next_since` and the counts.

## Evidence rules

- Every row carries its link: the thread `url`, or the comment's own `url` for a quote. Quote verbatim; do not paraphrase inside quote marks.
- Leave usernames out of research tables; Reddit users are private individuals.

## Without the connector

The work can be done by hand, slowly: Reddit's own search sorted by relevance, each community's sidebar and rules page, the "new" listing for a week of posts, and the thread itself. Google with `site:reddit.com` and the problem phrasing finds the threads Google ranks. Say plainly that a hand sample is small, and count nothing as a total.
