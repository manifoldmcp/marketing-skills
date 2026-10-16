# A 90-day Shopping plan

When the user wants a plan rather than one check: which products and queries to win, where the shop stands on Google Shopping and in AI shopping answers, and which work closes the gap fastest.

## Order of work

The order follows what blocks what. Skip a step only when the baseline shows it is fine.

1. **Eligibility.** Merchant Center account healthy, no disapprovals on top products, free listings on, the Merchant API in use if the shop pushed products through the Content API (shut down 18 August 2026). Nothing else works until products are eligible ([feed audit](feed-audit.md)).
2. **A feed in every channel the buyers use.** Google through Merchant Center; ChatGPT through Shopify or Etsy, an ACP provider or the application; Perplexity through Shopify, PayPal or its Merchant Program. Since July 2026 a shop without a ChatGPT feed is competing for the minority of picks taken from the web ([AI shopping answers](ai-shopping.md#chatgpt)).
3. **Checkout choices.** Whether to leave Google's native checkout on (on by default for eligible Shopify stores since September 2026) and whether to enable checkout flags in ChatGPT. A business decision for the user, made knowingly.
4. **Data quality on the products that earn.** GTINs, titles in shopper words, attributes AI performance insights says are missing, shipping and returns, images; then the conversational attributes for the top products ([feed audit](feed-audit.md)).
5. **Ratings.** A product reviews feed past the 50-review floor, Google Customer Reviews or another provider building toward about 100 store reviews per country, asked of every buyer ([ratings and reviews](reviews.md)).
6. **Price position** on the products where the user is undercut on delivered price ([price checks](price-check.md)).
7. **Paid on top**, once the free side is sound: Shopping and Performance Max, and in the EEA a comparison shopping service, through [paid-ads](../../paid-ads/SKILL.md).

## Baseline

- 10 to 20 shopper queries in `shopping_search_products` and `seo_get_serp` with `ai_overview: true`: about 3 credits a query, 30 to 60.
- Price checks on the 10 to 30 top products: about 3 credits a product.
- Reviews of the user's top three products and the three competitor products that win most: about 30 credits at 100 reviews each.
- Six to ten product prompts in `aeo_run_ai_answers` on `chatgpt`, `gemini`, `ai_overview`, `ai_mode` (8 credits a prompt) plus `perplexity` where its buyers are (14 a prompt): 50 to 140.
- Total about 150 to 350 credits. The user's own numbers (Merchant Center performance, free listing clicks, price competitiveness, AI performance insights) come as exports; no tool reads them.

## The 90 days

| Period | Typical work | KPI the tools or the user's exports re-measure |
|---|---|---|
| Days 1 to 30 | Disapprovals fixed, free listings on, ChatGPT and Perplexity feeds connected or applied for, checkout choices made, GTINs and titles fixed on the top 20 products, review requests to every buyer | Products eligible; feed live per channel; top products appearing for their queries |
| Days 31 to 60 | Attributes and conversational attributes on top products, shipping and returns shown, listing copy answering the top review complaints, price decisions on undercut products | Share of top-20 Shopping rows held; AI performance insights share of voice; free listing clicks |
| Days 61 to 90 | Second wave of products, store rating past its threshold where close, paid Shopping where the free side is sound, re-measure on the same queries and prompts | Products present and rows held per query, across runs; store named or linked per AI engine; feed-tagged ChatGPT links; ratings shown |

Give each period an owner, and three first actions for this week.

## Setting targets honestly

- Promise inputs (eligible products, live feeds, complete attributes, ratings shown), not positions. Google publishes no ranking weights, ChatGPT's picks moved by half on one day in July 2026, and Shopping results are personalised and vary by location.
- Re-measure on the same queries, locations and prompts, on different days (rows are cached a day, sellers six hours). Two fresh fetches of one query minutes apart shared only 2 of their top 10 in our runs, so read one run as one draw, report rows held and products present across runs rather than a rank, and call a change real only when it holds for three runs ([field notes](field-notes.md)).
- AI answer shares carry the margins in [ai-search](../../ai-search/SKILL.md); on four prompts and five engines one answer is five points.
- For an EEA shop, set free-listing targets from the September 2026 baseline, not from before it ([ranking](ranking.md#europe-since-september-2026)).
- Re-check the plan after any big platform change: a model release on ChatGPT, a Merchant Center specification update (usually April), a Google Marketing Live (May).
