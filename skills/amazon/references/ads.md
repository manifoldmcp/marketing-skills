# Sponsored placements and Amazon Ads

What the sponsored slots on an Amazon results page are, how much of the page they take, what clicks cost, and what Manifold can and cannot see. The user's own spend, ACoS and search-term reports live in the Amazon Ads console; ask for them. Ads on Meta, Google, TikTok, LinkedIn and ChatGPT belong to [paid-ads](../../paid-ads/SKILL.md). Checked October 2026.

## How much of the page is paid

- **About a third of results, and usually the top.** Dash and others (AIES 2024; 4,800 simulated searches on four Amazon marketplaces, 2M organic and 638K sponsored results) found nearly 30% of results sponsored, a sponsored item above the first organic result on about 85% of first pages, items ranking beyond 100th organically appearing as sponsored on page one, and top sponsored items usually pricier and lower rated than top organic ones.
- An older layout count found only four organic products among the first twenty a shopper saw (Marketplace Pulse, March 2022, no method published). Category matters: brand-crowded categories carry far more ads than staples.
- **The money:** Amazon's ad revenue was $68.6B in 2025, up 22%, mostly Sponsored Products (Amazon results, February 2026).

## Formats

- **Sponsored Products:** single products in the results and on detail pages, cost per click on keywords or product targets. These are the `sponsored` rows in Manifold.
- **Sponsored Brands:** the brand banner, store spotlight and video in results; Brand Registry only. Amazon said advertisers averaged only about 62% top-of-search impression share on their own branded queries with Sponsored Brands (unBoxed 2025, Amazon's figure), the pitch for its reserved share-of-voice product.
- **Sponsored Display and the DSP:** audiences on and off Amazon. Amazon began selling ChatGPT ad inventory through its DSP to some US advertisers in September 2026 (Digiday, as reported).
- **In Alexa for Shopping:** existing Sponsored Products and Sponsored Brands campaigns serve inside the assistant's conversations automatically, and Amazon turns them into sponsored prompts, suggested questions on results and product pages that open a conversation about the brand (Amazon Ads, 11 June 2026). Trade press reports prompts became billable at normal CPC on 25 March 2026 with a prompts report in the console. Amazon claims about 20% of shoppers who touch a prompt keep talking about the brand and a 6% conversion lift when prompts are added to Sponsored Brands; Amazon's own numbers. Volume was tiny in one reported account (88 clicks against 500,000 from all its Amazon ads, The Information, as reported). Being advertised barely predicted the assistant's organic picks (14.3% of picks, see [search ranking](search-ranking.md)).

## What clicks cost

Benchmarks vary by category, attribution window and whether they are means or medians; use them to sanity-check the user's own numbers, never as targets. The user's margin decides the ACoS they can afford: break-even ACoS is the margin before ad spend.

- **Trend:** US Sponsored Products spend +38% year on year in Q2 2026, clicks +37%, CPC +1%; Sponsored Brands CPCs up double digits five quarters running (Tinuiti, Q2 2026, client data). Pacvue reported Sponsored Products CPC down 3% year on year in Q3 2026 (as summarised; the report is gated).
- **By category:** Q2 2026 US Sponsored Products CPC ran from about $0.55 in clothing, shoes and jewellery to about $2.18 in health and household (Pacvue, as summarised); beauty and personal care $1.57, up 10.6%, with ROAS down 5.7%.
- **Account medians:** ACoS 28.2%, ROAS 3.55, CTR 0.69%, conversion 5.85%, US CPC $0.82 (WisePPC, 69 accounts, April to June 2026, 7-day attribution; the CPC figure rests on 19 US accounts; vendor, small sample). The middle half of accounts ran ACoS from about 16% to 63%.
- Unsourced figures circulate widely ("average CPC $1.94", "target 4x ROAS"): drop them.

## Reading sponsored rows in Manifold

- `amazon_search_products` marks each row `type` organic or sponsored. Rows are inline products; brand banners, video ads and sponsored carousels are not rows, so the page carries more paid space than the rows show. In Manifold's reads on 16 October 2026 inline sponsored rows were 2 of 100 on "standing desk" and 14 of 60 on "protein powder", 4 of them above the first organic result ([field notes](field-notes.md)): compare terms with each other, not with the 30% in studies.
- A product can hold a sponsored and an organic slot on one page (Orgain at sponsored 1 and organic 16). That is the brand paying above its own organic listing; count it when judging how much of the page a rival holds.
- Count advertisers per term: the brands in sponsored rows on the user's core terms, how high they sit, and whether the user's own brand terms carry a rival's ads. One page is one moment, one marketplace, logged out; ads rotate by time, budget and shopper. Act on an advertiser seen on two or more terms or two runs.
- `amazon_get_keywords` rows with `type` sponsored are searches where the ASIN was seen advertising when the index crawled; `sponsored_keywords` on `amazon_get_competitors` counts them. That is breadth, never spend or bids. Amazon's suggested bids are in the user's ads console; Manifold has no Amazon bid data.
- Nothing here shows impressions, clicks, spend or performance of anyone's Amazon ads.

## Brand defence

If rivals sit in sponsored rows on the user's brand name, the case for a branded Sponsored Brands or Sponsored Products campaign is stronger; with no rivals there, it mostly buys clicks the organic listing would have had. The evidence on brand bidding in general is in [paid-ads](../../paid-ads/SKILL.md).

## Never

Launch, pause, bid on or edit campaigns, or ask for ad account access. Deliver the keyword list, the competitor table and the reasoning; the user runs the account.
