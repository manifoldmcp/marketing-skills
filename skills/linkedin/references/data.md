# Reading LinkedIn data

What Manifold's `linkedin_*` tools return, what they cannot see, what each call costs, and the rules that keep a small sample honest. Every other reference in this skill reads LinkedIn through these notes. Checked against the tools and a live read in October 2026 (see [field notes](field-notes.md)).

## What LinkedIn shows a logged-out reader

All six tools read what LinkedIn shows the public, with no logged-in account. That sets the limits, and no vendor gets past them without running real accounts, which Manifold does not do.

- **No views, shares or saves.** `views`, `shares` and `saves` are always null. Reach is invisible; engagement is `likes` (all reactions) and `comments`. The user's own analytics (impressions, in-network and out-of-network reach since June 2026, followers gained) are the only reach data; ask for them when judging the user's own posts.
- **Ten commenters, no reactors.** A post shows up to 10 comments, each with the commenter's name and `author_url`, and no time, likes or replies for the comment. Who reacted is never shown. On a post with 200 comments, 10 come back: a sample, not the thread.
- **No headline, no job titles.** `linkedin_get_profile` returns the about text in `bio` (often cut to a line), `location`, `followers`, `experience` (companies, usually without titles or dates) and `recent_posts`. For a person's current title and company, `leads_get_person` (3 credits, see [outbound](../../outbound/SKILL.md)) on the profile URL.
- **No feed, no audience.** The tools cannot see what a member's feed shows, who follows a page, or who clicked.

## The tools

| Question | Tool | Cost | Notes |
|---|---|---|---|
| What is posted on a topic, by whom | `linkedin_search_posts` | 1 credit a page of 10 | LinkedIn's own ranking, never complete, no sort. `since` is day, week, month (default), year or all |
| One post's text, engagement, commenters | `linkedin_get_post` | 1 | The same fields as a search row; use it for a URL from elsewhere or a row with null counts |
| The commenters as rows | `linkedin_get_comments` | 1 | The same 10 people as `top_comments`, no paging; skip it when a search or post row is in hand |
| One person | `linkedin_get_profile` | 1 | `recent_posts` is their latest five or six (URL, first 280 characters, date), no engagement |
| One company page | `linkedin_get_company` | 1 | `followers`, `employees`, `industry`, `website`, `bio`, `topics` (specialties), `related` (similar pages), a few `people` |
| A page's own posts | `linkedin_get_company_posts` | 1 a page of about 9 | Newest first, no engagement at all: `likes`, `comments`, `author_followers` and `top_comments` are empty |

Searches, posts, comments and company feeds are cached 6 hours and profiles and pages 24 hours; a result this account already paid for is free inside that window. A typical job costs 30 to 50 credits; profiles and single posts are where the count grows, so fetch them for the shortlist only. Say the estimate before the first paid call, pass `max_credits` when the user set a budget and stop on `BudgetExceeded`; `dry_run: true` prices a call for free.

## Field traps

- **`media` is coarse.** video means a video stream; image covers photos, document carousels and GIFs, which have no value of their own, and polls read text. The company feed says nothing about media, so its rows read null: `linkedin_get_post` or search gives it. A non-null `transcript` also marks a real video (and gives what it says). Before judging a format, the host opens the top post URLs if it has a browser, or the user checks them by eye.
- **`created_at` looks exact and is not.** It comes back as a full timestamp, but LinkedIn shows an age ("3d", "2w"), so it is exact only to that unit. Enough for "this week"; not for posting-hour analysis.
- **`text` is cut at 2,000 characters**, so a longer post is known only as long.
- **`links` holds lnkd.in short links**, unresolved. A "link in the first comment" shows only if the author's own comment is among `top_comments`.
- **Authors.** Each row names its author as `author` (the handle), `author_name`, `author_url` and `author_followers`. Size authors from the row; call `linkedin_get_profile` only for a shortlist, passing `author_url` as `url`. An `author_url` under `/company/` is a company page, read with `linkedin_get_company`; in the October read, 4 of 30 search rows were company pages.
- **`employees` is LinkedIn's own headcount**, which lags hires and leavers. `people` is the handful of employees the page shows.
- **Company feed rows carry no engagement**, so a page audit is `linkedin_get_post` on every post judged, at 1 credit each.
- **A call can fail on an odd character** in a profile's text; retry once, then move on and note it.

## Thresholds

- **Sample.** Judge a format, a hook or a leader on at least 30 posts in total and 10 per group compared. A pattern in five posts is an anecdote.
- **Size.** Compare engagement per 1,000 followers when authors differ in size: 200 likes from a 5,000-follower author beats 2,000 from 500,000.
- **Medians, not means.** One viral post drags a mean.
- **Giveaways apart.** A "comment X to get it" post earns comments by design; spot it by comments far above likes, or `top_comments` that are all the same word. Count it apart, or it inflates every comment figure. Since July 2026 these posts also lose reach (see [the feed](feed.md#substance-bait-and-ai-writing)).
- **Suspect engagement.** Likes near or above the author's follower count on a small account, or a block of generic comments from branded pages ("Sales Tips", "Marketer Tips"), point to pods or automation. Leave such posts out of format medians and never copy what they did.
- **Freshness.** About 72% of a post's views come in its first three days. For commenting and buyer signals, only posts from the last few days are worth acting on; for judging formats, a month or a year is fine.
- **Search is a sample of what already did well.** Its ranking favours posts with engagement, so it shows what top posts share, not what every post gets. Narrower queries find steady posters a broad one misses.

## Without the connector

The same work by hand, slowly: search LinkedIn for the topic with the "Posts" filter and "Past month", open 30 or more posts, and log for each the author, their followers (on the profile), the first line, length, format, likes and comments, and whether it is a giveaway. Medians per group, per 1,000 followers where sizes differ. For a page, open its posts tab and log the last 20 the same way. Say plainly that a hand sample of a few dozen posts shows patterns, not proof, and that LinkedIn's search, logged in, is personalised to the person searching.

## The handoff

- Never post, comment, like, connect, message or schedule. The deliverable is a table or a plan with a link to every post, person and page it cites; the user acts from their own account.
- Draft a post or comment only when asked, one per row, tied to the evidence in that row and marked as a draft. Never pass off a creator's post or words as the user's.
- The server keeps no state. A daily commenting list or a weekly watch is a job the host schedules, keeping the post URLs already listed; if the host has no scheduler, say so.
