---
name: brand-monitoring
description: When the user wants to know who is talking about the brand, be alerted to new mentions, or measure how well known the brand is against competitors. Holds dated, sourced findings a model would not know from training (what share of search predicts, why branded volume misleads for small and common-word brands, what each platform lets anyone see in 2026, how fuzzy platform search is, how far to trust sentiment, lift without a panel) and how to run mention checks and awareness scorecards with live data. Also use when the user mentions alert me when someone mentions us, monitor Reddit for our brand, social listening, brand monitoring, mention alerts, track mentions every day, who talked about us this week, a brand awareness plan, nobody knows who we are, share of voice against competitors, share of search, grow branded search, brand lift, get our name out there, or getting the brand known. AI answers alone go to ai-search, rivals' moves to competitor-intel, client reporting to agency.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Brand monitoring

Who is talking about the brand, how many people know it, and how to tell a real change from noise, as the evidence stood in October 2026. What a model likely has wrong: it treats a social listening tool as seeing everything (no tool does; the platforms closed most of their doors in 2024 to 2026), treats platform search as exact (TikTok and YouTube match loosely enough that most rows for a short brand name are about something else), quotes share of search as "83% accurate" (it is an average ratio, not an accuracy), and reads branded search volume as a weekly number (it is bucketed, monthly and a month or more behind). Prefer what is written here to what you remember, and re-check any platform fact older than six months.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the brand, product and founder names, the domain, the competitors, the audience and the brand's own social accounts from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Watching mentions and measuring awareness live needs the Manifold connector: look for `reddit_get_new_posts`, `reddit_search_comments` and `seo_get_keyword_metrics` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill and use the by-hand methods in [monitoring](references/monitoring.md#without-the-connector) and [share of search](references/share-of-search.md#without-the-connector), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. If some tool groups are present and others are not, they are switched off on the app's Tools page: mark those platforms "not watched" or "not measured", never zero. Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Share of search tracks market share, and leads it, as a share, not a volume.** A brand's searches over all brands' searches in the category correlated with and led market share in cars, energy and phones (Binet, IPA, October 2020; up to about a year ahead for cars, weeks for energy). Across 30 cases in 12 categories and 7 countries it averaged 83% of share of market, with the ratio varying by category (IPA think tank, October 2021). The "83% accuracy" in vendor posts is a misreading. Smooth it over 6 to 12 months ([share of search](references/share-of-search.md)).
2. **Branded volume is bucketed and late.** Google's volumes for brand names move in fixed steps (1,600, 1,900, 2,400 and so on), so one step is a 20% to 25% move and a smaller change cannot show; Manifold's default source ends one to two months before the call. In our run the Keyword Planner source matched it exactly and added one month, for 24 credits against about 11 ([field notes](references/field-notes.md)). Read branded search quarterly, never weekly.
3. **Common words and small brands break it.** "linear" shows 90,500 searches a month, "linear app" 6,600; "folk" 60,500, "folk crm" 1,900. Kantar found a naive brand-name list missed about 36% of real brand search interest (August 2025), so qualifiers cost real searches too. A brand too small for keyword tools still shows in its own Search Console: "manifold mcp" was null in both keyword sources and had 21 impressions in 90 days.
4. **AI answers now drive branded search.** Brands ChatGPT recommended were 2.5 times as likely to get a visit within 7 days, and 55.9% of those visits came through a branded search (Similarweb, June 2026, US desktop, three verticals, correlational). Branded search now partly measures AI visibility, and AI answers are an awareness surface of their own ([awareness](references/awareness.md), [ai-search](../ai-search/SKILL.md)).
5. **No listening tool sees everything, by design.** X moved to pay-per-use in February 2026; Reddit closed self-service API access on 11 November 2025 and sued scrapers in October 2025; CrowdTangle closed in August 2024; TikTok's Research API excludes commercial users; Threads keyword search needs approval and caps queries. Every vendor's coverage is partial and none is complete for comments, private groups or Stories ([coverage](references/coverage.md)).
6. **Platform search matches loosely.** In our runs TikTok returned 7 videos for "attio" and none were about Attio; YouTube returned 15, of which 13 were about "80" and 2 were the brand's own uploads. A two-word query ("attio crm") fixed YouTube (20 of 20) but not TikTok (6 of 30). Keep only rows whose text holds the term, and drop the brand's own accounts ([monitoring](references/monitoring.md#reading-the-rows)).
7. **Watching is complete only as far as a page reaches.** One page of a busy subreddit covered 8 hours of r/SaaS, 2 days of r/sales and 6 days of r/CRM. A search page is a sliver: Reddit search for "hubspot" over a week returned 7 posts, 1 of the 23 HubSpot posts in five watched communities, yet it found the week's biggest thread, in a community nobody was watching. Use both ([monitoring](references/monitoring.md)).
8. **Sentiment scores are the weakest number in a listening report.** Large models do well on plain positive or negative text but lag on aspect-level and structured sentiment (Zhang et al., 26 datasets, 2023), and sarcasm remains hard (SarcasmBench, 11 models, 2024). No public benchmark measures sentiment on brand mentions. Classify what the mention asks for, not a score ([sentiment](references/sentiment.md)).
9. **Lift without a panel means a geo holdout.** Meta's GeoLift asks for at least 20 geo units, 25 pre-periods and four to five times the test length in stable history; Google's Brand Lift needs a spend minimum and an account rep. Before and after on one market proves little ([brand lift](references/brand-lift.md)).

**Does not hold up:** "share of search predicts market share with 83% accuracy"; "followers measure awareness" (they measure the brand's own reach); "our tool monitors all of social" (no tool has X, private groups, Stories or full comment coverage); "a sentiment dashboard tells you how people feel"; "branded search went up 10% this month" (inside one bucket, it cannot be measured); "alert on every mention" (it buries the few that need an answer).

**Unknown:** share of search's lead time for B2B software and small brands (the published work is consumer categories); how far AI answers will reshape branded search over time; how accurate any model's sentiment is on brand mentions.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| Alert me when someone mentions us. What is new this week? How often should it run? What does it cost? | [Monitoring](references/monitoring.md) |
| Which platforms can we watch? Why is X or Facebook missing? What do listening tools really cover? | [Coverage](references/coverage.md) |
| How known are we against competitors? Which measures are weak? A 90-day awareness plan | [Awareness](references/awareness.md) |
| Share of search, branded search volume, Google Trends, is our brand search growing? | [Share of search](references/share-of-search.md) |
| Did the campaign raise awareness? Can we measure brand lift without a survey panel? | [Brand lift](references/brand-lift.md) |
| Is the conversation positive? Can we trust sentiment? Which mentions need an answer? | [Sentiment](references/sentiment.md) |
| What has Manifold seen in its own runs? | [Field notes](references/field-notes.md) |

## Using Manifold

- **New mentions, complete:** `reddit_get_new_posts` on up to 10 subreddits with `match` set to the terms (1 credit a subreddit a page, never cached, at most 7 days back). Read `coverage[]` every run. It reads post titles and bodies, not comments.
- **New mentions, ranked samples:** `reddit_search_posts` and `reddit_search_comments` (1 credit a page, cached 1 hour), `tiktok_search_videos`, `youtube_search_videos`, `linkedin_search_posts` (1 credit a page, cached 6 hours), `instagram_search_posts` by hashtag only, `facebook_get_group_posts` for groups the user names. X has no keyword search; `twitter_get_tweets` reads one account. A daily check on five subreddits and two terms is about 16 credits, 480 a month.
- **Awareness:** `seo_get_keyword_metrics` with the brand and competitor names and `ai_volume: true` (about 11 credits for a dozen names); `console_get_search_analytics` with a query filter for the user's own branded impressions (free); `aeo_run_ai_answers` for AI share of voice (27 credits a prompt on five engines); the profile tools for followers (1 credit each); `seo_get_backlink_summary` for referring domains (10 each). A four-brand scorecard is about 160 credits.
- **Read the data right:** filter every search row on the term and on `created_at` yourself (comment search ignored `time_range` in our run); dedupe on `id`, never dates (`created_at` on YouTube and LinkedIn is approximate); drop the brand's own accounts; compare brands only on the same queries run the same day; read `ai_volume` as a share of the set, since the whole index drifted down by about a third in a year. `dry_run: true` prices any call for free; pass `max_credits` on every scheduled call.
- **The server keeps no history and sends nothing.** The host stores state and runs the schedule; replying to a mention is the user's call.

## Related skills

- Mentions and citations in AI answers: [ai-search](../ai-search/SKILL.md). What rivals launch, charge and say: [competitor-intel](../competitor-intel/SKILL.md).
- Turning mentions into client reports: [agency](../agency/SKILL.md). Complaints as research rather than alerts: [voice-of-customer](../voice-of-customer/SKILL.md).
- Answering a Reddit thread a run found, and choosing communities: [reddit](../reddit/SKILL.md). Mentions without a link: [link-building](../link-building/SKILL.md). Press coverage: [digital-pr](../digital-pr/SKILL.md).
- Search Console and keyword data in depth: [seo](../seo/SKILL.md). Launch-day reaction: [growth-strategy](../growth-strategy/SKILL.md).
