# Auditing an X account

What one account posts, what earns views and conversation against its own median, and how it compares with one or two competitors read the same way. The deliverable is a scorecard, a table of posts to learn from with a link to each, and three to five changes.

## Settle first

- **The handles:** the user's (company, founder, or both: they are different audits), and one or two competitors, a peer and one the user would like to be. The tools cannot search X, so ask, or take them from each company's site.
- **The goal**, which picks the metric that leads the scorecard: reach (views per follower), conversation (replies per view), traffic (link posts' views, with clicks from the user's own analytics), or a founder's reputation (who replies, read on X).
- **Older posts** the user wants in: a pinned, launch or remembered post, by URL.
- **Budget:** about 2 credits an account (profile and feed) plus 1 per older post: about 11 credits for the user, two competitors and five older posts. Say so before the first paid call.

## First check what the page is

`twitter_get_tweets` returns one of two very different pages, and its `feed` says which: `recent` or `most_liked` ([field notes](field-notes.md)). The tool tells them apart by date order, so on a page of four rows or fewer it reads `recent`; check the dates there.

- **A recent page:** 5 to 20 rows, newest first (a pinned post can sit first, out of order), spanning hours for a news account and weeks for a weekly poster. Use it for cadence and current medians.
- **A greatest-hits page:** about 100 rows sorted by likes, spanning years, often with no post from the last few months. Seen on 6 of 11 accounts in October 2026, including every personal account tried. Use it for what has ever worked for the account (topics, shapes, the first line of its best posts); never for cadence, current medians or "how are we doing now". Say which page you got.

On a greatest-hits page, current performance needs posts by URL: ask the user for recent post URLs or open the profile on X and read the last few weeks by hand, then `twitter_get_tweet` each (1 credit).

## Measure

Per account, on its own posts only:

- **Drop** rows whose text starts "RT @" (a repost row shows zero likes and numbers that are not the account's), rows starting "@" (replies), and posts under 48 hours old (For You stops offering a post at 48 hours, so earlier counts are incomplete). For an account posting twenty times a day the recent page may hold nothing older than 48 hours; say so and use the 24-hour-old posts as a floor.
- **Cadence:** own posts divided by the page's span in days, times 7. State the span. Compare per week, never per page.
- **Median views**, and **median views over followers** (the health number). Median, never mean: one viral post (Vercel's 925K against a 102K median) makes an average meaningless.
- **Median engagement per view:** likes, replies (`comments`) and `shares` (reposts plus quotes) over `views`. Report replies per view on its own line when the goal is conversation, since replies and quotes carry the weight in ranking. `saves` (bookmarks) shows reference value: a how-to or a list with saves above likes is being kept.
- **Shares of the mix:** link posts (an external URL in `links`; an x.com/i/article link is an X Article, not a link out), thread openers ("1/" or a numbered list), questions, and `media` (text, image, video; a link-card post reads text).
- **Leave out** null `views` (a post from before X showed views, late 2022) rather than counting it as zero.

## Tag and find the outliers

- Tag each post's topic (product news, how-to, opinion, story, industry news, promotion, engagement bait) and shape (one line, long post, thread opener, link post, question, image, video).
- The three posts furthest above the account's median views and the three furthest below. What do the top ones share: a topic, a shape, a number in the first line, a named person, no link?
- Several outliers of one topic or shape mean a repeatable format; one means luck. Shapes an account keeps posting usually work for it; one it tried once and dropped probably did not.
- On link posts, compare like with like: link and no-link posts on similar topics, by views per follower. Fewer than five of each is too few to call ([algorithm](algorithm.md#links)).

## Compare

- A competitor ten times the size is a benchmark for topics and shapes, not raw numbers: compare views per follower and engagement per view.
- Never compare across networks or to a published benchmark without matching its formula ([benchmarks](benchmarks.md)).
- A company account and a founder's are different instruments: the founder's usually earns more views per follower and more replies for the same news. Audit both if the company has both.

## Deliver

1. **Scorecard:** metric (followers, posts per week, median views, views per follower, engagement per view, replies per view, link share, top topic, top shape), the user, each competitor, and what the gap means, the goal's metric first, with the page type and span for each account.
2. **Posts to learn from:** account, post URL, views, views over the account's median, engagement per view, topic, shape, and why it worked in one line.
3. **Three to five changes,** each tied to a row: "lead with the numbered how-to threads that earn three times the median", "write link posts so the text stands alone", "the bio has no site link". Drafts only when the user asks, from the posts that worked.

Things the tools cannot see, said plainly when they matter: who replied and what they said, impressions by source (For You or Following), link clicks, follower growth, and whether the account pays for Premium. The user's own X analytics has the first three; for a Premium decision, see [strategy](strategy.md#premium).

## Re-run

An audit is a snapshot. Re-run on the same handles after four to six weeks; the server keeps no history, so the host keeps this run's scorecard. A weekly watch of a rival's posts is a scheduled re-run of the same reads, through the host's scheduler; see [competitor-intel](../../competitor-intel/SKILL.md).

## Without the connector

Open each profile on X while signed in and read the last 20 to 30 posts: date, text, views, replies, reposts, likes, bookmarks, whether it has a link or media. Leave out reposts, replies and posts under two days old, and compute the same medians in a sheet. Signed out, X has shown a profile's popular posts rather than the latest, the same trap as the greatest-hits page. Say that a hand count of a few weeks is a small sample.
