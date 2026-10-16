# Monitoring mentions

A mention check that runs every day or week and reports only what is new: posts, comments and videos by other people that name the brand, a product or a founder. Read it before setting up alerts, a schedule or a "who talked about us" report. Checked October 2026; the platform limits behind it are in [coverage](coverage.md).

## Two ways to see, and why both

- **Watching is complete for its window.** `reddit_get_new_posts` returns every post in up to 10 subreddits since `since`, oldest first, with `match` keeping posts whose title or body holds a term. Nothing else Manifold has is complete. It does not see comments, and it reaches back at most 7 days.
- **Search is a ranked sample.** Every other source (Reddit search, TikTok, YouTube, LinkedIn, Instagram hashtags) returns one ranked page per call and can miss a post. In our run a week of Reddit search for "hubspot" returned 7 posts, of which 1 was among the 23 HubSpot posts in five watched communities, but it found the week's biggest thread (score 42, in a community not on the list). Search is the net for communities nobody named; watching is the record for the ones that matter ([field notes](field-notes.md)).
- **Comments carry most mentions.** A brand is named in replies ("we switched to Acme") far more often than in titles, and watching does not see them. Search comments as well as posts.
- **Promote what search keeps finding.** A community that turns up in search twice goes on the watch list.

## Settle these first

- **Terms:** brand, product and founder names, the domain, common misspellings, up to 10. A short or common-word name needs the qualifier people use ("acme app", "acme.io"); test each term once and drop any whose rows are mostly about something else.
- **Communities:** three to ten subreddits where the audience talks, chosen with [reddit](../../reddit/SKILL.md). If the user has none, `reddit_search_subreddits` on the category with `time_range: "year"` (1 credit) and propose the top five.
- **Platforms:** default Reddit, TikTok, YouTube and LinkedIn. Instagram only with a brand hashtag (its search is by hashtag only). Facebook only for group URLs the user supplies. X cannot be searched; say so.
- **Own accounts:** the brand's company pages, channels and founders' profiles, so their posts are dropped. In our LinkedIn sample 3 of 10 rows for a brand were its own.

## Cadence

- **Daily by default.** The Reddit window reaches back 7 days at most, and one page of a busy subreddit covers far less: in our run one page covered 8 hours of r/SaaS, about 2 days of r/sales and r/startups, and 6 days of r/CRM. For a community that size, run daily with `pages: 3`, or accept that search is the only net there.
- **Weekly suits only quiet communities**, where `coverage[]` shows a page reaching the whole week. A skipped week leaves a gap that only ranked search partly fills, and a missed Reddit window cannot be recovered past 7 days.
- **Read `coverage[]` every run.** `complete: false`, or `covered_from` later than `since`, means the window was cut: raise `pages` for that community, run more often, or pass `meta.cursor`. Say the gap in the report rather than implying completeness.
- **The first run is a baseline.** Look back 7 days (Reddit `since` of "7d", platforms `since: "week"`) and report what it finds as the starting list, not as news.

## Calls per run

- **Watch:** `reddit_get_new_posts` with the subreddits, `match` set to the terms, `since` set to the stored `next_since`. Store the new `next_since`. If the stored value is older than 7 days because runs were missed, pass "7d" and say the gap before it is not covered.
- **Search Reddit:** per term, `reddit_search_posts` with `sort: "relevance"` and `time_range: "week"` (or "year" when the week comes back empty), and `reddit_search_comments` with `time_range: "all"`. Never sort by new across all of Reddit: the vendor then drops the query and returns the site's newest posts. Comment search ignored its time range in our run (every row was from 2023 and 2024), so filter on `created_at` yourself.
- **Search the platforms:** per term, `tiktok_search_videos` with `since: "day"` (or "week") and `sort: "latest"`, `youtube_search_videos` with the same `since` (it ranks by relevance whatever the sort), `linkedin_search_posts` with the same `since`. `instagram_search_posts` with the brand hashtag. `facebook_get_group_posts` on each named group, keeping posts whose text holds a term.
- **Read a thread that matters:** `reddit_get_post` and `reddit_get_comments`, 1 credit each; comments under a video or post one at a time with `tiktok_get_comments`, `youtube_get_comments`, `instagram_get_comments` or `linkedin_get_comments` (LinkedIn shows 10).

## Reading the rows

