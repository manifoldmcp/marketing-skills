# AI shopping answers

How products get into the shopping answers of Google's AI Mode, AI Overviews and Gemini, ChatGPT and Perplexity, and how to measure it. Checked October 2026. How each engine picks sources for brand and category questions, and the noise in any answer, are in [ai-search](../../ai-search/SKILL.md); this page is the product side.

## The short version

For a shop, the product feed is the AI search work. Google's AI answers link products to Shopping listings from Merchant Center; ChatGPT has taken most of its product picks from merchant feeds since July 2026; Perplexity takes catalogues through Shopify and PayPal. A shop with no feed in a channel is left to whatever that engine finds on the web.

## Google AI Mode, AI Overviews and Gemini

- **The Shopping Graph feeds them.** AI Mode shopping launched at I/O in May 2025 on Gemini plus the Shopping Graph. In our October 2026 runs, every AI Overview and AI Mode answer to a product prompt linked product names to Google Shopping product pages and cited them as sources, and the Gemini app did on two of four ([field notes](field-notes.md)).
- **Free listings are the gate.** Products appear in AI Mode without Shopping ads; Google's advice for AI visibility is free listings requirements met, shopper terms in titles and descriptions, missing attributes filled. Eligibility does not guarantee a showing.
- **Google's own measure:** AI performance insights in Merchant Center (announced 27 May 2026; seen in pilot accounts from 14 July) shows share of voice against Merchant Center's competitor set, products showing for top terms, attributes and intents, by stage (discovery, evaluation, ready to buy). English queries in the US, Canada, Australia, India and New Zealand; organic traffic only, ads excluded; competitors cannot be chosen; a few days' lag ([Google help](https://support.google.com/merchants/answer/17200695)). Ask the user for it when they have it; no Manifold tool reads it.
- **Checkout moved into the answer.** Agentic checkout ("buy for me" when a tracked price drops) went live in the US in November 2025 with Wayfair, Chewy, Quince and some Shopify stores. The Universal Commerce Protocol (UCP, 11 January 2026, with Shopify, Etsy, Wayfair, Target and Walmart) runs native checkout in AI Mode and Gemini with Google Pay, the merchant staying merchant of record. Universal Cart followed in May 2026, US first. **From September 2026 Google switched native checkout on by default for eligible stores on UCP platforms such as Shopify;** a merchant who wants every sale on its own site turns it off in the Shopify admin (Sales channels > Agentic) ([Search Engine Roundtable](https://www.seroundtable.com/google-native-checkout-emails-42140.html), 22 September 2026). Make the user decide, knowingly: native checkout trades the site visit, upsell and first-party data for fewer steps.
- **Virtual try-on** (apparel, from photos the shopper uploads) has been broadly available on US listings in Search, Shopping and Images since July 2025. Clean, full product images on a plain background are what it needs.
- **Ads** run above, below and inside AI Overviews and in AI Mode from Shopping and Performance Max: [paid-ads](../../paid-ads/SKILL.md).
- **In the EEA** free listings left web search in September 2026 ([ranking](ranking.md#europe-since-september-2026)); whether AI Mode product results there changed the same way is not documented.

## ChatGPT

- **Feeds took over in July 2026.** The share of ChatGPT Shopping picks Profound classes as feed-integrated jumped from 8.26% to 61.54% on 10 July 2026, a day after GPT-5.6, and stood at about 65% by 3 September. The top 10 stores' share of references rose from 22.5% to 41.8%, and unique merchants fell from 13,524 to 10,607; 450 of 687 tracked customers lost a third or more of their visibility within days (Profound, 1.76M prompt runs in July; one vendor's prompts, not shoppers; OpenAI has not confirmed).
- **Before that, Google Shopping supplied the carousel.** 83% of 43,000 carousel products strong-matched Google Shopping's top 40 organic results for ChatGPT's shopping fan-out query, against 11% for Bing; 60% of matches came from Google's top 10 (Peec AI, about 5,000 carousels, ten verticals, March 2026; 45.8% were exact title matches). For the web-retrieved third of picks that remain, ranking in Google Shopping is still an input.
- **Getting in:** Shopify stores are connected through Shopify Catalog and Etsy shops automatically. Others apply for direct feed access (waitlisted) or connect through a provider that speaks the Agentic Commerce Protocol (ACP, OpenAI and Stripe, September 2025; Salesforce and Stripe among the providers). OpenAI is reported to plan a self-serve feed platform. Feed fields are in [feed audit](feed-audit.md#a-chatgpt-feed).
- **How it ranks, in OpenAI's words:** product results are chosen by ChatGPT, are not ads and are not influenced by partnerships; a product's store list is ranked on availability, price, quality and whether the store makes or mainly sells the item. "Best price" goes to the lowest price ChatGPT can see.
- **Instant Checkout is gone.** OpenAI scaled it back in March 2026 (Shopify's president put active stores at about a dozen); purchases go to the merchant's site or app. Do not plan for in-chat checkout on ChatGPT.
- **Shopping shows on few prompts.** The shopping unit appeared on 8.8% of prompt runs; 79% of prompts never triggered it, about 6% did reliably, and a prompt that triggered one day had an 83% chance the next. Open-ended prompts triggered at 12.1%, brand-direct ones at 3.1% (Profound, about 2 million prompts each run 10 or more times, September 2025 to January 2026). Prompts that name a shippable product trigger; "I want to buy" without a product noun barely does.
- **Ads:** product feed ads (ads generated from the catalogue) began testing in May 2026 and show below the answer, labelled sponsored, like other ChatGPT ads: [paid-ads](../../paid-ads/SKILL.md).

## Perplexity

- Shopping has been free for all US users since November 2025, and PayPal's Instant Buy (25 November 2025) made PayPal merchants discoverable and buyable in the answer, PayPal acting as merchant of record for the checkout and the merchant keeping the customer (launch retailers included Abercrombie & Fitch, Ashley, Fabletics, Adorama and Newegg). Shopify stores shipping in the US are integrated; larger retailers can join Perplexity's free Merchant Program to share catalogue data.
- Perplexity says it has no sponsored slots in shopping and has been phasing out ads.
- In our runs Perplexity cited the most retailer pages (Best Buy's search and category pages above all) and editorial reviews (RTINGS, Wirecutter), and linked no Google Shopping pages.
- Amazon has fought Perplexity's Comet browser shopping on Amazon in court, and a US judge temporarily barred Comet from buying there; do not count on agent purchases on Amazon.

## Measuring AI shopping visibility

1. **Prompts:** 5 to 10 that name a product type the way shoppers ask ("best cordless vacuum for pet hair", "best espresso machine under $500", "where can I buy a standing desk cheapest"), plus one or two with the user's brand and product name. Brand prompts trigger shopping units far less.
2. **Run** `aeo_run_ai_answers` with the engines shoppers use (`chatgpt`, `gemini`, `ai_overview` and `ai_mode` are 2 credits each a prompt; `perplexity` 6), and in `brands` the user's store and brand, the competing stores and brands (names or domains, up to ten). Five engines on four prompts cost 56 credits.
3. **Score per engine:** whether the user's brand or store is named (`mentions[]`), cited, and which stores the answer links. The ai-search measurement rules apply: report how often, with a margin, never a rank ([ai-search](../../ai-search/SKILL.md)).
4. **Tell feed picks from web picks on ChatGPT:** retailer links from a catalogue integration carry the store's feed tags, such as `utm_medium=feed`, `utm_campaign=openai_catalog`, `afid=chatgpt` or a `refid` parameter; `utm_source=chatgpt.com` alone is ChatGPT's ordinary referral tag. A competitor whose links carry feed tags is in ChatGPT through a feed.
5. **Read Google's product picks:** product links in AI Overview, AI Mode and Gemini answers point to Google Shopping pages. Links in the text carry `catalogid`, `gpcid`, `headlineOfferDocid` and `productDocid`; cited sources carry `catalogid`, `gpcid` and `productid`. Pass `catalogid` as `product_id`, `gpcid` as `gid`, and `productDocid` (or `productid`) as `data_docid`: on the product we could match, that docid was the one `shopping_search_products` returns. All five seller calls we made from such ids worked, one with `product_id` alone; `headlineOfferDocid` also works but returned a different store list (10 stores against 3). The ids name the listing Google picked, so the price check and reviews run on exactly that product ([field notes](field-notes.md)).

**Reading it right:**

- On ChatGPT, `named_brands[]` holds product names on shopping answers ("Dyson V15 Detect", long Walmart product titles), not brands. Count stores and brands with `brands` and `mentions[]`.
- Retailers named and retailers linked differ: Amazon was named in four of our answers and linked by ChatGPT in none.
- AI Overviews do not appear for every product query (none for "standing desk" or "where can I buy a standing desk cheapest" in our runs); a null answer is not a loss.
- Gemini and AI Mode often answer with product cards whose text the answer field does not carry, so brand counts from their text are undercounts.
- **The listing an AI answer links can differ from Shopping's own.** AI Mode linked the Breville Bambino Plus to another listing than the one Shopping search shows (other `catalogid` and `gpcid`), whose stores were three unknown shops at $314.30 and Newegg. Check the store list behind an AI pick before reporting who it sends shoppers to.

## Unknown

- How much traffic or revenue ChatGPT shopping sends any merchant; every published figure measures what tracking prompts returned, not purchases.
- Whether conversational attributes, `popularity_rank` or ChatGPT's `review_count` and `star_rating` change which products are shown; nobody has published a controlled test.
- How AI performance insights' share of voice relates to sales.
