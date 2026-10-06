---
name: monitor-brand-mentions
description: When the user wants to know every time someone mentions the brand. A mention check that runs every day or week on the host's schedule and reports only what is new since the last run, from watched subreddits (a complete window), all of Reddit, TikTok, YouTube, LinkedIn, Instagram hashtags and Facebook groups, each mention triaged with its reach and a suggested action. Also use when the user mentions alert me when someone mentions us, monitor Reddit for our brand, social listening, brand monitoring, mention alerts, track mentions every day, or who talked about us this week. Reactions to a launch go to create-launch-plan, mentions in AI answers to check-ai-visibility, complaints as research to find-pain-points or find-competitor-complaints, one weekly digest of everything to write-weekly-report.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Brand mentions

A mention check that runs every day or week and reports only what is new: Reddit posts and comments, TikTok and YouTube videos, LinkedIn posts and Instagram posts that name the brand, a product or a founder since the last run. Reddit is watched two ways, because only one of them is complete. It ends in a table of new mentions, each with a suggested action; the host keeps the ids already seen.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_get_new_posts` and `reddit_search_comments` (hosts often add a prefix, for example `mcp__manifold__reddit_get_new_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The steps read `reddit_*` and the platform groups (`tiktok_*`, `youtube_*`, `linkedin_*`, `instagram_*`, `facebook_*`). If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run what the rest allow, and mark those platforms "not watched" in every report rather than reporting zero.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand, product and founder names, the domain, the category, the social accounts to leave out as the brand's own) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Terms**: the brand name, product names, the domain, founders' names and common misspellings, up to 10. A brand that is also a common word needs a qualifier ("acme app", "acme.io") or the search fills with namesakes.
- **Communities**: three to ten subreddits where the audience talks, chosen with [find-subreddits](../find-subreddits/SKILL.md). Default, if the user has none: `reddit_search_subreddits` with the category and `time_range: "year"` (1 credit), and propose the top five.
- **Platforms**: default Reddit, TikTok, YouTube and LinkedIn. Instagram only if the brand has a hashtag, since Instagram search is by hashtag only. Facebook only for group URLs the user supplies.
- **Cadence**: daily by default. Weekly suits only quiet communities: the Reddit window reaches back at most 7 days, and a busy subreddit can outrun three pages in a week (`coverage[]` shows when it does).
- **Budget**: a daily run with five subreddits and two terms costs about 5 + 2 + 2 + 2 + 2 + 2 + 1 = 16 credits, about 480 a month. A weekly run with `pages: 3` costs about 15 + 11 = 26 credits, about 112 a month. Reading a thread to triage it adds 1 credit a call. Say both numbers before setting up the schedule; pass `max_credits` on every call so no run overspends.

## Steps