- **Keep only rows whose text holds the term.** Platform search is fuzzy. TikTok "attio" returned 0 of 7 videos about Attio (it matched "aito", "atico", "atio"); YouTube "attio" returned 13 of 15 about "80". A two-word query ("attio crm") made YouTube precise (20 of 20) but TikTok still returned 24 of 30 videos that only said "crm". LinkedIn and Reddit post search were on topic in our sample. Check `text`, `hashtags` and `transcript` where filled.
- **Namesakes survive the text filter.** Reddit comment search for "attio" returned two comments using a Bengali word spelled the same. Read before counting.
- **Dedupe on `id`, never on dates.** `created_at` on YouTube and LinkedIn is computed from "3 days ago" and exact only to that unit, so a mention can look older than the last run. The search windows overlap between runs on purpose; repeats are expected. Cross-posts (one author, same text, two subreddits) count once.
- **Mark partners and paid posts.** A post tagged with a partner hashtag (#AcmePartner) or `is_ad` true is the brand's own reach, not organic talk.
- **Treat seeded posts as noise.** Several Reddit search hits in our sample read like seeded "review" posts (lowercase, a product question built around the brand name, generated two-word usernames). They are a 2026 pattern aimed at AI citations; do not count them as organic sentiment and do not answer them as buyers.

## Triage and alerts

Two tiers keep alerts rare enough to read.

- **Alert at once** on a complaint or question with reach (rule of thumb: Reddit `score` 10 or more, or 5 comments, inside a day; a video above its author's usual views; a LinkedIn post from an account with a large following), on a mention by a journalist or a creator the user named, and on a spike: mentions in a day above about three times the trailing daily average (our heuristic, not a tested threshold), which usually means a launch, an outage or a story.
- **Everything else goes in the digest** of the run, grouped by kind: complaint, question, comparison, recommendation, praise ([sentiment](sentiment.md)).
- **For each row:** about the brand or a namesake; kind; reach (`score` and `comments` on Reddit, views, likes and `author_followers` elsewhere); suggested action (reply, thank, pass to support, save as a testimonial, none); source (complete window or search).
- **Say what is complete.** Every report states "Reddit communities: complete for the window; elsewhere: best effort", and lists platforms not watched. A quiet run is one line ("no new mentions since Tuesday").
- **Replying is the user's call.** Before suggesting a Reddit reply, the community's rules are read the way [reddit](../../reddit/SKILL.md) reads them. The server never sends alerts, emails or posts; if the host has a notification tool, offer to send the table there.

## State and schedule

- The host runs the schedule. If it has a scheduler (a scheduled task, a routine, cron), set the check up there; if not, say so: the user asks again each day and the host reruns it with the stored state.
- The host stores the settings (terms, communities, platforms, own accounts) and the state: `next_since`, the ids seen in the last 14 days (the widest search window plus a late run), each run's date and `meta.credits_charged`. Without it every run is a first run. A changed setting starts its own baseline.
- Reddit searches are cached 1 hour and platform searches 6 hours, so a rerun inside that window returns the same rows for nothing and adds nothing. `reddit_get_new_posts` is never cached and charged in full.

## Cost

| Run | Calls | Credits |
|---|---|---|
| Daily, five subreddits, two terms | 5 watch + 4 Reddit search + 6 platform search + 1 hashtag | about 16, 480 a month |
| Daily, five busy subreddits at `pages: 3`, two terms | 15 + 11 | about 26, 780 a month |
| Weekly, quiet communities, `pages: 3`, two terms | 15 + 11 | about 26, 112 a month |
| Reading a thread to triage | per call | 1 |

Say both numbers before the schedule starts and pass `max_credits` on every call. Pages and threads that rank on Google for the brand are a cheap extra: `seo_get_serp` on the brand name once a week (1 credit), comparing URLs with last week's. New linking pages show up in `seo_get_backlinks` by `first_seen`; mentions without a link are a link-building job ([link-building](../../link-building/SKILL.md)).

## Without the connector

Free tools cover part of it by hand: F5Bot emails new Reddit and Hacker News posts and comments containing a keyword; Hacker News has a public search at hn.algolia.com; Bluesky's search is public; Google Alerts catches some web pages and news. Run the same terms on each platform's own search, sorted by newest, with the same qualifiers, and apply the same filters: the term in the text, namesakes out, own accounts out. Say plainly that hand checks are a sample, and that X, private groups and most comments are not covered.
