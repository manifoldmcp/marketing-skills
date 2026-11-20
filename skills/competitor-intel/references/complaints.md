# Complaints

What people dislike about named competitors, why they leave, and where they go, mined from public discussion. The richest source of a rival's weak spots and of the user's message angles, and the easiest to over-read. Checked October 2026.

## What the sample is

- **Unhappy and delighted people post; the middle rarely does.** Online ratings follow a J-shaped curve because of who chooses to write (Hu, Pavlou and Zhang, MIS Quarterly, 2017). A pile of complaints says what a rival's unhappy users say, never that the rival is failing or how many are unhappy.
- **Reddit search is ranked and never complete.** `reddit_search_posts` and `reddit_search_comments` return one ranked page a call; counts are counts in that sample. Compare themes with each other under the same queries, never with a total. Sort by relevance: sorted by new or top across all of Reddit, search drops the query. Run comment search with `time_range: "all"` and filter on `created_at` yourself, since a narrow window often comes back empty. If most rows do not contain the query's words, the search failed; do not count them. Full Reddit mechanics: [reddit](../../reddit/SKILL.md).
- **Review sites** (G2, Capterra, Trustpilot) hold the most structured complaints, sorted by role and company size, but no Manifold tool reads them. App store reviews (`apps_get_reviews`), Google Shopping reviews (`shopping_get_reviews`) and a Google Business Profile's review topics (`maps_get_place`) are readable where the competitor sells through those. Read them in the host's browser or ask the user, and note that vendors solicit reviews, so a rival's review count measures its review programme as much as its customers.
- **Engines reuse the same complaints.** Perplexity and Google's AI answers cite review sites and Reddit threads; ChatGPT stopped showing Reddit threads in August 2026 ([ai-search](../../ai-search/SKILL.md)). A complaint repeated on a cited page is one prospects will hear from an engine.

## Queries that find switching

Per competitor: posts for "alternative to <X>", "<X> alternatives", "switched from <X>", "leaving <X>", "<X> vs", "<X> pricing"; comments for "moved off <X>", "cancelled <X>", "<X> support", "<X> price increase". A brand that is also a common word ("Close", "Notion", "Monday") needs the category word in every query. Then `reddit_get_comments` on the strongest threads (most comments, a clear complaint or switch).

## Reading a switch

For each story note the four forces: the **push** (what failed and the trigger: a price rise, a missing feature, support, reliability, a contract), the **pull** (where they went and why), the **habit** (what kept them: data, a contract, a trained team) and the **anxiety** (what they feared about moving). Habit and anxiety are what the user's onboarding and migration offer must answer; the push alone does not move anyone.

- **Cluster by theme**: price and packaging, missing feature, reliability, support, complexity, contract or lock-in, data or privacy. Count distinct threads per theme.
- **Count destinations.** The products people say they moved to are the real competitive set. A surprise there goes back into [finding competitors](finding-competitors.md).
- **Date every complaint.** One from before the rival's last release or price change may be fixed; leave out anything older than two years.
- **Note defenders.** Where people defend the rival, the complaint is contested; say so.
- **A complaint the user shares is not an angle.** Mark it "no" and say so; the user needs to know before a prospect raises it.

## Fresh threads

Search misses most new posts. To find threads asking for an alternative this week, `reddit_get_new_posts` on the three communities where complaints clustered with `since: "7d"` and `match` set to the rival names (1 credit per subreddit per page; complete for its window). `reddit_get_post` shows `locked` and `archived`. Replying is the user's call, under the community's rules: answer the question and state the difference, never attack the rival. Threads close ranks against a vendor who does. Never post for the user.

## Evidence

Quote verbatim with the link; never paraphrase inside quote marks. Leave usernames out: the user needs the thread, not the person.

## Cost

About 53 credits for three competitors (per competitor four post searches, two comment searches, eight threads read; then a 7-day check-in on three communities and five open threads checked). Search results are cached an hour; `reddit_get_new_posts` never is.

## Deliver

A complaints table (competitor, theme, threads, top `score`, two dated quotes with links, destinations named, what holds people back, whether the user answers it: yes, partly, no; the message angle) and an open-threads table. Turning themes into positioning is [voice-of-customer](../../voice-of-customer/SKILL.md).
