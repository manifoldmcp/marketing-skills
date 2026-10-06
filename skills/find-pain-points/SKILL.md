---
name: find-pain-points
description: When the user wants the problems buyers name across Reddit, Facebook groups and social comments, ranked, with quotes. Collects what people write on Reddit, in Facebook groups and under TikTok, Instagram, YouTube and Facebook posts, clusters it into pains written as jobs, and ranks them by the people behind each and their intensity. Also use when the user mentions customer pain points, voice of the customer (VOC), jobs to be done, what people struggle with, or audience research across several platforms. Pain points from Reddit alone go to find-reddit-pain-points, Facebook groups alone to mine-facebook-groups, the comments on one platform to mine-tiktok-comments, mine-instagram-comments, mine-youtube-comments or mine-facebook-comments, complaints about a named competitor to find-competitor-complaints, doubts before buying to find-objections, whether demand exists to check-demand.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find pain points

The problems buyers name, from every place they talk, in one ranked list. Each source skill collects raw quotes (Reddit threads, Facebook group posts, comments under TikTok, Instagram, YouTube and Facebook posts); this skill clusters the quotes into pains, counts the people behind each, ranks them by frequency and intensity, and keeps two or three quotes with links as proof. It ends in a ranked table.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_posts` and `reddit_get_comments` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- This skill reads several tool groups: `reddit_*`, the platform tools (`tiktok_*`, `instagram_*`, `youtube_*`, `facebook_*`) and `seo_*` to find Facebook groups. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run on the sources that remain, and name the missing sources in the deliverable, since a signal that was not checked is not a signal that is absent.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the problem the product solves, the ICP, the competitors, customer language already collected) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first. After delivering, offer to write the pains and the customer language into `.agents/product-marketing.md` with [create-product-context](../create-product-context/SKILL.md), which says which section each result fills.

## Inputs to settle first

- **Topic**: the problem space or the category, in the buyer's words ("meal planning for families", "small business bookkeeping"), plus the competitor names if the pains around them matter.
- **Sources**: where these buyers talk. Default for business software: Reddit and YouTube. Default for consumer products: Reddit, TikTok and Instagram. Facebook only when the user has group URLs or pages to read, since Facebook has no keyword search. A request about one source alone goes to that source's skill, which delivers its own table: [find-reddit-pain-points](../find-reddit-pain-points/SKILL.md), [mine-facebook-groups](../mine-facebook-groups/SKILL.md), or the comment skill for the platform.
- **Window**: default the past year. Older pains may be solved already.
- **Budget**: the sum of the sources' estimates; the synthesis costs nothing. Reddit alone costs about 33 credits, three Facebook groups about 33, and the comments on one platform 35 to 41 (TikTok about 36, Instagram about 36, YouTube about 41, Facebook about 35). The business default (Reddit and YouTube) costs about 33 + 41 = 74 credits; the consumer default (Reddit, TikTok and Instagram) about 33 + 36 + 36 = 105. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the sources.** Choose from the defaults above, or from where the user's buyers are. If unsure whether a platform has the conversation, one search there costs 1 credit and settles it.
2. **Collect.** Run each chosen source's skill on the same topic and window, and take its raw rows, not its summary: the quote, the link, the author, the date and the score or likes.
   - Reddit: [find-reddit-pain-points](../find-reddit-pain-points/SKILL.md), steps 1 to 3.
   - Facebook groups, when the user has group URLs or wants some found: [mine-facebook-groups](../mine-facebook-groups/SKILL.md), steps 1 to 4.
   - Comments under TikTok videos: [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md). Instagram posts and reels: [mine-instagram-comments](../mine-instagram-comments/SKILL.md). YouTube videos: [mine-youtube-comments](../mine-youtube-comments/SKILL.md). Facebook page posts: [mine-facebook-comments](../mine-facebook-comments/SKILL.md). Take each up to its cleaning step.
3. **Cluster.** Group the quotes by the underlying problem, not by their words: "takes forever to load" and "so slow on my phone" are one pain. Name each cluster in the buyers' words. Split a cluster that hides two different jobs. Write each cluster as a job: "When <situation>, I want <outcome>, but <what gets in the way>". Aim for 5 to 12 clusters; a problem mentioned once goes into "other". Stop collecting when the last five threads or videos read added no new cluster, and say so; if new clusters still appear at the end of the default sample, say the list is not saturated and offer a second pass.
4. **Count.** For each cluster: distinct authors (dedupe on the author: one person posting five times is one voice), the number of sources it appears on, and the summed score or likes on its quotes as a measure of agreement. Cap any one thread or video at a third of a cluster's authors, so one viral post cannot carry a pain alone.
5. **Rank.** Rate each cluster's intensity from 1 to 3: 1 is an annoyance ("a bit clunky"), 2 is a cost or a workaround (hours lost, a spreadsheet, a hack), 3 is action: switching, paying, cancelling or asking for a recommendation now. Score each cluster as distinct authors times intensity; break ties by the number of sources, since a pain on three platforms is more general than one on one.
6. **Deliver** a table: rank, pain (in buyer words), the job behind it, what people do about it now (the workaround or the product they use), distinct authors, sources, intensity (1 to 3), score, the competitor named if any, and two or three quotes with links. Under it, one line for each of the top three pains on what it means for the user's product or message.

## Judgment

- Every finding carries its evidence: a quote with its link. Quote verbatim and short in the research tables; leave usernames out of anything the user will publish, and paraphrase in public copy.
- Every search here is ranked and never complete. A count is a count in the sample: say how big the sample was, and use counts to rank pains against each other, never to size a market. For how many people have the problem, use [check-demand](../check-demand/SKILL.md).
- Every Reddit search follows the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching): the sort and `time_range` to pass, the on-topic check, and what to do when a search fails.
- Comments under a brand's own posts skew to fans and support questions. Comments under reviews, comparisons and "how I fixed X" videos come from buyers weighing options: weigh those higher.
- A feature request is a pain stated as a solution. Record the problem behind it ("I wish it had X" becomes the job X would do).
- Loud is not common. A single thread with 400 upvotes is one conversation; the author count and the cap in step 4 keep it in proportion. Views and likes measure attention, not intent to buy; a request for a recommendation ("is there a tool that...") is the strongest intent signal a public post carries.
- Not every voice is a buyer. Where a source shows who is talking (a role in the post, a community made of the ICP), count the authors who fit the ICP separately; a pain carried mostly by people who would never buy goes below the line, marked as such.
- A pain with intensity 3 and few authors can still be the best opening: few people, but they are buying. Flag it rather than burying it at the bottom.
- Never post, reply, send a survey or contact anyone. The deliverable is a table for the user. Listening to the same topics on a schedule belongs to [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
- Credits: if the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. A result this account already paid for is free while cached, so a rerun on the same topic soon after costs little.

## Related skills

- One source alone, with its own table: [find-reddit-pain-points](../find-reddit-pain-points/SKILL.md), [mine-facebook-groups](../mine-facebook-groups/SKILL.md), [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md), [mine-instagram-comments](../mine-instagram-comments/SKILL.md), [mine-youtube-comments](../mine-youtube-comments/SKILL.md) and [mine-facebook-comments](../mine-facebook-comments/SKILL.md).
- Complaints about named competitors and why people switch: [find-competitor-complaints](../find-competitor-complaints/SKILL.md). The doubts that stop a purchase: [find-objections](../find-objections/SKILL.md).
- Whether demand exists: [check-demand](../check-demand/SKILL.md). Who the buyers are: [build-personas](../build-personas/SKILL.md).
- People posting about the problem, to engage with: [find-reddit-threads](../find-reddit-threads/SKILL.md) and [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md).
- Content ideas from the questions found here: [find-content-ideas](../find-content-ideas/SKILL.md).
