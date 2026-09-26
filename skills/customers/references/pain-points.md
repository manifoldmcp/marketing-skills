# Pain points across sources

The problems buyers name, from every place they talk, in one ranked list. The platform playbooks collect the raw quotes (Reddit threads, comments under TikTok, Instagram, YouTube and Facebook posts); this playbook adds what none of them does alone: it clusters the quotes into pains, counts the people behind each, ranks them by frequency and intensity, and keeps two or three quotes with links as proof. It ends in a ranked table.

## Inputs to settle first

- **Topic**: the problem space or the category, in the buyer's words ("meal planning for families", "small business bookkeeping"), plus the competitor names if the pains around them matter.
- **Sources**: where these buyers talk. Default for business software: Reddit and YouTube. Default for consumer products: Reddit, TikTok and Instagram. Facebook only when the user has group URLs or pages to read, since Facebook has no keyword search.
- **Window**: default the past year. Older pains may be solved already.
- **Budget**: a default run on three sources costs about 20 for Reddit + 2 x 12 for two video platforms = 44 credits; the synthesis costs nothing. Each linked playbook states its own estimate, and its figure wins where it differs. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the sources.** Choose from the defaults above, or from where the user's buyers are. If unsure whether a platform has the conversation, one search there costs 1 credit and settles it.
2. **Collect.** Run each chosen source's playbook on the same topic and window, and take its raw rows, not its summary: the quote, the link, the author, the date and the score or likes.
   - Reddit: [reddit pain points](../../reddit/references/pain-points.md).
   - TikTok: [tiktok comment mining](../../tiktok/references/comment-mining.md).
   - Instagram: [instagram comment mining](../../instagram/references/comment-mining.md).
   - YouTube: [youtube comment mining](../../youtube/references/comment-mining.md).
   - Facebook: [facebook comment mining](../../facebook/references/comment-mining.md), and [facebook group mining](../../facebook/references/group-mining.md) when the user has group URLs.
3. **Cluster.** Group the quotes by the underlying problem, not by their words: "takes forever to load" and "so slow on my phone" are one pain. Name each cluster in the buyers' words. Split a cluster that hides two different jobs. Aim for 5 to 12 clusters; a problem mentioned once goes into "other".
4. **Count.** For each cluster: distinct authors (dedupe per the [router](../SKILL.md#evidence)), the number of sources it appears on, and the summed score or likes on its quotes as a measure of agreement. Cap any one thread or video at a third of a cluster's authors, so one viral post cannot carry a pain alone.
5. **Rank.** Rate each cluster's intensity from 1 to 3: 1 is an annoyance ("a bit clunky"), 2 is a cost or a workaround (hours lost, a spreadsheet, a hack), 3 is action: switching, paying, cancelling or asking for a recommendation now. Score each cluster as distinct authors times intensity; break ties by the number of sources, since a pain on three platforms is more general than one on one.
6. **Deliver** a table: rank, pain (in buyer words), what people do about it now (the workaround or the product they use), distinct authors, sources, intensity (1 to 3), score, the competitor named if any, and two or three quotes with links. Under it, one line for each of the top three pains on what it means for the user's product or message.

## Judgment

- Quotes are what make the table credible. Keep them verbatim and short, with a link each.
- Comments under a brand's own posts skew to fans and support questions. Comments under reviews, comparisons and "how I fixed X" videos come from buyers weighing options: weigh those higher.
- A feature request is a pain stated as a solution. Record the problem behind it ("I wish it had X" becomes the job X would do).
- Loud is not common. A single thread with 400 upvotes is one conversation; the author count and the cap in step 4 keep it in proportion.
- The counts rank pains against each other ([router](../SKILL.md#evidence)). For how many people have the problem, use [demand check](demand-check.md).
- A pain with intensity 3 and few authors can still be the best opening: few people, but they are buying. Flag it rather than burying it at the bottom.
