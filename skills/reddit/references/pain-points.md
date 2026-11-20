# Pain points from Reddit

The problems people describe in the user's space, in their own words, clustered into themes with counts, quotes and links. Reddit is where people describe a problem before they know what to buy, so it gives the language for positioning, copy, content and the roadmap. For pains ranked across Reddit, reviews and social comments, this feeds [voice-of-customer](../../voice-of-customer/SKILL.md).

## Mine, do not ask

Posting "what are your biggest pain points with X?" is forbidden outright in 5 of the 20 communities we read, and gets the poster banned in some (r/marketing permanently, r/PPC on the first post), and r/smallbusiness tells researchers to search its archive instead ([field notes](field-notes.md#twenty-communities-rules-15-october-2026)). The archive is the better source anyway: people describing a problem unprompted are more honest than people answering a vendor.

## Method

1. **Search the problem, not the product.** `reddit_search_posts` with six phrasings of how people talk about the job: "struggling with <job>", "how do you <job>", "<job> is killing me", "hate <task>", "frustrated with <category>", "is there a better way to <job>", with `time_range: "year"` and the default relevance sort (1 credit each, six to seven rows each). Then the two best phrasings with `subreddit` set to each of the top three communities from [communities](communities.md) (6 credits). If most rows do not contain the query's words, the search failed: do not count them.
2. **Search the comments.** `reddit_search_comments` with three phrasings (1 credit each). People describe a pain in replies ("same here, we lost a client over this") more than in titles. Comment search's default sort leans to the last few weeks, and `time_range` only trims that page, since the vendor ignores it: run one `sort: "top"` search with `time_range: "all"` when older language matters.
3. **Read the richest threads.** The 15 threads with the most `comments` whose title or body states a problem: `reddit_get_comments` on each (1 credit, 25 replies). `reddit_get_post` only where the body was cut at 2,000 characters and the rest matters. Stop early once five threads in a row add no new problem, and say so.
4. **Extract and cluster.** Every statement of a problem: what went wrong, what they tried, what it costs them (hours, money, a client, a risk), what they wish existed. Cluster into five to ten themes by the underlying problem, not the words. Count distinct threads per theme: twenty agreeing replies in one thread are one thread with strong agreement, so note the top `score` as intensity.
5. **Deliver** a table: theme, the problem in one line in the buyer's words, threads (count), intensity, two or three verbatim quotes each with its link, what people tried (workarounds and tools named), and the implication (a message, a feature, a content idea). Under it, the phrases people repeat, for copy and keywords, and how many threads the table rests on. Default cost about 33 credits.

## Reading it honestly

- **Counts come from ranked samples.** Write "in 15 threads read", never "most people on Reddit". Rank themes against each other; never size a market from them ([market-research](../../market-research/SKILL.md) does that).
- **Separate the pain from the requested fix.** "I wish it had X" becomes the job X would do.
- **Specific beats loud.** A detailed post with a number ("this costs us six hours a week") outweighs ten one-line agreements; one thread with 400 upvotes is still one conversation.
- **A request for a recommendation** ("is there a tool that...") is the strongest intent a post carries. Flag a theme carried by such requests even when few threads hold it.
- **Skip planted posts and vendor research.** A "problem" post whose top reply is a product link from an account that only posts about that product; a post from "the team behind X" asking for pain points. In open communities these can be a large share of the results.
- **Reddit skews** toward technical, price-sensitive, hands-on users. For an enterprise ICP, say the themes may over-weight price and under-weight procurement and security; where a post shows the author's role, count the ICP authors separately.
- **Quotes keep their link; usernames stay out** of the table. Reddit users are private individuals; paraphrase in public copy.
