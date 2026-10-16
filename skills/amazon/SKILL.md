---
name: amazon
description: When the user sells on Amazon or plans to, and asks how to rank, what a listing should say, what rivals charge, who holds the Buy Box, or how big demand is on Amazon. Holds dated, sourced findings a model lacks (the 75-character title cap and Item Highlights from July 2026, Rufus renamed Alexa for Shopping and picking products that do not rank, the Buy Box eligibility gate removed, reviews split across variations, 2026 fees, Amazon blocking ChatGPT) and how to read Amazon search volume and results data without overclaiming. Also use when the user mentions Amazon SEO, A9 or A10, Amazon keyword research, backend search terms, a listing audit, product titles, bullets or main image rules, an ASIN teardown, rival listings, resellers undercutting us, Vine, getting reviews, Sponsored Products, ACoS, Brand Analytics, an Amazon launch, or a 30-60-90 day Amazon plan. Google Shopping and feeds go to google-shopping, other ad platforms to paid-ads, review mining to voice-of-customer, market sizing to market-research.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Amazon

How products get found, chosen and bought on Amazon as the evidence stood in October 2026, and how to read Amazon's public results and listings without claiming more than they show. A model's picture of Amazon is mostly from 2023 and 2024 and is wrong in places: titles are capped at 75 characters, Amazon's assistant (now Alexa for Shopping) sits in the search bar and recommends products that often do not rank, the Buy Box no longer screens sellers first, and reviews stopped pooling across unlike variations. "A10" is seller folklore, not an Amazon spec. Prefer what is written here to what you remember, and re-check any policy or fee older than six months in Seller Central before acting on it.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the product, its ASINs, price, margin, marketplace, whether the user is the brand owner (Brand Registry) or a reseller, fulfilment (FBA or merchant), and competitors with their ASINs from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Reading live results pages, listings, sellers and Amazon search volume needs the Manifold connector: look for `amazon_search_products`, `amazon_get_product`, `amazon_get_sellers` and `amazon_get_keywords` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill, use the by-hand methods in the references (a logged-out private window on the right marketplace, the user's own Seller Central and Brand Analytics exports), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. Never invent a measurement.
- **The user's own numbers are not in Manifold.** Sales, sessions, conversion, Brand Analytics, ad spend and ACoS live in Seller Central and the Amazon Ads console. Ask for exports; never estimate them from public data and present the estimate as theirs.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Titles are 75 characters now.** From 27 July 2026 every category except media caps titles at 75 characters with spaces; Amazon's AI rewrites longer titles gradually, and brand owners get 14 days to review. The new Item Highlights field (125 characters) is searchable and shows with the title (Amazon Seller Forums, updated 10 June 2026). The January 2025 rules still hold: no `! $ ? _ { } ^ ¬ ¦` outside a brand name, no word more than twice. The rewrite is slow: on 16 October 2026, 13 of the top 40 organic titles on two US searches were still over 75 characters, up to 199 (Manifold [field notes](references/field-notes.md)), so a rival's long title is not evidence the rule is dead ([listing audit](references/listing-audit.md)).
2. **Amazon documents only the broad ranking inputs.** Text match, price, availability, selection and sales history are the factors Amazon names; its search team trains on clicks and purchases and optimises for purchase and relevance. COSMO (SIGMOD 2024) maps products to uses and audiences, so a listing is matched on what it is for, not only the words typed. "A10" weights and timings are vendor guesses ([search ranking](references/search-ranking.md)).
3. **The assistant does not read the rankings.** Rufus became Alexa for Shopping on 13 May 2026 and answers from the main search bar; Amazon credits Rufus with nearly $12B of incremental 2025 sales, by its own attribution. Of 12,810 assistant picks, 36.1% ranked in the organic top 10 for the matching search, 40.9% never showed on the results page, 14.3% were advertised there (Autopilotbrand via Marketplace Pulse, May to June 2026, one account). Picks were almost never below 4.0 stars (Amalytix, 1,300+ picks, 2024).
4. **Ads take a large share of the page.** About 30% of results were sponsored and an ad sat above the first organic result on 85% of first pages, usually pricier and lower rated (Dash and others, AIES 2024, 4,800 searches). Category moves it a long way: inline sponsored rows were 2% of a US standing desk page and 23% of a protein powder page in Manifold's own reads (October 2026). Sponsored Products spend rose 38% year on year in Q2 2026 with CPC up only 1% (Tinuiti). Existing campaigns serve inside Alexa for Shopping automatically ([ads](references/ads.md)).
5. **The Buy Box stopped screening sellers first.** From July 2026 (US first, all stores by year end) offers go straight into the featured-offer comparison on landed price, delivery speed, stock and performance; the cheapest offer does not automatically win. Amazon can withhold the featured offer from a price out of line with prices elsewhere, the conduct at the centre of the FTC's case, trial set for 29 March 2027 ([pricing and the Buy Box](references/buy-box.md)).
6. **Reviews split by variation.** From 12 February to 31 May 2026 Amazon stopped sharing reviews between child ASINs that differ in function, materials or use; colour, size and pack count still share. A family can now show different stars per child in search. The FTC's review rule carries penalties up to $53,088 per violation, and its first warning letters went to about ten companies on 22 December 2025 ([reviews](references/reviews.md)).
7. **AI answers off Amazon barely cite Amazon.** Amazon's robots.txt blocks OpenAI's search bots (November 2025) and other AI agents, and a court blocked Perplexity's Comet from shopping there (March 2026). In 391K citations on US shopping questions, ChatGPT cited amazon.com 6 times and Perplexity never (LLM Pulse, July 2026, vendor). Amazon also left US Google Shopping ads in July 2025 and has not returned ([AI answers](references/ai-answers.md)).
8. **Every third-party Amazon search volume is an unaudited model.** No independent accuracy test exists; vendor tests grade themselves against different Amazon metrics, and DataForSEO, behind Manifold, publishes no method. Its Amazon volumes ran at a median 0.43 of its own Google volumes on 15 product terms (range 0.04 to 1.19; field notes), so the two do not share a unit. Amazon's own counts sit in Brand Analytics, for brand owners only ([keyword research](references/keyword-research.md)).
9. **Fees rose by small averages and a surcharge.** FBA fees rose an average $0.08 a unit from 15 January 2026 with no new fee types (Amazon, 15 October 2025), then a "temporary" 3.5% fuel and logistics surcharge on FBA fees from 17 April 2026, about $0.17 a unit on average in the US, with no end date ([fees](references/fees.md)).

