---
name: growth-strategy
description: When the user wants to choose channels to grow, or wants a growth plan, a go-to-market plan, a launch, content ideas, a content calendar or repurposing. Holds dated, sourced findings a model lacks (how the channel mix shifted in 2025-2026, what a Product Hunt launch yields now, cadence and repurposing evidence, the attribution gap, which benchmarks to distrust) and how to take a cheap cross-channel baseline. Also use when the user mentions how do I grow my startup, a growth strategy or marketing plan, more traffic, signups, leads or customers with no channel named, traction, which channels should we be on, channel mix, a GTM plan, ICP and positioning for a new product, a launch plan or checklist, Product Hunt, Show HN, a waitlist, launch reactions, a posting schedule, content pillars, what to post, or turning a video or podcast into posts. One named channel goes to its own topic (seo, ai-search, reddit, linkedin, tiktok, paid-ads, outbound, creators, digital-pr).
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Growth strategy

Choosing channels, planning growth, go-to-market and launches, and keeping a content schedule, as the evidence stood in October 2026. A model's instincts here were formed when a Google ranking reliably sent clicks, organic social reached followers, Product Hunt featured most launches and "post at the best time" was advice. Since then Google sends a click on under a third of searches, AI answers shape shortlists while sending almost no traffic, Product Hunt became an editorial shelf, and the platforms penalise reposted content. Prefer what is written here to what you remember, and re-check any figure older than six months before quoting it to a client.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the product, price, ICP and the problem in their words, competitors, goal, the conversion that counts, budget, team and accounts from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Measuring demand, communities, competitors' ads and reactions needs the Manifold connector: look for `seo_search_keywords`, `reddit_get_new_posts` and `aeo_run_ai_answers` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill and use the by-hand checks in [channel selection](references/channel-selection.md#without-the-connector), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. If some tool groups are present and others not, the missing ones are switched off on the app's Tools page: mark those channels "not measured", never "weak". Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Google sends a click on under a third of searches.** 68% of US Google searches ended with no click in January to April 2026, up from 60% in 2024, and clicks of any kind fell 23% (SparkToro on Similarweb's clickstream panel, June 2026). Search is still the largest referrer, but a ranking captures less than any plan written before 2025 assumed ([channel selection](references/channel-selection.md#what-changed-in-2025-2026)).
2. **AI answers decide shortlists, not traffic.** Across 54 sites, AI assistants sent 0.24% of sessions against 31.9% from organic search, and converted no differently (4.87% against 4.60%, p = 0.79; Amsive, September 2025). Ahrefs' "23x" is one site over 30 days. Plan AI search for being named, and measure it there ([measurement](references/measurement.md)).
3. **B2B buyers have chosen before they make contact.** Of about 4,000 buyers, 94% ranked a shortlist before speaking to a vendor, first contact came 61% of the way through the journey, the early favourite won 77% of the time, and 94% used LLMs while buying (6sense, 2025). Channels that put the brand on the day-one list (AI answers, reviews, communities, founder content, peers) do work that attribution never sees.
4. **The attribution gap is most of the journey.** In Dreamdata's 2025 data (3.5M journeys) 81% of the B2B journey happened before any pipeline record, a deal took 88 touches across 4 channels, and the first LinkedIn ad impression came a mean 281 days before revenue. Last-click reports undercount every channel that works early ([measurement](references/measurement.md#the-attribution-gap)).
5. **Organic social reach per post is small and moving.** Across 52M+ posts, Instagram's median engagement rate fell 26% from 2024 to 2025, LinkedIn's 5%, while X's and Facebook's rose; LinkedIn carousels earned about three times the engagement rate of LinkedIn video (Buffer, 2026). Different vendors' rates use different denominators and never compare ([benchmarks](references/benchmarks.md)).
6. **Consistency beats bursts, observationally.** Buffer users who posted in 20 or more of 26 weeks got about five times the engagement per post of those who posted in 4 or fewer (100K+ accounts, January 2025). On LinkedIn, an account's busier weeks brought more impressions per post, not fewer (2M posts, 94K accounts, August 2025). Neither is a controlled test ([content](references/content.md#cadence)).
7. **Repurpose your own work; reposting others' is penalised.** Meta cuts reach and monetisation for accounts that reuse others' content without meaningful change, and says a watermark or stitched clips do not count (July 2025). YouTube calls mass-produced and templated uploads inauthentic and demonetises them (July 2025) ([repurposing](references/repurposing.md)).
8. **Product Hunt is an editorial shelf now.** Per its changelog, moderators choose what is featured above the fold, the leaderboard ranks by points (mostly upvotes, with comments) since about December 2025, and launches go live at 12:01 a.m. Pacific on a date-only schedule. About 32 launches a day were featured from mid-September to mid-October 2026, with the day's top product near 430 votes (hunted.space). Every published visitors-per-launch figure is a consultant's estimate ([launch](references/launch.md#what-a-launch-yields-now)).
9. **Channel CAC benchmarks have no method behind them.** The most-quoted CAC-by-channel tables come from one agency's clients with no sample size; paid-search benchmarks give no campaign count. Use them for orders of magnitude and replace them with the user's own numbers within 90 days ([benchmarks](references/benchmarks.md)).

**Does not hold up:** "be everywhere" (no evidence; a small team cannot reach a readable sample on five channels); "AI search traffic converts 23x better" (one site, 30 days); "organic social is dead" (engagement rates rose on X and Facebook in 2025); "post at the best time" (no tool sees when an audience is online; only the account's own analytics do); "launch on Product Hunt for thousands of signups"; "repurpose by posting the same file everywhere" (watermarks and duplicates are demoted).

**Unknown:** how many channels a small team should run (no controlled study; the case for two or three rests on effort and sample size); how much an AI answer contributes to a sale; what a launch adds after 30 days; the share of signups that come through untracked "dark social".

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| Which channels should we be on? Where is our audience? What changed in the channel mix? | [Channel selection](references/channel-selection.md) |
| How do we grow? A growth or marketing plan for the next 90 days | [Growth plan](references/growth-plan.md) |
| A go-to-market plan for a new product: ICP, positioning, motion, channels | [Go-to-market](references/gtm-plan.md) |
| A launch on a date, Product Hunt, Show HN, where to post it, creators and press for launch week | [Launch](references/launch.md) |
| What did people say since we launched? | [Reaction report](references/reaction-report.md) |
| What should we post? A content calendar, pillars, how often to post | [Content](references/content.md) |
| Turn this video, podcast or post into posts for other channels | [Repurposing](references/repurposing.md) |
| What is working? Attribution, dark social, is this change real, AI traffic in analytics | [Measurement](references/measurement.md) |
| What is a good CAC, conversion rate or engagement rate for a channel? | [Benchmarks](references/benchmarks.md) |
| What has Manifold seen in its own runs? | [Field notes](references/field-notes.md) |

## Using Manifold

- **A cross-channel baseline** costs about 130 credits: `seo_search_keywords` on the category (10 for 100 rows; charged on rows returned), `aeo_run_ai_answers` on two buyer prompts (27 a prompt, read with `get_task`), `reddit_search_subreddits` (1) then `reddit_get_new_posts` on the top communities (1 per subreddit per page), `tiktok_search_videos`, `youtube_search_videos` and `linkedin_search_posts` (1 a page), `ads_get_advertiser_ads` per competitor (1), and for B2B `leads_search_companies` (1 at `limit: 1`; `meta.rows_available` sizes the market). [Growth plan](references/growth-plan.md#baseline) has the calls in order.
- **Read the data right.** Subtract competitors' brand names before summing keyword demand (in our test they were 81% of a category's volume). One page of `reddit_get_new_posts` covered nine hours to three days of a busy subreddit, not seven days: pass `pages: 3` and read `coverage[]`. On Google ads `active` comes back null, so judge a running ad by `last_shown`; a domain lookup also returns other advertisers' ads, so group by advertiser. Platform searches are ranked samples, never counts. See [field notes](references/field-notes.md).
- **What the tools cannot see:** conversions, revenue, Product Hunt, Hacker News, Slack, Discord, newsletters, X search and private groups. Signups and sales come from the user's analytics (`analytics_get_report`, free) and CRM; branded search from `console_get_search_analytics` (free) or `seo_get_keyword_metrics`.
- **Budget.** Say the estimate before the first paid call, pass `max_credits` when the user names a budget, and use `dry_run: true` to price a call for free. Results already paid for are free while cached (7 days for most search data, 6 hours for platform searches, never for `reddit_get_new_posts`).
- **Never post, send, schedule, submit, vote or contact anyone** on the user's behalf. The deliverable is a plan or a table; the host keeps it, since the server keeps no state.

## Related skills

- Search: [seo](../seo/SKILL.md), [technical-seo](../technical-seo/SKILL.md), [link-building](../link-building/SKILL.md). AI answers: [ai-search](../ai-search/SKILL.md).
- Communities and social: [reddit](../reddit/SKILL.md), [linkedin](../linkedin/SKILL.md), [tiktok](../tiktok/SKILL.md), [instagram](../instagram/SKILL.md), [youtube](../youtube/SKILL.md), [facebook](../facebook/SKILL.md), [x-twitter](../x-twitter/SKILL.md).
- Paid: [paid-ads](../paid-ads/SKILL.md). Sales-led: [outbound](../outbound/SKILL.md). Creators and press: [creators](../creators/SKILL.md), [digital-pr](../digital-pr/SKILL.md).
- The inputs to a plan: competitors and positioning in [competitor-intel](../competitor-intel/SKILL.md), customers' words and pains in [voice-of-customer](../voice-of-customer/SKILL.md), demand and market size in [market-research](../market-research/SKILL.md). Awareness and mentions over time: [brand-monitoring](../brand-monitoring/SKILL.md). Plans and reports for clients: [agency](../agency/SKILL.md).
