---
name: paid-ads
description: When the user wants to plan, research or brief paid ads on Meta, Google, LinkedIn, TikTok or ChatGPT. Holds dated, sourced findings a model would not know from training (Meta's Andromeda and why creative diversity now does the targeting, Performance Max controls and AI Max results, ads in Google AI Overviews, AI Mode and ChatGPT with reported prices, CPC and CPM benchmarks from large datasets, brand bidding law) and how to read ad library data without overclaiming. Also use when the user mentions a paid ads strategy, Meta or Google ads, where to spend an ad budget, PPC, paid social, whether LinkedIn ads are worth it, the Meta, TikTok or LinkedIn ad library, the Google Ads Transparency Center, a swipe file, winning ads in a niche, competitors' ads, PPC or negative keywords, CPC, who bids on our brand, conquesting, an ad creative brief, ad fatigue, Advantage+ or Performance Max. Creators who make the ads go to creators, a competitor beyond its ads to competitor-intel, which channels at all to growth-strategy.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Paid ads

How paid ads are bought and judged as of October 2026, and how to read the public ad libraries without claiming more than they show. A model's memory of paid ads is mostly 2023 and 2024 and is wrong in places: on Meta the creative now does the targeting, Performance Max has negatives and search terms, Dynamic Search Ads began upgrading to AI Max in September 2026, ads run inside ChatGPT and Google's AI answers, and the TikTok ad library covers Europe only. Prefer what is written here to what you remember, and re-check any price or policy older than six months before quoting it to a client.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the product, price, buyer, competitors with their pages and domains, market and ad budget from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Reading the ad libraries and keyword prices needs the Manifold connector: look for `ads_get_advertiser_ads`, `ads_search_ads`, `ads_get_ad` and `seo_search_keywords` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill, read the libraries by hand (Meta Ad Library, TikTok's library, LinkedIn's Ad Library, Google's Ads Transparency Center) with the rules in [ad libraries](references/ad-libraries.md), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Ad libraries show survival, never results.** No library publishes clicks, conversions or spend for commercial ads, or the keywords a search ad bids on. An ad that has run 30 days and still runs is the best available proxy for a winner, but no study has tested that proxy, and automated campaigns keep weak ads alive at low spend ([ad libraries](references/ad-libraries.md#winners-what-run-length-means)).
2. **EU-shown ads carry far more data.** The Digital Services Act makes platforms keep ads shown in the EU for a year with targeting and reach; US-only ads carry none of that. TikTok's library holds European ads only, so a US-only TikTok advertiser is invisible, and it lagged about three weeks in our October 2026 run ([field notes](references/field-notes.md)).
3. **On Meta, the creative set is the targeting.** Andromeda retrieves ads for people by their content (+6% recall, +8% ads quality, Meta, December 2024), and Meta's guidance is creative diversification: distinct concepts, not variants of one. Brief concepts first ([platforms](references/platforms.md#meta-the-creative-is-the-targeting), [ad briefs](references/ad-brief.md)).
4. **AI Max adds volume at a higher price.** Google claims 14% more conversions at similar CPA; an independent read of 250+ campaigns found +13% conversion value, +16% CPA and ROAS swinging from +42% to -35% (Smarter Ecommerce, March 2026). DSA campaigns upgrade to AI Max automatically from September 2026, and only AI Max or Performance Max can serve in AI Mode.
5. **Performance Max has controls now.** Campaign-level negatives (up to 10,000), search terms, channel and asset reporting arrived in 2025. An unexamined PMax campaign is a choice, not a limitation.
6. **ChatGPT ads are real but thin.** US test from February 2026, self-serve and CPC bidding from May, nine markets by September. Reported CPMs fell from $60 to about $25; US CTR 0.73% against 6.64% on Google Search (Similarweb, Graphite, WordStream, 2026). Paid ChatGPT tiers, including company seats, see no ads, which removes most B2B buyers ([AI answer ads](references/ai-answer-ads.md)).
7. **Prices moved apart.** Google Search median CPC $5.42 and cost per lead $66.69, the first fall in cost per lead in about five years (WordStream, 13,474 campaigns, to March 2026); Meta CPM about $14 to $15 after a roughly 20% rise in 2025; TikTok CPM $4.08, down 28%, but conversion rate down 37% (Triple Whale, 5,900 brands). LinkedIn clicks cost several times Meta's yet returned the most revenue per B2B ad dollar in Dreamdata's data, over a 272-day journey ([benchmarks](references/benchmarks.md)).
8. **A brand campaign pays mainly when someone else bids.** eBay's field experiment found no short-term benefit from brand-keyword ads with no rivals (Econometrica, 2015). Bidding on a rival's trademark as a keyword is allowed; using it in ad text can be restricted after a complaint ([brand bidding](references/brand-bidding.md)).

**Does not hold up:** "a competitor with hundreds of ads is winning" (count long-runners, not ads); "no ads in the library means they don't advertise" (lag, regions, other accounts, Europe-only TikTok); "narrow interest targeting beats broad on Meta" (retrieval works from the creative); "test ten variants of one ad"; "the library shows spend" (only for political ads); "use benchmarks as targets" (the user's allowable cost per result decides); "the SERP shows who bids on our brand" (our result pages returned no ads at all; use Auction Insights).

**Unknown:** whether ads in ChatGPT or AI Overviews are incremental to search ads; how many concepts a Meta ad set should carry; whether run length tracks real performance.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| What can an ad library tell us? How do we read run length, reach, the EU data? What does it cost? | [Ad libraries](references/ad-libraries.md) |
| What ads do our competitors run? A swipe file of the category's winning ads? | [Competitor ads](references/competitor-ads.md) |
| What changed on Meta, Google, LinkedIn, TikTok? Advantage+, Andromeda, PMax, AI Max? Which platform first? | [Platforms](references/platforms.md) |
| What does a click or an impression cost? Is our CPA normal? | [Benchmarks](references/benchmarks.md) |
| Should we advertise in ChatGPT, AI Overviews or AI Mode? | [AI answer ads](references/ai-answer-ads.md) |
| Which keywords should we bid on? Negatives? Match types? | [Google search ads](references/google-search-ads.md) |
| Who bids on our brand? Should we run a brand campaign? Trademark rules? | [Brand bidding](references/brand-bidding.md) |
| A creative brief, new angles, our ads are fatigued | [Ad briefs](references/ad-brief.md) |
| A paid ads plan for the next 90 days | [Strategy](references/strategy.md) |
| What has Manifold seen in its own runs? | [Field notes](references/field-notes.md) |

## Using Manifold

- **A competitor's ads:** `ads_get_advertiser_ads` per platform (a Meta page, TikTok advertiser name, LinkedIn company, or a domain on Google), 1 credit a page. **A category's ads:** `ads_search_ads` on Meta, TikTok or LinkedIn, 1 credit a page; Google needs advertisers, found with `ads_search_advertisers` (1 credit). **One ad in full:** `ads_get_ad`, 1 credit. Lists cache 24 hours, ads 7 days.
- **The words of a video ad:** `facebook_get_transcript` or `tiktok_get_transcript`, 1 credit. **Landing pages:** `seo_get_page`, free.
- **Click prices and demand:** `seo_search_keywords` (10 credits for 100 rows) or `seo_get_keyword_metrics` with `source: "google_ads"` for Keyword Planner's own figures (24 flat). Quote `bid_low` to `bid_high` as the price; `cpc` is a modelled average that can sit outside it. **Who buys search clicks:** `paid_traffic` from `seo_get_traffic_estimates`, a modelled estimate, never spend.
- **Data traps:** `active_only` works on Meta only; elsewhere read `last_shown`. On Google take `first_shown` from the list row, since `ads_get_ad` often has none. A Google domain pull mixes in agencies and unrelated advertisers. `seo_get_serp` returned no paid rows in our runs, so it cannot show who bids. Published `impressions` and `spend` are text ranges: quote, never sum.
- **Never buy, launch, pause or edit ads, or ask for ad account access.** The user's own results live in their ad accounts; ask for them.

## Related skills

- Creators who make the ads, UGC and Spark Ads rights: [creators](../creators/SKILL.md). Organic posts on the same platforms: [tiktok](../tiktok/SKILL.md), [instagram](../instagram/SKILL.md), [facebook](../facebook/SKILL.md), [linkedin](../linkedin/SKILL.md), [youtube](../youtube/SKILL.md).
- A competitor beyond its ads, and a recurring watch of its new ones: [competitor-intel](../competitor-intel/SKILL.md). Customer words for hooks: [voice-of-customer](../voice-of-customer/SKILL.md).
- Whether paid belongs in the mix at all: [growth-strategy](../growth-strategy/SKILL.md). Organic keywords and comparison pages: [seo](../seo/SKILL.md). The unpaid answer in ChatGPT and Google's AI: [ai-search](../ai-search/SKILL.md). Ads research for a prospect: [agency](../agency/SKILL.md).
- Shopping ads and product feeds: [google-shopping](../google-shopping/SKILL.md). Sponsored products on Amazon: [amazon](../amazon/SKILL.md). Apple Ads and store search: [app-store](../app-store/SKILL.md).