**Does not hold up:** "Amazon's A10 algorithm weighs X at Y%" (Amazon publishes no such thing); "repeat the main keyword in the title"; "rank top 3 and the assistant will recommend you"; "the lowest price wins the Buy Box"; "Amazon is the default product source in ChatGPT"; "the 'bought in past month' badge is unit sales" (it counts customers, in buckets, and only above a threshold); "tool X's Amazon volume is 93% accurate" (a vendor grading itself against a different Amazon metric).

**Unknown:** what share of Amazon searches now go through Alexa for Shopping; whether outside traffic lifts organic rank; whether A+ content is indexed for search; whether a new listing gets a "honeymoon" boost; how accurate any Amazon volume or sales estimate is.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| How does Amazon rank products? What is A10, COSMO, Rufus or Alexa for Shopping? | [Search ranking](references/search-ranking.md) |
| Which keywords should our listing target? How many people search for X on Amazon? Is there demand? | [Keyword research](references/keyword-research.md) |
| Audit our listing: title, bullets, Item Highlights, backend terms, images, variations | [Listing audit](references/listing-audit.md) |
| Tear down a competitor's listing, price and keywords; who competes with our ASIN? | [Competitor teardown](references/competitor-teardown.md) |
| Who holds the Buy Box? Are resellers undercutting us? Is Amazon selling it? | [Pricing and the Buy Box](references/buy-box.md) |
| How do we get reviews? Vine? Is this review tactic legal? | [Reviews](references/reviews.md) |
| Who advertises on our searches? What do Amazon ads cost? Sponsored prompts? | [Ads](references/ads.md) |
| Do ChatGPT, Perplexity or Google's AI show our Amazon products? | [AI answers](references/ai-answers.md) |
| What do FBA and referral fees cost now? Does the product make money? | [Fees](references/fees.md) |
| We are launching on Amazon: a 30-60-90 day plan | [Launch plan](references/launch-plan.md) |
| What do Manifold's Amazon fields mean and where do they mislead? | [Reading the data](references/data.md) |
| What has Manifold seen in its own Amazon calls? | [Field notes](references/field-notes.md) |

