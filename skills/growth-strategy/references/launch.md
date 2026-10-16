# Launching

A launch is a date that concentrates attention. The plan decides where the attention comes from (communities, launch platforms, creators, press, the user's own list), what has to happen before the date for each, and how the result is measured after it, on a timeline from T-30 to T+30. Checked October 2026.

## What a launch yields now

- **Product Hunt is curated.** Its changelog (years inferred from the order of entries): since September 2025 launches are scheduled by date only and go live at 12:01 a.m. Pacific for a full day, and drafts are not indexed; since December 2025 the leaderboard ranks by points, mostly upvotes with comments counted, and makers get a launch-day dashboard of points and rank; moderators choose what is featured above the fold; Coming Soon pages are gone. Its CEO said in November 2025 that bought votes are caught and penalised.
- **How many get the shelf.** hunted.space listed about 32 featured launches a day from 17 September to 15 October 2026 (929 in 29 days, range 8 to 91), and the day's top product had a median of about 430 votes. The widely repeated "only 10% of launches get featured" traces to one blog analysis from September 2024; no primary source confirms it.
- **What it brings.** Every published figure for visitors and signups per launch (one to five thousand visitors when featured, a few hundred when not) comes from launch consultants with no method. Treat them as unknown and measure with a UTM link. The lasting search value is small: in our test Product Hunt's 45,664 ranking keywords were mostly product pages ranking for their own product's name, and "alternatives" pages; a launch page catches people already searching for the brand ([field notes](field-notes.md)).
- **Hacker News.** Show HN is for something people can try; HN's guidelines ask users not to solicit upvotes or comments, and voting rings are detected. No tool reads HN; the user reads the current rules and posts.
- **Social launch posts without an audience reach almost no one.** In our TikTok sample, 23 of 30 recent videos on an app category were vendors announcing or promoting their own product; their median was 138 views, against 724 for creators' videos on the same topic.
- **Launch day is one day.** Plan T+7 and T+30 with the same care: the results post, an answer to every question, press follow-ups with numbers. That is where a quiet launch recovers.

## Venues

- **Two or three venues done well beat eight done badly.** A founder alone cannot answer comments in eight places on one day.
- **Product Hunt, Hacker News, Indie Hackers, Slack and Discord groups and newsletters** are common venues no Manifold tool reads. Name them with the user's notes and have the user read each venue's current rules and timing. Never report votes, ranks or comments from them; pasted comments go in the [reaction report](reaction-report.md), marked as pasted.
- **Ask for attention, not votes.** Ask the list to look and comment; buying or trading votes is penalised on Product Hunt and Hacker News.

## Communities

Where the launch can be posted and in what form, so the post stays up. Finding subreddits and reading their rules in depth belong to [reddit](../../reddit/SKILL.md); Facebook groups to [facebook](../../facebook/SKILL.md). The launch reading:

- **Sort each community into one format, quoting the rule line:** launch post allowed (with its flair, day or karma condition); only in the weekly or pinned promotion thread; a useful post with no link (the building story, a lesson, a free resource; the link in a comment only if allowed, otherwise in the profile); comments only. Not stated is not permission. A community that bans self-promotion bans launch posts.
- **Study launches that survived** in communities that allow them: `reddit_get_post` for `score`, `upvote_ratio` and `comments`, `reddit_get_comments` for how the community treated it (1 each). Note the title shape, text or link post, and the weekday and hour.
- **Take part from T-30.** An account whose first post is its own launch looks like spam to moderators and members, and filters remove many such posts unseen. No tool sees an account's age or history: ask. If the date does not allow a month of participation, move the date or drop the venue.
- **One post per community, written for its rules and readers;** the same text in ten places reads as spam. Space posts so every comment in the first hours gets an answer.
- **Small and exact beats big:** a community with high weekly volume buries a post within hours. Rules change: read them again in launch week.

## Creators for launch week

Searching, sizing and vetting creators belongs to [creators](../../creators/SKILL.md). The launch reading:

- **Fit:** one page of each creator's recent posts (`tiktok_get_videos`, `youtube_get_videos` or `instagram_get_reels`, 1 each): do they cover new products, do they take paid posts (`is_ad`, or "ad" in the text), and do they post often enough to hit a date.
- **Dates:** product in hand by T-21 (T-30 for YouTube reviews), brief by T-10, post live between launch day and T+3, a code or link per creator.
- **A gifted creator chooses whether and when to post.** Only a paid or agreed post is a launch-week commitment; plan gifted ones as a bonus. Default 15 small creators (10,000 to 100,000 followers) over one large one; median views, not followers, predict reach. Paid posts carry the platform's disclosure. An embargo with creators is a request, not a contract.

## Press for launch

Media lists, contacts and pitching belong to [digital-pr](../../digital-pr/SKILL.md). The launch reading:

- **The news test:** something new beyond "we launched" (a new category, data, a known customer, funding, a founder with a story). Without it, trade press, newsletters and podcasts are realistic and national press is not. Say so.
- **Find the angle that got similar launches covered:** `seo_get_serp` for "<competitor> launches" and "<competitor> announces" (1 each), keeping publications, not vendors' pages.
- **Tiers:** at most one exclusive at T-14, offered only when that outlet reaches the ICP better than the rest combined; 5 to 10 embargoed briefings at T-7, asking for the embargo before sending details; launch-day pitches to trade press and newsletters; a T+7 follow-up with results. A reporter who breaks an embargo leaves the list. After T+30 it is ordinary PR.

## Timeline

Every part dates its rows in days from launch (T). With fewer than 30 days left, say which T-30 work no longer fits.

| When | Work |
|---|---|
| T-30 | Venues chosen and their rules read; community accounts taking part; creators shortlisted; press list built; baseline of branded search taken |
| T-21 to T-14 | Product to creators; exclusive offered |
| T-7 | Embargoed briefings out; one post drafted per community; the email to the list written; one UTM link per venue |
| Launch day | H-2: the founder tests signup, payment and analytics end to end. H0: primary venue live. H+1: email to the list. H+2: the founder's own posts. Then one community post every 60 to 90 minutes, never two at once. Pulse checks at about H+3, H+6 and H+10 (below) |
| T+1, T+7 | [Reaction report](reaction-report.md); replies to every open question; press follow-ups with early numbers |
| T+30 | Second reaction report; branded search and referring domains measured again; ongoing watch handed to [brand-monitoring](../../brand-monitoring/SKILL.md) |

**Pulse checks:** `reddit_get_new_posts` on the launch subreddits with `since: "6h"` and `match` set to the names (1 a subreddit, never cached), and `linkedin_search_posts` with the name and `since: "day"` (1). Every open question goes on the founder's reply list.

## KPIs the tools re-measure

- **Branded search, daily:** with Search Console connected, `console_get_search_analytics` with `dimensions: ["date"]` and a filter on `query` with `operator: "contains"` and the brand (free; about two days' lag) shows branded impressions day by day from T-7, the first awareness read by T+3.
- **Branded search, monthly:** `seo_get_keyword_metrics` on the brand; `trend[12]` is monthly, so read it at T+30, not before.
- **Coverage:** at T+30, `seo_get_referring_domains` with `limit: 1000` (25 credits); rows with `first_seen` after launch day are new linking sites. The default 100 rows are the domains passing the site the most rank and the index lags by weeks, so smaller or earlier pulls miss most launch coverage.
- **Signups and sales** come from the user's analytics, split by the UTM per venue.

## Budget and the deliverable

The baseline is about 30 credits (category keywords, branded metrics for the brand and competitors, community and social searches, competitors' launch threads and results pages, the user's own profiles). Community research about 50 to 100; creators about 190; press about 370 with contacts; a reaction report about 42 a pass, 64 at T+30. Say the total for the parts chosen.

Deliver one document: inputs with defaults marked, the baseline in a short table, the gaps (no community history, no press relationships, branded search near zero), the chosen parts and why, the timeline table with owner and KPI per row, and three first actions this week. Posting, emailing, submitting and scheduling stay with the user; the host keeps the date, the plan and the launch post URLs.
