# Reddit notes

What every skill that reads Reddit shares: which tools it needs, how to read the numbers, the floors, the credits and the handoff. Every skill that calls a `reddit_*` tool follows these notes.

## Tools

Reddit skills run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_posts` and `reddit_get_subreddit` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `reddit_*` tools are not, the Reddit tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: every Reddit skill starts from Reddit.
- The Reddit threads AI answers cite ([build-ai-citations](../../../build-ai-citations/SKILL.md)) and the [Reddit strategy](../../SKILL.md) also use `aeo_run_ai_answers` and `seo_get_serp`. If the `aeo_*` tools are off, find the threads from Google alone and say AI engines were not checked; if the `seo_*` tools are off, from AI answers alone.

## Search and watching

- **Search is a sample.** `reddit_search_posts` and `reddit_search_comments` return one ranked page per call (1 credit) and are never complete; `meta.cursor` pages the next set. Use them for "what has Reddit said about X". A count from search is a count in the sample: compare like with like (same query, sort and time range) and never report it as a total.
- **Watching is complete.** `reddit_get_new_posts` returns every post in up to 10 subreddits since `since` (at most 7 days), oldest first, with `match` to keep posts whose title or body holds a term. It costs 1 credit per subreddit per page. Read `coverage[]`: `complete: false`, or a `covered_from` later than `since`, means a busy community was cut; raise `pages` (up to 3), shorten `since`, or pass `meta.cursor`. Dedupe rows on `id`.
- **Sort and time range.** Search with `sort: "relevance"`: sorted `new` or `top` across all of Reddit, the search drops the query. On `reddit_search_posts` a `time_range` works; on `reddit_search_comments` a narrow one often comes back empty, so run comment search with `time_range: "all"` and keep the rows whose `created_at` falls in the window.
- **Check that search rows are on topic.** If most titles and bodies do not contain the query's words, the search dropped the query; do not count the rows. If a search is off topic or comes back empty when it should not, search again with `sort: "relevance"` and `time_range: "all"`, and filter on `created_at` yourself.
- `reddit_search_subreddits` counts the communities behind one page of posts. It answers "where is this discussed", not "every subreddit about X", so run several phrasings and ask the user for communities they know.

## Floors

- **Active community.** Keep a subreddit when people post there daily: `weekly_contributions` above about 100, or at least 7 posts in a 7-day check-in. Subscribers alone mislead: a large community can be dead, and a 20,000-member one that is exactly the ICP beats a general one with millions where a post sinks in an hour.
- **Open thread.** A thread qualifies for a reply only when it is neither `locked` nor `archived` (from `reddit_get_post`) and its body is not "[removed]" or "[deleted]". Fresh beats old: a reply in the first 48 hours is read by the poster and the voters; after 7 days it is read only by search visitors, except threads Google or AI engines show, which stay read for months ([build-ai-citations](../../../build-ai-citations/SKILL.md)).
- **Rules first.** No skill suggests a reply or a post before the community's rules are read the way the rules check in [find-subreddits](../../../find-subreddits/SKILL.md) reads them. No rule on self promotion is not permission.

## Evidence

- Every row carries a link: the thread `url`, or the comment's own `url` for a quote. Quote verbatim; do not paraphrase inside quote marks.
- Leave usernames out of research tables. The user needs the thread, not the person, and Reddit users are private individuals.

## Credits

- Say the estimate before the first paid call; each skill gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Every `reddit_*` call is 1 credit (per subreddit per page for `reddit_get_new_posts`), so most Reddit jobs cost 20 to 60 credits. The expensive call is `aeo_run_ai_answers`: 18 credits per prompt on its five default engines.
- Search, thread and comment results are cached 1 hour and a subreddit's record 7 days; a result this account already paid for is free while cached. `reddit_get_new_posts` is never cached, because it must see what arrived.

## Handoff

- Never post, reply, vote, send messages or schedule. No manifold tool does, and the account at risk is the user's. The deliverable is a table the user works through from their own account.
- Draft a reply or a post only when the user asks. Then: answer the question in the thread first, use a detail from the post, say the user's affiliation plainly ("I work on X"), mention the product only where it answers the question and the rules allow it, and add at most one link.
- The server keeps no state. To check the same communities every day or week, the host schedules `reddit_get_new_posts`, stores `next_since` to send back as `since`, and keeps the ids already seen: that is a job for the [monitor-brand-mentions](../../../monitor-brand-mentions/SKILL.md) skill.
