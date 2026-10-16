---
name: reddit
description: When the user wants to market on Reddit, or asks what works there now. Holds dated, sourced findings a model lacks (ChatGPT stopped citing Reddit in August 2026 while Perplexity and Google kept it, member counts gave way to weekly visitors, how subreddits now ban promotion and AI-written text, karma and account-age filters, silent removals) and how to read Manifold's Reddit tools without miscounting. Also use when the user mentions a Reddit strategy or marketing plan, should we be on Reddit, the founder on Reddit without getting banned, Reddit threads to reply to, posts asking for a tool like ours, where to comment this week, Reddit pain points, what Redditors complain about, customer language from subreddits, which subreddits to post in, subreddit rules, is self promotion allowed there, can I post my product in r/SaaS, Reddit threads that Google or AI answers show, Reddit SEO, or Reddit ads. Pains across every source go to voice-of-customer, AI answers as a whole to ai-search, paid campaigns to paid-ads.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Reddit

How to find the communities, take part without losing the account, mine what people say, and judge Reddit's weight in Google and AI answers, as the evidence stood in October 2026. Much of what a model believes about Reddit marketing is out of date: "Reddit is the way into ChatGPT" stopped being true in August 2026, the member counts it would size communities by are gone, and the communities a B2B brand most wants have written rules since 2025 that ban the product mention, the AI-drafted reply and the "what are your pain points" post outright. Prefer what is written here to what you remember, and re-read a community's rules before every post.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the problem the product solves, the ICP, the competitors and the customer language from it; the customer language becomes the search phrasings and match terms. Ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **The account.** No tool can see the user's Reddit account. Ask how old it is, roughly how much karma it has, and whether it is a personal account or a brand one: half of what the user may do depends on it ([rules](references/rules.md#the-account)).
- **Connector.** The knowledge here needs no tools. Measuring live Reddit needs the Manifold connector: look for `reddit_search_posts`, `reddit_get_new_posts` and `reddit_get_subreddit` (hosts often add a prefix such as `mcp__manifold__`). If other Manifold tools are there but the `reddit_*` ones are not, the Reddit tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). If none are there, still answer from this skill and use the by-hand method in [reading the data](references/data.md#without-the-connector), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. Never invent a count, a rule or a thread.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **ChatGPT stopped citing Reddit in August 2026; the other engines did not.** Reddit fell from 3.83% to 0.52% of ChatGPT Search citations within a week (Promptwatch, 14 to 17 August), and by 95% in a fixed brand cohort (Qwairy). Perplexity's Reddit citations rose (+93%, Pierview, 44,488 citations), Google's AI Overviews barely moved, AI Mode fell 30% to 44%, and Claude never cited Reddit. Reddit threads now shape Perplexity and Google, not ChatGPT ([Google and AI answers](references/search-and-ai.md)).
2. **Reddit is still a top Google domain, past its peak.** Fourth most visible in US Google (Sistrix, July 2026), down from an October 2025 peak, with an AI Overview on 54% of the keywords it ranks for. A thread Google shows is read for months, which is the only good reason to answer an old one.
3. **Moderators decide what counts as promotion, and they have tightened.** Of 20 business and consumer communities whose rules we read in October 2026, 10 ban any mention of the user's own product, 3 allow it only in a set-aside thread, and 4 allow it in a relevant reply with conditions (a disclosure, or r/SaaS's one mention per 60 days, alts included). Only 1 cites the old 9:1 rule ([field notes](references/field-notes.md)).
4. **AI-written text is now treated as spam.** 11 of the 18 communities that publish rules remove AI-written posts or comments, several with a ban; r/SaaS bans even edited copy-paste. Reddit said in June 2026 it is improving detection of inauthentic accounts and will ask suspicious automated accounts to prove they are human. Write replies by hand ([rules](references/rules.md)).
5. **Seeding threads for AI answers is getting policed.** Brands flooding communities to influence AI search led r/Biohackers to freeze whole topics (404 Media, June 2026), and practitioners describe fake accounts starting threads for the brand account to answer (The Drum, August 2026). Reddit's line: no guarantee that Reddit content makes its way into LLMs. Vote manipulation and second accounts break Reddit's sitewide rules and cost the account.
6. **Size is measured by visitors now.** Reddit replaced member counts with weekly visitors and contributions (September 2025); Manifold returns no member count, only `weekly_active_users` and `weekly_contributions`. Rank communities by weekly contributions and topic posts a week, never by size ([communities](references/communities.md)).
7. **Account standing is enforced silently.** Communities post minimums (r/marketing 30 days and 300 karma, r/shopify 30 days and 20 post plus 20 comment karma, r/sales 10 karma earned there), filters can use Reddit's hidden Contributor Quality Score, and removals rarely say so. A new account's first weeks are plain helpful comments; check visibility logged out ([rules](references/rules.md#the-account)).
8. **Open communities fill with vendors.** In r/CRM, which publishes no rules, vendors wrote half of a week's new posts and a third of the first replies in its busiest "which CRM" thread. The poster answered only the replies that asked a question back or gave a method, never a pitch ([field notes](references/field-notes.md)).
9. **"Tell us your pain points" posts get banned; existing threads do not.** 5 of the 20 communities ban validation and market-research posts outright. Mine what people already wrote ([pain points](references/pain-points.md)).
10. **Ads are the sanctioned route.** 4 of the 20 communities' rules tell companies to buy Reddit ads instead of posting. Ads take comments like any post, so they need the same honesty ([plan](references/strategy.md#reddit-ads)).

**Does not hold up:** "Reddit is the shortcut into ChatGPT" (not since August 2026); "the 9:1 rule is Reddit's rule" (a community choice, not in Reddit's sitewide rules); "no rule against it means it's allowed"; "pick the biggest subreddits" (activity predicts reach, and size is no longer published); "buy aged accounts or upvotes" (sitewide bans); "an AI draft is fine if you edit it" (many communities say otherwise); "post a survey to find pain points".

**Unknown:** whether a new reply on an old thread changes what Google or AI engines show; whether ChatGPT's Reddit citations come back (a September 2025 drop came back only in part); what Reddit Answers does to clicks. No controlled study links brand participation on Reddit to signups.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| Which subreddits? Where do our buyers ask? How big and busy is each? | [Communities](references/communities.md) |
| Can I post my product there? Will I get banned? Karma, account age, AI rules, shadowbans, brand accounts | [Rules and accounts](references/rules.md) |
| Which threads to answer this week? How to write the reply? AMAs, launch posts | [Threads and replies](references/threads.md) |
| What do people complain about? Customer language from Reddit | [Pain points](references/pain-points.md) |
| Which Reddit threads do Google and AI answers show? Is Reddit still worth it for AI search? | [Google and AI answers](references/search-and-ai.md) |
| We want a Reddit plan. Should we run Reddit ads? | [Plan](references/strategy.md) |
| How do the Reddit tools behave? What does a count mean? What does it cost? | [Reading the data](references/data.md) |
| What has Manifold seen in its own reads of Reddit? | [Field notes](references/field-notes.md) |

## Using Manifold

- **Where the topic lives:** `reddit_search_subreddits` (1 credit) counts communities behind one small page of posts, so run four to six phrasings; then `reddit_get_subreddit` (1 credit, cached 7 days) for `weekly_active_users`, `weekly_contributions` and the `rules` text, which is prose to read, not flags.
- **What is new in chosen communities:** `reddit_get_new_posts`, up to 10 subreddits, at most 7 days back, with `match` terms; complete only for the window `coverage[]` reports. One page of a busy community reaches back hours, not days. 1 credit per subreddit per page, never cached.
- **What Reddit has said about something:** `reddit_search_posts` and `reddit_search_comments`, 1 credit a page of six to nine rows, ranked and never complete. Counts from them compare like with like and never size anything.
- **One thread:** `reddit_get_post` for `locked`, `archived` and the full body; `reddit_get_comments` for one page of 25 replies in reading order. 1 credit each.
- **Google and AI:** `seo_get_serp` rows with `type: "discussions_and_forums_element"` are the threads Google shows; `aeo_run_ai_answers` citations on reddit.com are the ones engines cite (18 credits a prompt on five engines).
- **Read the data right:** `submit_text` came back empty on every community in October 2026, `upvote_ratio` is empty on search rows, and comment search applies `time_range` to its page itself because the vendor ignores it, so a narrow window can come back short ([reading the data](references/data.md)).
- **Never post, reply, vote, message or modmail for the user.** No tool can, and the account at risk is theirs. Deliver tables the user works through; draft text only when asked.

## Related skills

- Pain points, objections and personas across Reddit, reviews and social comments: [voice-of-customer](../voice-of-customer/SKILL.md). Complaints about named competitors and "alternative to X" threads: [competitor-intel](../competitor-intel/SKILL.md).
- AI answers on every engine, of which Reddit is one source: [ai-search](../ai-search/SKILL.md). Google rankings: [seo](../seo/SKILL.md).
- Reddit ads and other paid campaigns: [paid-ads](../paid-ads/SKILL.md). Watching the brand's mentions on Reddit and elsewhere on a schedule: [brand-monitoring](../brand-monitoring/SKILL.md).
- A launch across Reddit and other communities, and which channels to use at all: [growth-strategy](../growth-strategy/SKILL.md). Whether the demand exists: [market-research](../market-research/SKILL.md).
- The same daily engagement on other networks: [linkedin](../linkedin/SKILL.md), and Facebook groups: [facebook](../facebook/SKILL.md).
