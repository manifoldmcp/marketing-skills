# Field notes

What Manifold has seen in its own calls of the Amazon tools. Small samples of public products: read them as evidence of how the data behaves, not as benchmarks. Newest first.

## First live run of the Amazon tools (16 October 2026)

Inputs: results pages for "standing desk" (US, depth 100; UK, depth 60), "electric toothbrush" and "protein powder" (US, depth 60); `amazon_get_product` on four ASINs from those pages (ErGear B0B41YH9B6, FEZIBO B0F8MHPVPH, Oral-B Pro 1000 B01AKGRTUM, FLEXISPOT B0HK37BDQV); `amazon_get_sellers` on three (B01AKGRTUM, B0B41YH9B6, Oral-B iO5 B0B5YPHMW8); `amazon_get_keywords` on B0B41YH9B6 (100 rows) and B01AKGRTUM (40 rows); `amazon_get_competitors` on B0B41YH9B6 (30 rows); `amazon_search_keywords` on "standing desk"; `amazon_get_keyword_metrics` on 17 US terms and 5 UK terms, with `seo_get_keyword_metrics` on the same 15 US terms for comparison. 69 credits in all.

**Cost and speed.** Billing matched the descriptions: 2 credits a results page or listing or seller list, 0 when cached (one listing was a day-old cache). List tools bill 5 plus 5 per 100 rows rounded up on the rows returned: 24, 30 and 40 rows each cost 7, 100 rows 10, 17 keywords 6. `amazon_search_keywords` asked for 100, returned 24 and charged 7, not the estimated 10. Calls took about 3 to 6 seconds each, `amazon_search_keywords` about 10; none came near the 60-second timeout.

**Results pages.**

| Page | Rows | Sponsored rows | Sponsored above first organic | Rows with `bought_past_month` null |
|---|---|---|---|---|
| standing desk, US | 100 | 2 | 0 | 37 |
| standing desk, UK | 60 | 2 (one ASIN, twice) | 0 | 25 |
| electric toothbrush, US | 60 | 1 | 0 | 6 |
| protein powder, US | 60 | 14 | 4 | 0 |

- Sponsored share of the returned rows ran from 2% to 23%, against the roughly 30% Dash and others measured. The rows are inline Sponsored Products only; banners, video and sponsored carousels are not rows. Read the count as a floor on paid space, and as a category signal (supplements carry far more inline ads than desks).
- The same ASIN can appear as both: Orgain was sponsored at 1 and organic at 16; Devoko (UK) sponsored at 4 and 50 and organic at 13. Dedupe by ASIN before counting products.
- `rank` had one gap (51) on both standing desk pages and none on the other two: the page-two break, not a hidden block. Ranks ran past `depth` by one where a gap fell (rank 101 on a depth of 100).
- `bought_past_month` values seen: 50, 100, 200 to 900, then 1,000 to 9,000 in thousands, 10,000, 20,000, 30,000, 40,000, 50,000, 60,000, 70,000, 100,000. It is per child: three colours of the Oral-B Pro 1000 shared 165,200 ratings but showed 10K, 20K and 10K bought.
- `ratings_count` on results rows is Amazon's rounded display (5,200; 4,900); `amazon_get_product` gave the exact counts (5,255; 4,904).
- `price_to` was null on all 280 rows. `price` was null on 3 toothbrush rows; one of them (B0B5YPHMW8) had no featured offer in `amazon_get_sellers`.
- `offers[]` holds badge text ("Exclusive Prime price", "You pay $369.99 with coupon", "Extra 25% off when you subscribe"). On the UK page six of 60 rows had one badge split across three entries ("25% off a", "£120.00", "purchase from this seller"; four at "10% off a"). The tool now joins the pieces into one badge.
- `related_searches[]` held six terms repeated three times (US standing desk) and twice (UK), six once (protein powder), and nothing for electric toothbrush: the vendor repeats the box once per results page it reads. The tool now keeps each term once; do not rely on the list being there.
- Titles: 13 of the top 40 organic titles (top 20 on each of the two US pages) were over 75 characters, up to 199, nearly three months after the 27 July cap, including Oral-B iO listings and a FLEXISPOT converter at 185.
- A FLEXISPOT converter with no ratings ranked third organic for "standing desk" (Best Sellers Rank about 195,000 in Office Products), and a desk with 4 ratings ranked eighth. One snapshot; it says nothing about how long they stay.
- The UK call read amazon.co.uk, in GBP, with the same row shape.

**Listings (`amazon_get_product`).**

