# Launch reaction report

What people said after a launch, from the platforms the tools read: Reddit posts and comments, TikTok, YouTube and LinkedIn posts since launch day, and the comments under the user's own launch posts. It ends in counts by platform and sentiment, the themes behind them with quotes, and the questions still waiting for an answer. Run it at T+1, T+7 and T+30 with the same names and queries, so the passes compare.

## Inputs

- **Names:** product name, company name, domain, common misspellings. Pair a name that is a common word with a qualifier (the category, the domain).
- **Window:** launch day to today.
- **Launch post URLs** on every platform: the comments under them are the richest reaction.
- **Launch subreddits**, competitors' names (to separate category talk from talk about the product), and any reactions pasted from venues no tool reads (Product Hunt, Hacker News, Slack), marked as pasted.

## Calls

About 42 credits a pass, 64 at T+30.

1. **Reddit, two ways, because only one is complete.** In the launch subreddits while launch day is within 7 days: `reddit_get_new_posts` with `since` set to launch day as an ISO timestamp, `match` set to the names and `pages: 3` (1 a subreddit a page); read `coverage[]` to confirm nothing was cut. Across Reddit: `reddit_search_posts` and `reddit_search_comments` with each name, `sort: "relevance"` and `time_range: "month"`, or `"all"` when that comes back empty, as comment search often does (1 a page); keep rows created after launch day that hold the name. For each launch thread, `reddit_get_post` and `reddit_get_comments` (1 each).
2. **Video.** `tiktok_search_videos` with the name, `sort: "latest"` and `since: "week"` in launch week or `"month"` after; `youtube_search_videos` with the same window (it ranks by relevance whatever the sort, so the window does the work). Keep videos about the product, not the word. Comments on the five most viewed: `tiktok_get_comments` or `youtube_get_comments` (1 a page).
3. **LinkedIn.** `linkedin_search_posts` with the name and a `since` covering the window. Each row carries up to 10 `top_comments`; `linkedin_get_comments` lists the comments under the user's own launch post.
4. **The user's own posts.** `instagram_get_comments`, `facebook_get_comments`, `tiktok_get_comments` or `youtube_get_comments` with each URL. Instagram search is by hashtag only (`instagram_search_posts` with the brand's hashtag). X has no search or comments here: `twitter_get_tweet` on the launch post gives its counts only.
5. **Coverage.** At T+1 and T+7, `seo_get_serp` on the product name with `depth: 30` (3). At T+30, `seo_get_referring_domains` with `limit: 1000` (25): rows with `first_seen` after launch day.

## Reading it

- **Search is ranked and never complete.** Counts from search are what the searches returned, a lower bound: say so, and never set them against another product's launch as if they were totals. Only `reddit_get_new_posts` in the launch subreddits is a full count for its window.
- **Read before counting.** A common-word name brings unrelated posts, and sarcasm is not praise. Give each kept item a sentiment (positive, neutral, negative, question) and a theme (price, onboarding, a bug, a missing feature, a comparison, praise for a use case).
- **YouTube and LinkedIn dates are approximate** ("3 weeks ago"); near launch day, open the post.
- **A repeating negative theme is the most useful line.** Put it first with its count; do not bury it under praise.
- **Unanswered questions are the first action.** A launch thread where the founder answers every question keeps being read.

## Deliver

Four tables: mentions by platform and sentiment; themes with count, share and two quotes each with URLs; the launch posts with their engagement; open questions (the comment, the URL, its age) for the founder. Then three lines on what to fix or say next. After T+30, the recurring watch belongs to [brand-monitoring](../../brand-monitoring/SKILL.md).
