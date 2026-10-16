# Price checks against other stores

Who sells the same product, at what delivered price, and whether the user is undercut. Checked October 2026.

## Why price position matters on every surface

- **Google** lets shoppers sort and filter by price, adds price-drop annotations computed from the product's running historical average price, and benchmarks every product with a GTIN against other stores' prices in Merchant Center's price competitiveness report.
- **ChatGPT** gives a "Best price" tag to the lowest price it can see, machine-assigned, not set by the merchant. It was on every feed-retrieved offer in Profound's June 2026 snapshot (about 1 million offers) and on 21% of offers retrieved from web pages. OpenAI says its merchant list for a product is ranked by availability, price, quality and whether the store makes or is the main seller of the item. Neither company publishes weights; no study isolates price as a cause of rank.

## Merchant Center's own report

- **Price competitiveness** (Growth > Price competitiveness) replaced the old price benchmarks report on 1 September 2025. It shows a click-weighted benchmark price per product from other stores' offers on Shopping ads and free listings, the current gap to it and the gap averaged over a date range, by product or by category, brand and product type.
- It needs a GTIN: a product Google cannot match to other stores has no benchmark. It counts auctions, not products.
- No Manifold tool reads it. Ask the user for an export, and use it as the user's own view; the tools below are the outside view.
- Third-party thresholds ("lose eligibility at 20% above benchmark", "sale price is what counts") have no Google source. Do not quote them.

## The check

1. **Find the product:** `shopping_search_products` on the model name or the user's product title (1 credit). Pick the row that is the same product (title, brand, model); carry its `product_id`, `gid` and `data_docid`. If several rows are the product under different stores, they share a `gid`; different ids for one product can return different store lists, so when the answer matters run two and merge them.
2. **Its stores:** `shopping_get_sellers` with all three ids (2 credits, cached 6 hours). Per row: `seller`, `domain`, `price` (base), `shipping`, `total_price`, `availability`, `seller_rating`, `seller_ratings_count`, and `note`. The tool asks for up to 20 stores; lists ran 3 to 10 in our runs, often 5, and can miss the store on the product's own search row, so they are Google's first stores, not every store: say "among the stores Google lists".
3. **Compare delivered prices,** not base prices: `total_price`, which adds `shipping` when the store sends it. `shipping` was empty on 40 of 48 rows we saw, so an empty one is free or unknown. Rows carry no tax and no condition, which the vendor never filled, so read used or refurbished offers from the store's name and URL. Say which you compared.
4. **Report** per product: the user's delivered price, the lowest and median delivered price among other stores, the gap in money and percent, who is cheapest, and any store marked "Best price". Flag used or refurbished offers apart.
5. **For a catalogue:** run the check on the 10 to 30 products that earn the most, not every SKU. About 3 credits a product.

## Reading it right

- **The same store appears once per variant or seller.** One brand store listed one desk three times, in three colours at two prices; eBay filled eight of ten rows on one product, one per eBay seller. Compare like variants, and count a store once when counting competitors.
- **Too cheap is a warning, not a rival.** Stores with no `seller_rating` listed a $499.95 espresso machine at $314.30 and a $500 one at $104.65, and a catering site and a jewellery site sold the same machine ([field notes](field-notes.md)). Leave a store out of the lowest price when it has no rating, an unrelated domain and a price far below everyone else, and tell the user why.
- **`note` is mixed.** It holds Google's labels ("Best price", "Most popular", "Usually $110", "26% off $150") and the store's own promotion text ("30% Off FALL"). "Best price" was given when five stores asked the same price, so it does not prove an undercut.
- **Equal prices are common.** All five stores listing one machine asked exactly $499.95, the brand's list price. A brand with a price policy reads this as compliance; a reseller should compete on delivery, returns and ratings instead.
- **Location changes everything.** Stores, shipping and currency follow `location`. Check in each country the user sells to, never one for all.
- **Matching is Google's, and the ids decide the list.** A different pack size or model year can be grouped as the same product (a K1 grinder sat in a K6 list). The same product reached by other ids, from another search row or an AI answer, returned a different store list: 3 stores against 10 for one machine. Read titles, and run the check from the ids of the row the shopper actually sees.
- **Six hours old at most when cached,** and prices move daily in some categories. Date every price and re-check before acting on a small gap.
- **The store list is not the whole market.** It is capped, and stores that do not submit to Merchant Center are not in it. Amazon was in none of our seven seller lists; its offers are read with `amazon_get_sellers` ([amazon](../../amazon/SKILL.md)).
- `shopping_get_product` (1 credit) returned the same stores in `offers[]` as the seller list on both products we checked, with `regular_price` beside `price`, which shows who is discounting, and no shipping. It is the cheaper check where shipping does not matter.

## What to do with a gap

- No platform publishes the gap at which a listing loses out, so the threshold is the user's call against their margin. Shipping speed, free returns, ratings and member pricing also show on the listing and can outweigh a small gap.
- If the user is cheapest but not winning, the problem is relevance or trust: titles, attributes, ratings ([feed audit](feed-audit.md), [ratings and reviews](reviews.md)).
- Never change prices, promotions or feeds for the user. Hand over the table.

## Watching prices

The server keeps no history and has no scheduler. For a weekly watch, the host stores each run's date, location, the three ids (match products by `gid`, since `product_id` can change with the store shown) and per-store delivered price, and compares; set it in the host's scheduler if it has one. Ten products weekly is about 120 credits a month.

## Without the connector

Open the product in the Shopping tab, open "Compare prices" or the store list, and note each store's price, shipping and total in a private window with the country set.
