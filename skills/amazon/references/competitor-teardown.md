# Competitor teardown

Who competes with a product on Amazon, how each rival's listing, price and search footprint compare, and what the user can take from it. Checked October 2026. For a rival beyond Amazon (its site, ads elsewhere, company data), see [competitor-intel](../../competitor-intel/SKILL.md).

## Finding the set

- **Product competitors, not brand competitors.** On Amazon the rival is the ASIN shown beside the user's for the same searches. `amazon_get_competitors` (10 credits for 100 rows) returns ASINs that share the user's ranked searches, with `shared_keywords`, `avg_position` on those, and their own `organic_keywords`, `organic_volume` and `sponsored_keywords`. It returns ASINs only; titles need `amazon_get_product`. In a live test its top rivals by `shared_keywords` were mostly the products on page one for the main term (8 of 10), so it is a fair starting set ([field notes](field-notes.md)).
- **Dedupe by parent.** Several children of one parent can appear as separate competitors. `parent_asin` on `amazon_get_product` groups them.
- **Check against the page.** A competitor by keyword overlap can be a different product (an accessory, a cheaper tier, a refill). Keep ASINs that appear on the user's core results pages from `amazon_search_products` at a comparable price. Three to five real rivals beat twenty overlaps.
- **Do not forget Amazon itself.** Amazon Basics and other Amazon-owned brands, and offers sold by Amazon.com on a rival's listing, compete on different terms. `byline` on the listing and the seller rows show them.

## The teardown

Per rival, from `amazon_get_product` (2 credits), `amazon_get_sellers` (2) and `amazon_get_keywords` (10 for 100 rows, biggest search first; see [reading the data](data.md)):

| Row | Field | What to read |
|---|---|---|
| Positioning | `title`, `description`, `specs[]` | The product type, audience and use case in the first 75 characters; what the bullets answer |
| Price | `price`, `regular_price` (the list price) and `discount_percent` from the listing; `offers[]` from the results row | List against charged price; coupons and Prime-exclusive prices change the real price |
| Social proof | `rating` and exact `ratings_count` from the listing; `is_amazon_choice`, `is_best_seller` from the results row | Stars and count per child since reviews split in 2026 |
| Demand floor | `bought_past_month` from the results row; the "Best Sellers Rank" row in `specs[]` | The badge's floor in buyers a month, not units, per child; the rank is today's only and compares within one category node (see [reading the data](data.md)) |
| Search footprint | `organic_keywords`, `organic_volume` | Breadth of searches it ranks on; a visibility proxy, never sales |
| Paid footprint | `sponsored_keywords`; `type` on `amazon_get_keywords` rows | How many searches it was seen advertising on, and which |
| Who sells it | `amazon_get_sellers` rows | Brand only, Amazon retail, or resellers; who holds the Buy Box (see [pricing and the Buy Box](buy-box.md)) |
| Content | `images[]`, `variation_asins[]`, the "Item Highlight" row in `specs[]` | Image count (a "play-button" entry is a video), variation range, whether Item Highlights are filled |

Then the gaps that matter: searches two or more rivals rank on that the user does not; use cases rivals claim that the user's product serves but does not say; price bands where the user is the only one or the most expensive; rivals with fewer ratings ranking above the user (their listing or price is doing the work).

About 80 credits for three rivals with competitor discovery and 300 keyword rows each (about 50 with the default 100 rows, which hold a rival's head terms). Date the table: results pages are cached a day, sellers six hours, keyword data comes from a periodic index.

## What not to conclude

- Ratings and review counts say little about recent sales; a listing can carry years of reviews and sell little now. The badge is the better recency signal and only a floor.
- `organic_volume` is the sum of monthly volumes of terms it ranks on at any position, over a million for big desk listings; position 40 on a big term inflates it. Compare it between rivals only.
- A `rank` from `amazon_get_keywords` is the position when the index last crawled that search, about two weeks back; take today's position from `amazon_search_products` before reporting it.
- A rival absent from sponsored rows on one page may be advertising elsewhere, on other days or in other placements the tool does not capture.
- No tool here shows a rival's sales, conversion, ad spend or Brand Analytics share. Say so rather than estimate.

## Reviews of rivals

No Manifold tool reads Amazon review text. Read the rival's 1 to 3 star reviews by hand on the listing (sort by most recent, filter by star), or have the user paste them; how to mine and weigh them is in [voice-of-customer](../../voice-of-customer/SKILL.md). Treat the rating as J-shaped: extremes over-post.

## Ethics

Read public pages only. Never buy a rival's product to leave a review, report a rival falsely, pose as a buyer to message a seller, or copy their images or copy text (Amazon enforces copyright and brand claims through Brand Registry).
