# Amazon products in AI answers

Whether a product sold on Amazon shows up when people ask ChatGPT, Perplexity, Gemini or Google's AI what to buy, and what a seller can do about it. Amazon's own assistant is covered in [search ranking](search-ranking.md). Checked October 2026.

## Amazon has walled itself off from most engines

- **robots.txt.** In August 2025 Amazon added blocks for AI agents and crawlers from Anthropic, OpenAI, Google's Project Mariner agent and Meta. In November 2025, days after ChatGPT launched shopping research (24 November), it blocked OpenAI's ChatGPT-User and OAI-SearchBot, the bots ChatGPT uses to fetch and index pages for answers. Googlebot is not blocked: Amazon pages stay in Google Search and the index Google's AI answers draw on.
- **Agents in court.** Amazon sued Perplexity in November 2025 over its Comet browser shopping on Amazon for users. In March 2026 a federal judge granted a preliminary injunction blocking Comet from buying on Amazon, citing evidence it accessed the site without authorisation and evaded a block within a day. Not a final ruling.
- **Google Shopping ads.** Amazon's impression share in US Google Shopping auctions fell from about 60% to zero between 21 and 23 July 2025 and was still zero in Tinuiti's Q2 2026 data; it returned to its other markets within about a month (Search Engine Journal, 28 July 2026, from Tinuiti and Smarter Ecommerce data). Amazon still buys US Google Search text ads.

## What that does to visibility

- **ChatGPT and Perplexity rarely cite Amazon.** In 391,000 citations on non-branded US shopping questions across ChatGPT, Perplexity, Gemini, AI Mode and AI Overviews, ChatGPT cited amazon.com 6 times and Perplexity never; nearly all Amazon citations came from Google's AI answers. Retailers and marketplaces together took 2.9% of shopping citations; brand and manufacturer sites about two-thirds; Reddit and YouTube together out-cited every big-box retailer (LLM Pulse, July 2026, vendor dataset).
- **So the brand's own site and feed carry the AI answer, not its Amazon listing.** When ChatGPT recommends a product it links the brand's site or retailers integrated with it (Walmart, Target, Etsy and others), and its product cards come mostly from merchant feeds (see [ai-search](../../ai-search/SKILL.md)). An Amazon-only brand with no site is close to invisible there.
- **Google's AI can still show Amazon pages**, organically, and Gemini often answers product questions with Google Shopping lists, which draw on merchant feeds, not Amazon listings.

## What a seller can do

1. **Have a crawlable brand site** with the product's specs, price and use cases in plain HTML, and the Amazon link as the way to buy. That is the page engines can read.
2. **Submit a product feed** to Google Merchant Center and to ChatGPT's merchant programme where eligible, so the products exist in the feeds AI shopping answers draw on. Feeds and Google Shopping are in [google-shopping](../../google-shopping/SKILL.md).
3. **Be talked about where engines read:** reviews on the brand's site, YouTube reviews, editorial roundups, Reddit. That is the [ai-search](../../ai-search/SKILL.md) work; it applies to an Amazon brand like any other.
4. **Measure, do not assume.** `aeo_run_ai_answers` asks the buyer's question on the engines that matter and shows which products and pages are named; 27 credits a prompt on the default five engines. Expect amazon.com to be absent from ChatGPT and Perplexity citations even when the product is named.

## Unknown

Whether Amazon and OpenAI or others will strike a data deal (Amazon has said talks with third-party shopping tools continue); whether the Perplexity injunction survives appeal; whether being named in AI answers off Amazon lifts Amazon sales (no study separates it).
