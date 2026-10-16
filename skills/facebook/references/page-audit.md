# Auditing a Facebook page

A read on one page, the user's own or a competitor's, and a benchmark when competitor pages sit beside it: how big it is, how often it posts, which posts earn a response, what it pays for, and what its videos say. Ends in a scorecard per page, the posts that worked with links, and three recommendations tied to the numbers. Field traps are in [data](data.md).

## Check the page first

`facebook_get_profile` on each page. If `website` is not the brand's domain, or `followers` is far below the brand's other accounts, it may be a fan, regional or abandoned page: ask before going on. Note `industry`, whether `phone`, `location` and `website` are filled, and `running_ads`.

## The window

Read about 30 posts (about 10 calls of `facebook_get_posts`, paging with `meta.cursor`) and report the dates they span. A page posting twice a day gives about two weeks; a page posting weekly gives seven months. For a fair comparison, use the same number of posts or the same date span for every page and say which. Under about 8 posts, a median says little: say so rather than score it.

## Scoring

- **Posts a week** and the **video share** (`media: "video"`, which lumps images with text, see [data](data.md#traps-in-the-fields)).
- **Median engagement per post** (`likes` plus `comments`) and **per 1,000 followers.** Medians, because one viral post drags a mean. Shares are not in listings; leave them out of medians.
- **Giveaways and contests apart.** Like-share-tag posts inflate the numbers (25 times the median on one page we read) and are engagement bait Meta demotes. Report them in their own line.
- **Outliers:** posts at three times the page's median or more. Six outliers in the window mean a repeatable format; one means luck. Read the top ten and bottom ten for topic, first line, video or not, link or not, question or not.
- **The shortlist in full:** `facebook_get_post` on the top three to five for `shares`, reel `views` and `duration_s`, which listings lack.
- **Cadence:** compare the last third of the window with the rest before recommending more posts. Posting more is not the answer when engagement per post is falling; a rival whose cadence fell recently may have cut effort, which is an opening.

## What a number means

- **Engagement, not followers.** Followers pile up over years and most never see a post now that recommendations fill much of the feed ([feed](feed.md)). A page with 200,000 followers and 15 reactions a post is not a working channel; say so plainly.
- **The benchmark is the peers.** 0.15% engagement by followers was the 2025 average across 130,683 business pages (Socialinsider), with link posts at 0.05%. Use it only to say whether a page is broadly normal; judge it against its competitors over the same window.
- **Views mean little alone.** Facebook counts a video view after a few seconds, so many views and few comments can mean people scrolled past. Read `views` with `comments`.
- **Formats a page keeps posting usually work for it;** one it tried once and dropped probably did not.
- **Recycled content is a warning.** A page posting other creators' clips, memes or stock reels risks reduced distribution on everything since July 2025; flag it ([feed](feed.md#original-content-since-july-2025)).

## Paid apart

`ads_get_advertiser_ads` with `platform: "facebook"`, `advertiser` set to the page's `ad_library_id` (or its name), and `active_only: true`. Count active ads, their formats and offers, and the oldest `first_shown`: an ad running for months is one that pays. An ad whose `body` repeats the first line of an organic post means the page boosted that post: mark it and take it out of the organic medians. If a rival's page is quiet but its ads are many, its Facebook presence is paid and the organic comparison says little; say so. Reading the ads in depth is [paid-ads](../../paid-ads/SKILL.md).

## Video transcripts

When the user wants what a page's videos say, alone or as part of the audit:

- Pick the videos: from the page's rows with `media: "video"`, ranked by `likes` plus `comments` (listing `views` are mostly null on reels), then `facebook_get_post` on the top ones for real views. About 15 credits for ten videos from a page.
- `facebook_get_transcript` on each; pass `language` for non-English videos. Take the hook (the first sentence or two), the problem named, the claims, the proof, the offer and the call to action.
- Compare the top half by views with the bottom half: kind of hook (question, claim, story, demo), transcript length in words (the nearest reading of length when `duration_s` is null), topic and call to action.
- A transcript is speech only; burned-in text carries many Facebook hooks. When a top video's transcript is thin, say the hook is probably on screen.
- Use a competitor's transcripts for patterns and claims to answer, never for lines to copy: copying is also what the unoriginal content rules penalise.

## Deliver

A scorecard with one row per page: followers, posts a week, video share, median engagement per post and per 1,000 followers, outliers, active ads, best format, window covered. A table of top posts: page, link, date, video or not, first line, likes, comments, shares, views, and why it worked in one line. Three recommendations, each tied to a number in the tables. For the user's own page, ask for a Meta Business Suite export if reach, clicks or demographics matter: the tools see only what a visitor sees.
