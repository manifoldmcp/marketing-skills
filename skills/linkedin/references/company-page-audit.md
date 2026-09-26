# Company page audit

An audit of one LinkedIn company page, the user's or a competitor's: whether the page says who it is for, how often it posts and about what, which posts earn engagement, and how it compares with two peers. It ends in a scorecard and a short list of fixes.

## Inputs to settle first

- **Page**: the company slug (the part after `/company/`) or the full page URL.
- **Whose**: the user's own page (the output is fixes) or a competitor's (the output is what to learn and what to avoid).
- **Peers**: two companies to compare with. Default: the competitors the user names.
- **Window**: default the latest 20 posts on the page, 10 on each peer.
- **Budget**: a default run costs about (1 + 3 + 20) + 2 x (1 + 1 + 10) + 2 = 50 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the page.** `linkedin_get_company` (1 credit): `bio` (the description or tagline), `website`, `industry`, `employees` and `location`. Check that the bio says who the company serves and what it does in its first line, that the website is set, and that the industry is the one buyers would expect. The page's follower count is not published here, nor its logo, banner or button; list those for the user to check by eye.
2. **Read the feed.** `linkedin_get_company_posts`, paging until it has the latest 20 posts (1 credit a page, about three). From `created_at` (approximate), posts a week over the last 90 days. From `text`, the topic of each post: product news, customer story, thought leadership, hiring and culture, events, reshared press. Note the length and the hook of each.
3. **Measure engagement.** The feed carries no engagement, so `linkedin_get_post` on each of the latest 20 (1 credit each): `likes` and `comments`. Median per post, and the top five and bottom five. What the top five share (a topic, a person featured, a number in the first line) is the page's best evidence.
4. **Compare with peers.** For each peer: `linkedin_get_company` (1 credit), one page of `linkedin_get_company_posts` (1 credit) and `linkedin_get_post` on its latest 10 (1 credit each). Same measures.
5. **See who talks about the company.** `linkedin_search_posts` with the company name and `since: "month"`, two pages (1 credit a page): employees, customers and partners posting about it. A page whose people post about it reaches far beyond its own feed.
6. **Deliver** a scorecard: item (bio, website, industry, employees, posts a week, median likes, median comments, topic mix, best topic, mentions by others last month), this page, each peer, and a verdict per row. Then the top five posts (URL, first line, likes, comments), and five fixes for the user's page, or five lessons from a competitor's.

## Judgment

- Company pages reach fewer people than the people behind them. A page with modest engagement and employees who post about it is healthier than a page shouting alone.
- Judge the median, not the best post. A page with one viral post and twenty posts under ten likes has a cadence problem, not a success.
- Hiring and culture posts often earn the most likes from employees and candidates, and the least from buyers. Separate them when judging what works for pipeline.
- `employees` is LinkedIn's own headcount, not the company's. It lags hires and exits.
- `created_at` is approximate. Posts a week over 90 days is reliable; the exact day a post went out is not.
- For a competitor's whole marketing beyond this page, use the [competitors](../../competitors/references/teardown.md) group; for their LinkedIn ads, the [paid-ads](../../paid-ads/references/competitor-ads.md) group.