- Best Sellers Rank is there, as a `specs[]` row named "Best Sellers Rank" holding text such as "#1,418 in Home & Kitchen ... #2 in Home Office Desks", on all four listings. Current value only; no history.
- Item Highlights showed as a `specs[]` row named "Item Highlight" on the two desk listings, not on the Oral-B or the FLEXISPOT converter. `amazon_get_sellers` returned ErGear's `title` as the title, " | ", then its Item Highlights.
- The listing's second price was the list (struck-through) price, not the top of a variation range: 99.99 to 119.99 with `discount_percent` 17, 99.99 to 112.99 at 12, 300.99 to 369.99 at 19, and null with no discount on the Oral-B. It matched `regular_price` in the seller rows, and the field is now named `regular_price` on the listing too (it was `price_to`).
- `specs[]` depends on the category's page layout: the furniture listings returned 37 to 50 attribute rows in groups (Style, Measurements, Item details, Materials & Care, Features & Specs, User guide); the Oral-B returned 8 rows in one "Product details" group, including "Date First Available". Values are what the seller entered and can be wrong ("Additional Features: Bagless" on a desk).
- `description` starts "About this item" and ends "See more product details" or "Show more"; strip both.
- `images[]` held 7 entries; on two listings the last was a video thumbnail (its URL contains "play-button"). Count it as a video, not a photo.
- `variation_asins[]` held 59, 18, 5 and 0 children; `parent_asin` was null on the listing with no variations.

**Sellers (`amazon_get_sellers`).**

- `ships_from` reads "Amazon.com" for fulfilment by Amazon, not "Amazon". `seller` "Amazon.com" is Amazon retail, with `seller_url` null. Every other `seller_url` carries isAmazonFulfilled=1 or 0, the cleanest FBA flag. The vendor sends these links as http://; the tool now returns https://.
- On the Oral-B Pro 1000, Amazon.com held rank 1 at $49.94 while an FBA reseller offered $49.79 at rank 2: the cheapest offer did not win.
- No featured offer: on B0B5YPHMW8 rank 1 came back with every field null, then five offers from rank 2 at $119 to $179.99, and the results row showed `price` null. The tool now drops that empty row and returns `has_featured_offer` false, with the offers still ranked from 2.
- ErGear's listing had one offer, from its company store ("Bestqi Ergonomic"), not a seller named after the brand; `condition` was null on it. `regular_price` was null on every reseller row. `delivery` was null on some rows and joins options in one string ("FREE delivery Thursday, October 29. Details Or fastest delivery Tomorrow").

**Keywords and competitors (Labs index).**

- `amazon_get_keywords` rows were not sorted by volume, though the description says they are. ErGear's first 100 of 3,903 rows ran 37, 37, 0, 37, 0, 74; its largest, "electric desk" (1,570), was row 52, and "standing desk" (68,105), where it ranked first live, was not in the first 100. The Oral-B's first 40 of 1,862 missed "electric toothbrush" (58,306), where it ranked second live. The tool now asks the vendor for volume order, biggest first.
- `rank` on those 140 rows was only ever 1, 2, 4 or 5, while `amazon_get_competitors` put the same rivals' average positions on shared terms at 14 to 29. The cause was the order, not the field: unordered, the vendor leads with a product's best placements, and ErGear's rows 3,801 to 3,820 (a follow-up call, 6 credits) ranked 50 to 65. `rank` is the page position across organic and sponsored rows when the index crawled the search (a row at 5 had Amazon's sr_1_5 in its URL).
- Low volumes come in steps of about 37 (37, 74, 112, 149, 187, 224, 261, 336), and about half the rows had volume 0. The rows include other languages ("escritorio ajustable altura", Portuguese and Chinese queries), other retailers ("desk costco", "office depot standing desk", "walmart toothbrush oral b") and rival brands ("razer desk"). One keyword can appear twice, once organic and once sponsored ("elevated table"). Index dated 3 October 2026, 13 days before the call.
- `amazon_get_competitors` matched the live page: 8 of its top 10 by `shared_keywords` were on the US "standing desk" page one. `avg_position` ran 14 to 29; `organic_volume` reached 1.57 million (a sum over thousands of terms, so a footprint, not demand); `sponsored_keywords` ran from 1 to 1,645. The product itself was left out; `meta.data_as_of` was null.
- `amazon_search_keywords` on "standing desk" returned 24 rows sorted by volume, and drifted into neighbouring categories: desk 209,066, chair 95,631, treadmill 72,406, walking pad 72,182. Filter before use.

**Amazon volume against Google volume.** Both from DataForSEO, US, the same day.

| Keyword | Amazon | Google | Amazon / Google |
|---|---|---|---|
| standing desk | 68,105 | 135,000 | 0.50 |
| standing desk converter | 15,109 | 18,100 | 0.83 |
| electric standing desk | 4,974 | 22,200 | 0.22 |
| l shaped standing desk | 4,338 | 9,900 | 0.44 |
| walking pad | 72,182 | 246,000 | 0.29 |
| electric toothbrush | 58,306 | 135,000 | 0.43 |
| oral b electric toothbrush | 38,858 | 90,500 | 0.43 |
| sonicare toothbrush | 19,074 | 49,500 | 0.39 |
| electric toothbrush for kids | 635 | 14,800 | 0.04 |
| toothbrush electric | 3,440 | 2,900 | 1.19 |

Across all 15 terms the median ratio was 0.43 and 14 of 15 were lower on Amazon. Neither number is audited; the point is that the two models do not share a unit, so a channel split from them is an assumption. Amazon volume is case-insensitive ("Electric Toothbrush" returned the same 58,306), has no monthly trend (the Google rows show walking pad swinging from 135,000 to 368,000), and a nine-word phrase came back null. UK volumes came back for all five terms tried (standing desk 11,519, electric toothbrush 11,818).
