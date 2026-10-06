# Launch reaction report

What people said after the launch, from the platforms the tools can read: Reddit posts and comments, TikTok, YouTube and LinkedIn posts since launch day, and the comments under the launch posts themselves. It ends in counts by platform and sentiment, the themes behind them, the quotes that show each theme, and the questions still waiting for an answer.

## Inputs to settle first

- **Names**: the product name, the company name, the domain, and common misspellings. If a name is a common word, pair it with a qualifier (the category, the domain).
- **Launch day**: the start of the window. Default window: launch day to today.
- **Launch posts**: the URLs of the user's own launch posts and threads (Reddit, TikTok, YouTube, Instagram, Facebook, LinkedIn, X). The comments under them are the richest reaction.
- **Launch subreddits**: the communities the launch was posted in, from [communities](communities.md).
- **Competitors**: names that separate talk about the category from talk about the product.
- **Pasted reactions**: comments from venues no tool reads (Product Hunt, Hacker News, Slack), if the user wants them in, marked as pasted.
- **Budget**: a default pass costs about 19 + 9 + 5 + 6 + 3 = 42 credits (Reddit, video, LinkedIn, the launch posts, coverage), and 64 at T+30, when coverage reads the backlink index (25). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Reddit.** Two ways, because only one is complete; the search rules are in the [Reddit notes](../../create-reddit-plan/references/platforms/reddit.md).
   - In the launch subreddits, when launch day is within the last 7 days: `reddit_get_new_posts` with the subreddits, `since` set to launch day as an ISO timestamp, `match` set to the names and `pages: 3` (1 credit per subreddit per page). It returns every post in the window; read `coverage[]` to check nothing was cut.
   - Across Reddit: `reddit_search_posts` and `reddit_search_comments` with each name, `sort: "relevance"` and `time_range: "month"`, or `"all"` when that comes back empty, as comment search often does (1 credit a page, two pages each). Keep the rows whose `created_at` is after launch day and whose title, body or comment holds the name.
   - For each launch thread: `reddit_get_post` (1 credit) for `score`, `upvote_ratio` and `comments`, and `reddit_get_comments` (1 credit a page) for every reply.
2. **Video.** `tiktok_search_videos` with the name, `sort: "latest"` and `since: "month"` (or `since: "week"` in launch week), and `youtube_search_videos` with the same `since` (it ranks by relevance whatever the sort, so the window does the work; 1 credit a page each). Keep the videos about the product, not the word, posted after launch day. For the five with the most `views`, the comments: `tiktok_get_comments` or `youtube_get_comments` (1 credit a page).
3. **LinkedIn.** `linkedin_search_posts` with the name and `since` covering the window (1 credit a page). LinkedIn exposes no comments; `linkedin_get_post` (1 credit) gives `likes` and `comments` counts for the posts that matter.
4. **The launch posts.** The comments under the user's own posts: `instagram_get_comments`, `facebook_get_comments`, `tiktok_get_comments` or `youtube_get_comments` with each URL (1 credit a page). Instagram search is by hashtag only, so `instagram_search_posts` with the brand's hashtag (1 credit) is the one way to find others' posts there. X has no search or comments here: `twitter_get_tweet` on the launch post (1 credit) gives its reply, repost, like and view counts only.
5. **Coverage.** At T+1 and T+7, `seo_get_serp` on the product name with `depth: 30` (3 credits): articles and posts about the launch that already rank. At T+30, `seo_get_referring_domains` on the site with `limit: 1000` (25 credits): rows with `first_seen` after launch day are the sites that linked since. The default 100 rows are the strongest domains, and the backlink index lags the web by weeks, so earlier or smaller pulls miss most launch coverage.
6. **Classify.** Read every mention and comment kept. Give each a sentiment (positive, neutral, negative, question) and a theme (price, onboarding, a bug, a missing feature, a comparison with a competitor, praise for a use case). Count by platform, sentiment and theme. Keep the two clearest quotes per theme, with the URL.
7. **Deliver** four tables: mentions by platform and sentiment; themes with their count, share and two quotes each; the launch posts with their engagement; and open questions (the comment, the URL, its age) for the founder to answer. Add three lines on what to fix or say next. To keep watching after T+30, [monitor-brand-mentions](../../monitor-brand-mentions/SKILL.md) sets up the recurring run with the host.

## Judgment

- Search on every platform is ranked and never complete. Counts from search are what the searches returned, a lower bound: say so, and do not set them against another product's launch as if they were totals. Only `reddit_get_new_posts` in the launch subreddits is a full count.
- Read before counting. A name that is a common word brings in unrelated posts, and a sarcastic "great, another AI app" is not praise.
- YouTube and LinkedIn dates are approximate ("3 weeks ago" turned into a date). Near launch day, open the post to check.
- Product Hunt, Hacker News and private communities are not searched. Their reactions are in the report only if the user pasted them, and marked so.
- Unanswered questions are the first action. A launch thread where the founder answers every question keeps being read.
- A negative theme that repeats (a bug, a price objection) is the most useful line in the report. Put it first with its count; do not bury it under praise.
- Run the report at T+1, T+7 and T+30 with the same names and the same queries, so the passes compare.
