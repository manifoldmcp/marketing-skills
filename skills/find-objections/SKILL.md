---
name: find-objections
description: When the user wants to know what makes buyers hesitate before they buy. Finds the doubts about price, trust, fit, effort and timing that buyers type into Google, ask on Reddit and argue about under YouTube reviews, ranks them with quotes and search volume, and says where each needs an answer. Also use when the user mentions buyer objections, why people hesitate to buy, what holds people back from signing up, is X worth it, what people doubt, sales objections, or objections for the FAQ and sales page. Problems people have before they look for a product go to find-pain-points, complaints about a named competitor to find-competitor-complaints, comments under one platform's posts to mine-tiktok-comments, mine-instagram-comments, mine-youtube-comments or mine-facebook-comments, a sales battlecard against one rival to write-battlecard.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Buyer objections

What makes buyers hesitate before they buy a product like the user's: the doubts about price, trust, fit, effort and timing they type into Google, ask on Reddit and argue about under review videos. Pain points are the problem; objections are the doubts about the solution. It ends in a ranked table of objections with quotes, how often they are searched, and where each one needs an answer.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords`, `reddit_search_posts` and `youtube_get_comments` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- This skill reads three tool groups: `seo_*`, `reddit_*` and `youtube_*`. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run on the sources that remain, and name the missing sources in the deliverable, since a signal that was not checked is not a signal that is absent.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the category, the product, the competitors, the objections already written down) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. After delivering, offer to write the top objections and their answers into section 7 of `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Inputs to settle first

- **Category and product**: the category in buyers' words, the user's product, and one or two competitors buyers weigh it against.
- **What the user already hears**: objections from sales calls, demos or support, if any. The run checks them against what buyers say in public and finds the ones nobody raised in a call.
- **Window**: default the past year. Older doubts may be answered by now.
- **Budget**: a default run costs about 20 + 4 + 2 + 8 + 2 + 8 + 5 = 49 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Doubts people search.** `seo_search_keywords` with the category as `seed`, and again with the best-known competitor (10 credits each). Keep the keywords that carry a doubt: "worth it", "legit", "scam", "safe", "review", "cost", "price", "hidden fees", "free", "cancel", "refund", "vs", "alternative". Their `volume` shows which doubts buyers take to Google, and each one with volume needs a page that answers it.
2. **Doubts people ask.** `reddit_search_posts` with four of "is <competitor> worth it", "<category> worth it", "<competitor> legit", "regret buying <category>", "should I get <category>" (1 credit each). `reddit_search_comments` with "<competitor> worth it" and "<category> not worth" and `time_range: "all"` (1 credit each), keeping the rows inside the window. Follow the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching). Then `reddit_get_comments` on the eight busiest on-topic threads (1 credit each): the replies hold the doubts other buyers add, and the answers that settled them.
3. **Doubts under reviews.** `youtube_search_videos` for "<competitor> review" and "is <category> worth it" with `since: "year"` (1 credit each). `youtube_get_video` on the eight most viewed (1 credit each) for the `comments` count, and `youtube_get_comments` on the five with the most comments, one page each (1 credit). Viewers of a review are deciding; their comments are objections more than complaints.
4. **Code each doubt.** One type per quote: price or value, trust or risk (will it work, is the company safe, what about my data), fit ("not for a team like mine"), effort or switching cost (setup, migration, learning), timing ("not now"), or the status quo ("a spreadsheet does this"). Drop complaints from people who already use the product: that is [find-pain-points](../find-pain-points/SKILL.md) or [find-competitor-complaints](../find-competitor-complaints/SKILL.md). Note any reply that resolved the doubt ("I worried about that too, but...") and what did it: a number, a guarantee, a trial, a story.
5. **Rank.** Per objection: distinct authors (dedupe on the author: one person posting five times is one voice), the sources it appears on, and the summed `volume` of its Google keywords. Rank by authors, and break ties by search volume: a doubt people both ask and search is the one most buyers carry silently.
6. **Deliver** a table: rank, objection (in buyer words), type, voices, sources, Google searches (keywords and volume), two quotes with links, what resolved it in public (or "nothing seen"), and where to answer it: the pricing page, an FAQ entry, a comparison page, a guarantee or trial, a sales talk track, an ad. Mark the objections the user listed that buyers did not raise, and the ones buyers raised that the user never mentioned.

## Judgment

- An objection is answered with proof, not with a claim. For each of the top three, name the proof that would settle it (a number, a named customer, a demo, a refund policy) and whether the user has it.
- A doubt with search volume needs a page, because buyers look for the answer before they ever talk to sales. A doubt that appears only in threads belongs in sales talk tracks and onboarding.
- Status quo is the most common objection and the one least often stated. When buyers say "we just use a spreadsheet", the objection is the effort of changing, not the price.
- Price objections from people far outside the ICP (students, hobbyists on a business tool) are noise. Keep them apart.
- Every quote carries its link, verbatim. Counts come from ranked samples: use them to rank objections against each other, never to size a market. Leave usernames out of anything the user will publish.
- Never answer in the threads from here. Turning an objection into copy belongs to the copy skills; turning it into a reply belongs to [find-reddit-threads](../find-reddit-threads/SKILL.md).
- If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.

## Related skills

- The problems behind the purchase: [find-pain-points](../find-pain-points/SKILL.md). Complaints about named competitors and why people switch: [find-competitor-complaints](../find-competitor-complaints/SKILL.md).
- Objections under one platform's posts in more depth: [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md), [mine-instagram-comments](../mine-instagram-comments/SKILL.md), [mine-youtube-comments](../mine-youtube-comments/SKILL.md) or [mine-facebook-comments](../mine-facebook-comments/SKILL.md).
- Pages that answer "X vs Y" and "alternative to X" doubts: [plan-comparison-pages](../plan-comparison-pages/SKILL.md). A talk track against one rival: [write-battlecard](../write-battlecard/SKILL.md).
- Replies in the threads where buyers ask: [find-reddit-threads](../find-reddit-threads/SKILL.md).
