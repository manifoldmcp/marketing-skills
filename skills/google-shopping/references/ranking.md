# Where products show and who wins

Where a product can appear on Google, what decides the order, how much of it is paid, and how to see who wins a product search. Checked October 2026.

## The surfaces

One Merchant Center feed now supplies several places, each with its own rules:

- **The Shopping tab** (`udm=28`): a grid of offers, with Google's grouped carousels ("Electric Standing Desks") above the list. Paid offers carry a "Sponsored" label; the rest are free listings.
- **Product blocks on the web results page:** "Popular products", "Deals on ...", "More products". In our October 2026 fetches they showed on head terms ("standing desk", "espresso machine"), while "best X" queries got an AI Overview instead and no product block ([field notes](field-notes.md)).
- **AI Overviews and AI Mode:** product names in the answer link to Google Shopping product pages, and those pages are cited as sources. In our runs every AI Overview and AI Mode answer on a product prompt did this. See [AI shopping answers](ai-shopping.md).
- **Gemini, Images, YouTube and Maps** (local inventory) draw on the same Shopping Graph: over 50 billion listings, about 2 billion refreshed every hour (Google, 2025).

## What decides the order

- **Google's own words:** Shopping tab listings are ranked by relevance to the search, can be personalised by recent activity, and the weight of each parameter is set by algorithms; offers labelled Sponsored may be ranked and grouped by what the advertiser pays ([Google Shopping help](https://support.google.com/googleshopping/answer/9128904)). Shoppers can sort and filter by price, category and brand. Google publishes no weights.
- **Google says ads do not influence free listing eligibility or rank.** Running Shopping ads buys the Sponsored slots, not a better free position.
- **What the data shows matters, without weights:** relevance of title and attributes to the query (Google's AI performance guidance is to put shopper search terms into titles and descriptions and fill missing attributes), price against the same product elsewhere, availability and delivery, product and store ratings. Treat any published "ranking factor list" with weights as invented.
- **Same product, many stores.** Google groups offers by product (GTIN and title matching). Without a correct GTIN a product cannot be grouped and gets no price benchmark ([price checks](price-check.md)).

## How much is paid

- **Paid takes a growing share of product clicks.** Comparing January 2025 with January 2026 on US queries, paid results (text ads plus Shopping ads) took about a third of clicks in product categories; on the top 5,000 headphone queries, 16% to 36% (Similarweb, February 2026, four verticals). Organic click share fell from 73% to 50% on headphones.
- **Performance Max is most of Shopping spend:** 67% of Google Shopping spend and 68% of Shopping-attributed sales among advertisers running both campaign types in Q1 2026 (Tinuiti, managed accounts, via secondary summaries). Bidding and campaign setup belong to [paid-ads](../../paid-ads/SKILL.md).
- **Amazon left the auction.** Amazon's Shopping ad impression share went from about 60% to zero in the US (55% to zero in the UK, 38% in Germany) over 21 to 23 July 2025 (Tinuiti's client data). In Q2 2026 Google Shopping clicks rose about 19% with CPC down about 1% (Tinuiti, as summarised). Walmart, Temu and Shein held 10% to 30% impression share. Amazon still ranks in organic web results and is named in AI answers ([field notes](field-notes.md)).

## Europe since September 2026

- **Free listings left European web search on 17 and 18 September 2026.** Popular products carousels fell 90% to 100% in Germany, France, Belgium, Sweden and the Netherlands (Productrise and Semrush trackers, unaudited). Google's Ads Liaison tied it to the Digital Markets Act, after the European Commission's €460 million fine of 23 July 2026 with a 60-day order to stop self-preferencing ([Search Engine Roundtable](https://www.seroundtable.com/google-drops-free-product-listings-eea-42113.html), 18 September 2026). Google has posted no help-page note.
- **What remains:** paid Shopping ads (bought directly or through a comparison shopping service), the Shopping tab, Images and other surfaces, and new comparison-site units that send shoppers to aggregators first. The UK, Switzerland, the US and Australia were not affected.
- For an EEA shop, a fall in free-listing clicks from mid-September 2026 is this change, not the feed. Check the date before diagnosing anything else, and weigh comparison sites and a CSS for paid Shopping.

## Who wins a product search

The job: for the queries that sell the user's products, which stores and products take the top positions, and where the user sits.

1. **Pick 5 to 15 queries** the way shoppers type them: product type ("standing desk"), type plus need ("standing desk for small spaces"), brand plus model, and "best X". Head terms show product blocks; "best X" shows AI Overviews.
2. **Shopping results:** `shopping_search_products` per query (1 credit per 40 results). Per row read `rank`, `seller`, `price`, `old_price`, `rating`, `ratings_count`, `tags[]` (discount badges such as "62% OFF", and "LOW PRICE") and `delivery`. Count distinct products (by `gid`) and rows per seller in the top 20 and top 40, and the median price of the top 10. For a query that matters, fetch it on two or three days and report what holds across them.
3. **Web page:** `seo_get_serp` with `ai_overview: true` (2 credits) shows whether a product block or an AI Overview appears and which products the overview names. Its product block rows carry no products; read those with `shopping_search_products`.
4. **Report** a table per query: top sellers by rows held, the user's best rank (or absent), price position against the top-10 median, rating against the top 10, and which surfaces the query shows. Say the date, location and that results are personalised for real shoppers and differ by location.

**Reading it right:**

- **`rank` counts the rows returned.** It runs 1 to N with no gaps, in Google's order. Carousels and results with no product page are left out first, and those can be half a page (one query once returned 17 products from the 40 results of a page), so expect fewer rows than `depth` and read `rank` as a place among the products returned, not the slot a shopper sees.
- **One fetch is one draw.** Two fresh fetches of "espresso machine" five minutes apart shared about half their products and 2 of their top 10; one product went from rank 1 to 20 ([field notes](field-notes.md)). A rank move between two runs means nothing until it holds on a third. Results are cached a day, so a same-day repeat returns the same fetch, free, and is not a second sample.
- Rows do not say which offers were Sponsored. The ranking is the mixed page a shopper sees, not the free listing order.
- **A row is an offer.** The same product can fill two rows in one fetch, or one row on each 40-row page of a deep fetch; `product_id` changes with the store shown while `gid` stays. Count and track products by `gid`, and dedupe before counting rows per seller.
- **Odd stores rank.** An unknown store with no rating held ranks 2 and 14 for "espresso machine" with Breville machines at well under half the retail price. Flag such rows rather than calling them competitors.
- Amazon was in none of the 166 Shopping rows we fetched in October 2026, free listings included. Read its offers with the [amazon](../../amazon/SKILL.md) tools, not here.
- A query where the user is absent but competitors hold three or more rows is a feed and relevance question first ([feed audit](feed-audit.md)), a price question second ([price checks](price-check.md)).

## Without the connector

Open the Shopping tab in a private window with the country set (`&gl=us`), note the top 20 offers with store, price and Sponsored label, and repeat for the web results page. Say it is one sample from one place on one day, and that a logged-in shopper may see a different order.
