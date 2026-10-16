# Reading Manifold's Amazon data

What each Amazon tool returns, where its fields come from, and the readings that would be wrong. Every Amazon tool runs on DataForSEO: the results page, listing and sellers are read live from Amazon (then cached); keywords, competitors and volumes come from DataForSEO Labs' periodic Amazon index. Checked against live calls on 16 October 2026; the sample, inputs and numbers are in [field notes](field-notes.md).

## Results page: `amazon_search_products`

- One logged-out page for one search, one marketplace (the country in `location` picks it: "United Kingdom" read amazon.co.uk in GBP), at one moment. 2 credits per 100 results with `depth` up to 700, cached 1 day; a call took about 5 seconds.
- `rank` is the position counted across organic and sponsored rows together. To give an organic position, count organic rows above it. Gaps are rare: one gap at the page-two break (rank 51) on the pages we read, so `rank` can run one past `depth`.
- `type` is organic or sponsored; a sponsored row is an inline Sponsored Products placement. Banners, video and sponsored carousels are not rows, so the rows understate paid space: sponsored rows were 2% of a standing desk page and 23% of a protein powder page, against about 30% in the academic count. Read the sponsored count as a floor and compare it between terms, not with studies.
- One ASIN can appear as sponsored and organic on the same page, even twice as sponsored. Dedupe by ASIN when counting products; count every row when counting paid slots.
- `bought_past_month` is the badge's floor in customers, not units: 50, 100, 200 up to 900, then thousands, 10K, 20K and so on up to 100K. Null means no badge (below Amazon's threshold), not no sales; 37 of 100 standing desk rows were null against none on the protein powder page. It is per child: three colours sharing one rating count showed different badges. A 2023 PMG analysis found the badge close to true sales most of the time; seller anecdotes report gaps of 2x. Use it to order products and as a floor, never as a sales figure.
- `rating` and `ratings_count` are star ratings; `ratings_count` here is Amazon's rounded display (5,200 for 5,255). Take exact counts from `amazon_get_product`.
- `price` is the displayed price; null usually means no featured offer (confirm with `amazon_get_sellers`). `price_to` was null on every one of 280 rows; do not build on it.
- `offers[]` holds badge text: coupons ("You pay $369.99 with coupon"), Prime-exclusive and subscribe-and-save prices. Amazon.co.uk sends some badges in pieces; the tool joins them, so one entry is one badge ("25% off a £120.00 purchase from this seller").
- `results_count` is Amazon's "over N results" for the search: how many products match, not how many people search.
- `related_searches[]` is free seed material, each term once, and empty on some searches (electric toothbrush had none).

## One listing: `amazon_get_product`

- 2 credits, cached 1 day, about 5 seconds live. Pass the ASIN, the 10 characters after /dp/.
- `description` is the About-this-item text, wrapped in "About this item" and "See more product details" or "Show more"; strip both.
- `specs[]` holds the product detail tables, and its shape depends on the category: furniture listings returned 37 to 50 attribute rows in named groups, a toothbrush 8 rows under "Product details". Look in it for:
  - **Best Sellers Rank**, as a row of that name with the current ranks as text ("#1,418 in Home & Kitchen ... #2 in Home Office Desks"). Current value only; no history. It ranks sales within a category against other products, so compare it only within the same category node.
  - **Item Highlights**, as a row named "Item Highlight", on listings that have filled it (two of four in our sample).
  - "Date First Available" on some layouts, useful for the age of a listing.
  - Values are what the seller entered and can be wrong or junk; quote them as the listing's claim.
- The brand's long marketing sections (A+ content), backend search terms and attributes the page does not display are never returned.
- `regular_price` is the list (struck-through) price, the same figure as `regular_price` on the seller rows, and `discount_percent` is the cut from it (99.99 against 119.99, 17). Null with no discount. There is no variation price range; read each child's `price` from its own listing.
- `images[]` can end with a video thumbnail (its URL contains "play-button"); count it as a video, not a photo.
- `variation_asins[]` lists the other children, `parent_asin` groups them (null when there are none). `byline` is the brand or store line under the title ("Visit the ErGear Store").
- No review text, price history or sales. Do not infer sales from `ratings_count`.

