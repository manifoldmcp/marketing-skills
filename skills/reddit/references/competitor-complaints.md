# Competitor complaints

What people dislike about named competitors, what makes them leave, and where they go instead, from Reddit posts and comments. It ends in a table by competitor and theme with quotes and links, and a short list of fresh "alternative to X" threads the user could answer.

## Inputs to settle first

- **Competitors**: two to four names, with the short names people use. A brand name that is also a common word ("Close", "Notion", "Monday") needs the category word in every query.
- **User's product**: what it does better and worse, so each complaint is marked as one the user answers or not.
- **Window**: default the past year. Older complaints may be fixed.
- **Budget**: a default run for three competitors costs about 3 x (4 + 2 + 8) + 5 + 3 = 50 credits (per competitor four post searches, two comment searches and eight threads read, then five open threads checked and the rules of three communities). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the switch phrasings.** Per competitor, `reddit_search_posts` with four of "alternative to <X>", "<X> alternatives", "switched from <X>", "leaving <X>", "<X> vs", "<X> pricing" and `time_range: "year"` (1 credit each). Apply the on-topic check in the [router](../SKILL.md#search-and-watching).
2. **Search the comments.** Per competitor, `reddit_search_comments` with two of "moved off <X>", "cancelled <X>", "<X> support", "<X> price increase" (1 credit each), keeping the rows whose `created_at` falls in the window. Comments hold the candid lines that posts dress up.
3. **Read the threads.** For the eight strongest threads per competitor (the most `comments`, a clear complaint or switch), `reddit_get_comments` (1 credit each). Note what failed, the trigger (a price rise, a missing feature, support, reliability, a contract), where they went, and whether anyone defends the competitor.
4. **Cluster by competitor and theme.** Use themes such as price and packaging, a missing feature, reliability, support, complexity, contract or lock-in, and data or privacy. Count distinct threads per theme, and count the destinations people name ("moved to Y").
5. **Mark the open threads.** From the rows, the "alternative to X" and "switching from X" posts from the last 7 days. `reddit_get_post` on each (1 credit) and drop any that is `locked` or `archived`; then the rules verdict from [Subreddit rules](subreddit-rules.md) for their communities. Judge them further with [Threads to reply](threads-to-reply.md) if the user wants to answer.
6. **Deliver** two tables. Complaints: competitor, theme, threads, intensity (top `score`), two quotes with links, destinations named, whether the user's product answers it (yes, partly, no), and the message angle. Open threads: title, URL, subreddit, age, comments, competitor, rules verdict.

## Judgment

- Unhappy customers post; happy ones rarely do. Say what the competitor's unhappy users say, never that the competitor is failing.
- Date every complaint. A pricing complaint from before the competitor's last price change may be stale.
- A complaint the user's product shares is not an angle. Mark it "no" and say so; the user needs to know before a prospect raises it.
- The destinations people name are the real competitive set. A surprise there goes to the `competitors` group's [find competitors](../../competitors/references/find-competitors.md).
- In a reply, answer the question and state the difference; never attack the competitor. Threads close ranks against a vendor who does.
- Turning the themes into positioning or pricing is the `competitors` group's [positioning](../../competitors/references/positioning.md) and [messaging and pricing](../../competitors/references/messaging-pricing.md).