1. **Set up (first run only).** Settle the inputs, store them with the host (see [Schedule and state](#schedule-and-state)), and set the schedule. The first run is the baseline: it looks back 7 days (Reddit `since` of "7d", platform `since: "week"`) and reports what it finds as the starting list, not as news.
2. **Watch the chosen subreddits (complete).** `reddit_get_new_posts` with the `subreddits` (up to 10 a call), `match` set to the terms, and `since` set to the stored `next_since` (1 credit per subreddit per page, never cached). Read `coverage[]`: when `complete` is false or `covered_from` is later than `since`, the window was cut, so raise `pages` (up to 3) for that community or run more often. Store the new `next_since`. The window reaches back at most 7 days: if the stored value is older because a run was missed, pass "7d" and say the gap before it is not covered. Dedupe on `id`; the two-minute overlap repeats rows on purpose. `match` reads titles and bodies, not comments.
3. **Search all of Reddit (ranked, misses happen).** For each term, `reddit_search_posts` and `reddit_search_comments` with `sort: "relevance"` and `time_range: "year"`, or `"all"` when that comes back empty, as comment search often does (1 credit a page each). Keep only the rows whose `created_at` is after the last run and whose text holds the term, and drop ids already seen. Never sort these by new: across all of Reddit the vendor then drops the query and returns the newest posts on the site; the search rules in the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md) show how to spot that. Ranked by relevance, a fresh mention can sit below older threads, so this is a net for communities not on the watch list, not the main source. Search comments as well as posts: a brand is named in replies ("we switched to Acme") far more often than in titles, and step 2 does not see comments. A community that turns up here twice is a candidate for the watch list.
4. **Search the platforms.** For each term, `tiktok_search_videos` with `since: "day"` (or `"week"`) and `sort: "latest"`, `youtube_search_videos` with the same `since` (it ranks by relevance whatever the sort, so the window does the work), and `linkedin_search_posts` with the same `since` (1 credit a page each). `instagram_search_posts` with the brand's hashtag and the same `since` (1 credit a page). For Facebook groups the user named, `facebook_get_group_posts` on each group URL (1 credit a page), keeping the posts whose text holds a term. Drop rows whose `author` is one of the brand's own accounts, and ids already seen: the `since` buckets overlap between runs, so repeats are expected.
5. **Triage.** Sort each new mention into the alert tiers (see Judgment), then for each: is it about the brand and not a namesake; what kind it is (praise, question, complaint, comparison, recommendation); its reach (Reddit `score` and `comments`, views and likes on the platforms); and the action (reply, thank, pass to support, save as a testimonial, none). For a Reddit thread worth an answer, `reddit_get_post` and `reddit_get_comments` (1 credit each), then [find-reddit-threads](../find-reddit-threads/SKILL.md), after reading the community's rules with [find-subreddits](../find-subreddits/SKILL.md).
6. **Store and deliver.** The host stores the new `next_since`, the ids seen in the last 14 days (the widest `since` bucket plus one late run), the run's date and its credits. Deliver a table: date, platform, community or author, title or first line, URL, reach, kind, suggested action, and the source (complete window or search). A quiet run is one line ("no new mentions since Tuesday").

## Judgment

- Mentions come in two tiers. Alert at once on a complaint or a question with reach (a rule of thumb: Reddit `score` 10 or more, or 5 comments, inside a day; a video over its author's median views) and on any mention by a journalist or a creator the user named. Every other new mention goes into the run's digest.
- Say which rows are complete. Watched subreddits are complete for the window `coverage[]` reports; every other source is a ranked search that can miss a post. The report says "Reddit communities: complete; elsewhere: best effort".
- The watch list is the reliable path. When searches keep finding mentions in one community, add it to step 2 rather than relying on search.
- A missed Reddit window cannot be recovered past 7 days. For a daily schedule, a skipped day is fine; a skipped week leaves a gap that only the ranked search partly fills.
- `created_at` on YouTube and LinkedIn rows is approximate, so a mention can look older than the last run. Dedupe on `id`, not on dates.
- Not covered: X (no search), Facebook beyond the groups the user supplies, comments under TikTok, YouTube and Instagram videos (readable only one video at a time), and LinkedIn comments. Say so once in the first report.
- Pages and threads that rank on Google for the brand are a cheap extra: `seo_get_serp` on the brand name once a week (1 credit), comparing the URLs with last week's.
- Replying to a mention is the user's call. The server never sends alerts, emails or posts.

## Schedule and state

- The host runs the schedule. If it has a scheduler (a scheduled task, a routine, a cron job), set the skill up there with the cadence the user chose. If it has none, say so: the user asks again each day or week, and the host reruns the skill with the stored state.
- The host stores the settings (terms, communities, platforms) with the state (`next_since`, ids seen, each run's date and `meta.credits_charged`) where the next run can read it: a file, a doc, a sheet, its memory. Without the stored state every run is a first run. A changed setting starts a new baseline for what it changed; report those rows apart until they have history.
- A month is 30 daily runs or about 4.3 weekly runs. `reddit_get_new_posts` is never cached and is charged in full every run; Reddit searches are cached 1 hour and platform searches 6 hours, so a rerun inside the window returns the same rows for nothing and adds nothing new. `dry_run: true` prices any call for free.
- The deliverable is the change table. If the host has an email, chat or notification tool, offer to send the table there; the host sends it, not the server.

## Related skills

- Choosing the communities to watch: [find-subreddits](../find-subreddits/SKILL.md). Answering a thread a run found: [find-reddit-threads](../find-reddit-threads/SKILL.md).
- Mentions in AI answers: [check-ai-visibility](../check-ai-visibility/SKILL.md).
- The reaction to a launch, from launch day to T+30: [create-launch-plan](../create-launch-plan/SKILL.md).
- What customers complain about, as research rather than an alert: [find-pain-points](../find-pain-points/SKILL.md).
- Mentions, ranks, competitors and AI visibility in one weekly report: [write-weekly-report](../write-weekly-report/SKILL.md).
