# Go-to-market for a new product

A go-to-market plan answers four questions in order: who it is for, why they would pick it over what they use now, how they will hear about it, and how it launches. The order matters: channels chosen before the ICP reach the wrong people, and a launch before the positioning holds spends the one day of attention on an unclear message.

## Settle first

- **Product**: what it does, its stage (idea, beta, ready) and its price or price range.
- **ICP hypothesis**: who the founder thinks buys, stated as a hypothesis to test.
- **Alternatives**: products and workarounds (spreadsheets, an agency, doing nothing). Default the ones the market check finds.
- **Market**: country and language; default United States, English.
- **Launch date**, if there is one; **team and budget**.

## The market check

About 41 credits. It is a sample (one AI prompt, two results pages, one page of Reddit) that points the deeper work; it is not the verdict.

- **Demand:** `seo_search_keywords` on the category (10). Read `volume` without competitors' brand names, and the median of `trend[12]`. Existing demand can be captured; near-zero means a new category where demand is made.
- **Who wins now:** `seo_get_serp` for "best <category>" and "<leading competitor> alternatives" (1 each), and `aeo_run_ai_answers` with "best <category> for <ICP>" and the competitors in `brands` (27), read with `get_task`.
- **The problem in buyers' words:** `reddit_search_posts` on the problem at the default relevance sort (1); keep the rows that are on topic.
- **B2B size:** `leads_search_companies` with the ICP's industries, headcount and locations (1 at `limit: 1`); `meta.rows_available` is an upper bound.

Depth for each lives elsewhere: demand and market size in [market-research](../../market-research/SKILL.md), competitors in [competitor-intel](../../competitor-intel/SKILL.md), the ICP's pains and language in [voice-of-customer](../../voice-of-customer/SKILL.md).

## In order

1. **ICP: one segment to win first.** A plan for "small businesses and enterprises" is two plans. Take the segment where the problem is sharpest in buyers' own words and the alternatives are weakest.
2. **Positioning: one line.** For whom, the problem, the alternative they use now, and why this is better. Test it against the three closest competitors' claims and prices ([competitor-intel](../../competitor-intel/SKILL.md)).
3. **Motion: from the price** ([channel selection](channel-selection.md#price-sets-the-motion)).
4. **Channels: two or three**, each checked against the [signals](channel-selection.md#signals-a-channel-is-worth-it).
5. **Launch: once the ICP and positioning hold**, with a date ([launch](launch.md)).

## Be on the shortlist before launch

B2B buyers rank a shortlist before contacting anyone (94% of about 4,000 buyers), the day-one list averaged 3.6 vendors and the early favourite won 77% of the time (6sense, 2025). A new product that is absent from the places buyers and AI engines consult at the start (category lists, review sites, communities, comparison pages, AI answers) loses deals it never sees. Put the first months of channel work where the shortlist forms, not only where the last click happens; [ai-search](../../ai-search/SKILL.md) covers the AI side.

## The deliverable

A one-page GTM document: the ICP (one segment, with buyers' own words), the positioning line and the three alternatives, the price context and the motion, the channels with the evidence for each, the launch date and plan, KPIs per stage (conversations, trials or signups, customers), and three first actions. Each section links to the topic skill that goes deeper.