## Sellers: `amazon_get_sellers`

- 2 credits, cached 6 hours, about 4 seconds live. `rank` 1 is the featured offer at fetch time; it can rotate within a day.
- `ships_from` "Amazon.com" means fulfilled by Amazon (FBA or Amazon retail); the merchant's own name means it ships the order itself. `seller` "Amazon.com" is Amazon retail, and its `seller_url` is null. Every other `seller_url` (https) carries isAmazonFulfilled=1 or 0, the cleanest FBA flag.
- **No featured offer** comes back as `has_featured_offer` false, with the other offers from rank 2 and no rank 1. Report "no featured offer at that time", which the live page shows as "See all buying options".
- `seller` is the account's store name, which can differ from the brand. `regular_price` is usually null; `condition` can be null on the featured offer. `delivery` is the promise text for the default address and can join several options in one string or be null.
- `title` on this response can carry the Item Highlights after a " | ".

## Keywords and competitors: `amazon_get_keywords`, `amazon_get_competitors`

- 5 credits plus 5 per 100 rows, rounded up on the rows returned (30 or 40 rows cost 7), cached 7 days, from Labs' index: a snapshot from its last crawl, 13 days old in our calls, not today's page. A brand-new listing may have few or no rows for weeks.
- **`amazon_get_keywords` rows come biggest search first**, so the default 100 rows hold the head terms. A leading desk ranks on about 3,900 searches, about half of them volume 0; page further only for the long tail.
- **`rank` there is the page position when the index crawled it**, counting organic and sponsored rows together, as on `amazon_search_products`. It runs from 1 to past 60 (a desk ranked 50 to 65 on its tail terms); it is about two weeks old, so confirm a position that matters on the live page before reporting it.
- Other rows to filter: volume 0 (about half), other languages, other retailers' names ("desk costco"), rival brands. `type` sponsored marks a term where the ASIN was seen advertising, and one term can appear twice, organic and sponsored.
- `amazon_get_competitors` rows: `shared_keywords`, `avg_position` on those, `organic_keywords` and `organic_volume` (sum of monthly volume of every term it ranks on organically at any position, over a million for big desks, so a footprint, not demand), `sponsored_keywords`. ASINs only; the product itself is left out. The top rivals by `shared_keywords` matched the live page well (8 of the top 10 were on page one). Several children of one rival can appear separately.

## Volumes: `amazon_search_keywords`, `amazon_get_keyword_metrics`

- `amazon_search_keywords` expands a seed from Amazon's related-searches box (two steps), not autocomplete, with volume, sorted by volume. It can return far fewer rows than `limit` (24 of 100 for "standing desk", charged 7) and drifts into neighbouring categories (desk, chair, treadmill): filter by product type before use.
- `amazon_get_keyword_metrics` returns volume for up to 1,000 keywords in the order given; 5 plus 5 per 100 keywords, rounded up. A null is no data, not zero; long phrases often come back null. Case does not matter.
- `volume` is DataForSEO's modelled monthly Amazon searches, with no monthly trend. Low values come in steps of about 37, so differences under about 100 are noise. The method is unpublished and untested independently (see [keyword research](keyword-research.md)). Rank keywords with it; do not sum it into a forecast without saying so.
- **Against Google volume from the same vendor**, Amazon ran at a median of 0.43 of Google on 15 product terms, from 0.04 to 1.19. Do not read the gap as a channel split; the two models share no unit.
- Labs' Amazon coverage includes the UK (all five terms tried came back) and other large markets, but is narrower than the live results page. A table of nulls outside the US is coverage, not zero demand.

## What no tool returns

Review text, Best Sellers Rank history, unit sales, revenue, conversion, Brand Analytics, ad impressions, spend or bids, Alexa for Shopping answers, and the user's own Seller Central data. Ask the user for exports or say "not measured".

## Cost and freshness

Say the estimate before a run, pass `max_credits` when the user names a budget, and use `dry_run: true` to price a call free (the estimate ignores the cache, so a cached call can cost less than quoted). Repeating a call inside its cache window is free and returns the same data; date every table, and for a watch let the host store each run, since Manifold keeps no history.