## Using Manifold

- **A results page:** `amazon_search_products` (2 credits per 100 results, cached 1 day) gives organic and sponsored rows in order, with price, rating, `bought_past_month`, badges and `related_searches[]`. The country in `location` picks the marketplace.
- **One listing:** `amazon_get_product` (2, cached 1 day) for title, price, rating, specs (with the current Best Sellers Rank and Item Highlights as rows), variations and images; `amazon_get_sellers` (2, cached 6 hours) for every offer, rank 1 holding the Buy Box, and `has_featured_offer` false (rows from rank 2) meaning no featured offer.
- **Keywords and competitors:** `amazon_get_keywords` (5 + 5 per 100 rows, rounded up on rows returned) for what an ASIN ranks or advertises on; `amazon_get_competitors` (same price) for ASINs sharing its searches; `amazon_search_keywords` (same) for ideas from Amazon's related searches; `amazon_get_keyword_metrics` (5 + 5 per 100 keywords) for volume on a list. All four cached 7 days.
- **Typical jobs:** keyword research about 95 credits, a listing audit about 35, a competitor teardown about 80 for three rivals, a Buy Box check 2 per ASIN, a launch plan about 150. Say the estimate first, pass `max_credits` when the user sets a budget, and price any call free with `dry_run: true`.
- **Data traps:** no tool returns review text, Best Sellers Rank history, unit sales or ad spend. On results rows `rank` counts organic and sponsored together, one ASIN can appear as both, banners and video ads are not rows, and `ratings_count` is rounded. `bought_past_month` is the badge's floor, per child, and null when no badge shows. On a listing `regular_price` is the struck-through list price; there is no variation range. `amazon_get_keywords` rows come biggest search first, with `rank` the page position at the index's last crawl. Keyword and competitor rows come from an index about two weeks old, so confirm a position that matters on the results page. Volumes compare keywords with each other, never as counts ([reading the data](references/data.md)).
- **Never buy, review, message sellers, file complaints or change listings.** Deliver tables the user acts on in Seller Central.

## Related skills

- Google Shopping, merchant feeds, prices across other stores and ChatGPT's product picks: [google-shopping](../google-shopping/SKILL.md). Brand mentions and citations in AI answers beyond products: [ai-search](../ai-search/SKILL.md).
- Meta, Google, TikTok and ChatGPT ads, and creative briefs: [paid-ads](../paid-ads/SKILL.md). Creators, affiliate links and Amazon storefront creators: [creators](../creators/SKILL.md).
- Mining what buyers say in reviews and comments: [voice-of-customer](../voice-of-customer/SKILL.md). A rival beyond Amazon: [competitor-intel](../competitor-intel/SKILL.md).
- Market size and whether demand exists beyond Amazon: [market-research](../market-research/SKILL.md). The brand's own site in Google: [seo](../seo/SKILL.md). Where Amazon fits in the channel mix: [growth-strategy](../growth-strategy/SKILL.md).
