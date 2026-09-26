# Pain points from Reddit

The problems people describe in the user's space, in their own words, clustered into themes with counts, quotes and links. Reddit is where people describe a problem before they know what to buy, so it gives the language for positioning, copy, content and the roadmap. It ends in a table of themes.

## Inputs to settle first

- **Space**: the job the buyer is trying to get done and the problem around it, not only the category name. Ask for two to four phrasings ("chasing late invoices", "freelance bookkeeping").
- **Communities**: default the top three from [find subreddits](find-subreddits.md); if there are none, run its first step (a few credits).
- **Window**: default the past year, for current language and tools.
- **Competitors**: optional. Complaints about named competitors are [competitor complaints](competitor-complaints.md); here they only tag a theme.
- **Budget**: a default run costs about 12 + 3 + 15 + 3 = 33 credits (twelve post searches, three comment searches, fifteen threads read, three cut posts read in full). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the problem, not the product.** `reddit_search_posts` with six phrasings built from how people talk about the problem: "struggling with <job>", "how do you <job>", "<job> is killing me", "hate <task>", "frustrated with <category>", "is there a better way to <job>", with `time_range: "year"` and the default relevance sort (1 credit each). Then the two best phrasings with `subreddit` set to each of the three communities (6 credits). Apply the on-topic check in the [router](../SKILL.md#search-and-watching).
2. **Search the comments.** `reddit_search_comments` with three of the phrasings and `time_range: "year"` (1 credit each). People describe a pain in replies ("same here, we lost a client over this") more than in titles. If a search comes back empty, rerun it with `time_range: "all"` and keep the rows whose `created_at` is inside the window.
3. **Read the richest threads.** Take the 15 threads with the most `comments` whose title or body states a problem, and call `reddit_get_comments` on each (1 credit, one page). Call `reddit_get_post` (1 credit) only where the row's body was cut at 2,000 characters and the rest matters.
4. **Extract and cluster.** From posts and comments, pull every statement of a problem: what went wrong, what they tried, what it costs them (hours, money, a client, a risk), and what they wish existed. Cluster into five to ten themes by the underlying problem, not by the words used. Count distinct threads per theme: twenty agreeing replies in one thread are one thread with strong agreement, so note the top `score` as intensity.
5. **Deliver** a table: theme, the problem in one line in the buyer's words, threads (count), intensity (top score in the theme), two or three verbatim quotes each with its link, what people tried (workarounds and tools named), and the implication (a message, a feature, a content idea). Under the table, list the exact phrases people repeat, for copy and keywords.

## Judgment

- Counts come from ranked samples. Write "in 15 threads read", never "most people on Reddit".
- Separate the pain from the requested fix. People ask for features; the problem behind the ask is the finding.
- A detailed post with numbers ("this costs us six hours a week") outweighs ten one-line agreements. Specific beats loud.
- Skip planted posts: a "problem" post whose top reply is a product link from an account that only posts about that product.
- Reddit skews toward technical, price-sensitive and hands-on users. If the ICP is enterprise buyers, say the themes may over-weight price and under-weight procurement and security.
- The same job across reviews, social comments and Reddit together is the `customers` group's [pain points](../../customers/references/pain-points.md), which uses this playbook for its Reddit part.
