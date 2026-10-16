# Pricing and the Buy Box

Who wins the featured offer on a listing, why, what resellers and Amazon itself do to a brand's price, and how to check it. Amazon calls the Buy Box the Featured Offer. Checked October 2026.

## How the featured offer is chosen now

- **No seller screen first.** Until mid-2026 Amazon checked each seller's account (performance metrics, tenure, order defect rate) before comparing offers; sellers who failed never competed. Amazon told sellers it would retire those seller-level eligibility requirements: the US rollout began in early July 2026, the EU and UK from 20 July, other stores through the end of 2026 (seller communication, as reported by beBOLD Digital). Offers now go straight into the comparison.
- **The comparison is unchanged in kind:** landed price (item plus shipping), delivery speed and promise, fulfilment reliability, stock, and seller performance. Amazon publishes the factors, not the weights. Consideration is not a win: a newer or weaker seller can now compete but still lose on delivery or performance.
- **The cheapest offer often loses.** A faster delivery promise or Prime eligibility can beat a lower price: on the Oral-B Pro 1000 on 16 October 2026, Amazon.com held the featured offer at $49.94 while an FBA reseller sat at rank 2 at $49.79 ([field notes](field-notes.md)). Third-party guides give thresholds (stay within 5% of the lowest offer, on-time delivery above 97%); Amazon has published neither, so treat them as rules of thumb.
- **Amazon can show no featured offer.** When every offer looks out of line with recent prices on Amazon or prices elsewhere, the listing shows "See all buying options" and conversion falls sharply. The FTC's monopolisation suit says Amazon uses this to punish sellers who price lower on other sites, setting a price floor everywhere; Amazon denies it. The bench trial is set for 29 March 2027 in the Western District of Washington (the judge refused Amazon's October 2026 request to delay). For a brand, the practical point: a deep discount on its own site or another marketplace can cost the Buy Box on Amazon.
- **Amazon retail on the listing.** When Amazon.com sells the product (as a vendor buys it, or sources it elsewhere), it competes for the featured offer like any seller and usually wins on delivery.

## Resellers

- Anyone can add an offer to an existing ASIN. Brand Registry gives the brand control of the listing content, not of who sells. Unauthorised resellers, grey-market stock and counterfeits are the usual causes of a lost Buy Box.
- Minimum advertised price (MAP) policies are agreements between the brand and its resellers; Amazon does not enforce them. A brand's remedies are its distribution contracts, Brand Registry reports for counterfeit or listing abuse, and Transparency or similar serialisation programmes.
- A reseller winning the Buy Box below the brand's price can be legitimate (old stock, a clearance) or not. Read the seller page before calling it counterfeit, and never file a false report.

## Checking it with Manifold

- `amazon_get_sellers` (2 credits, cached 6 hours) on each ASIN: one row per offer with `rank` (1 holds the Buy Box), `seller`, `ships_from`, `price`, `regular_price`, `condition` and the `delivery` promise.
- `ships_from` reads "Amazon.com" for an order Amazon fulfils; `seller` "Amazon.com" is Amazon retail (with no `seller_url`). For a third-party offer, isAmazonFulfilled=1 or 0 inside `seller_url` says FBA or merchant-fulfilled. `seller` is the account's store name, which may not match the brand: a brand-only listing can show one offer from a company name the user has to recognise. A rank-1 offer below the brand's own price, from someone else, is the alert. Several sellers clustered within a few cents means a repricer war.
- **No featured offer** returns `has_featured_offer` false, no rank-1 row, the other offers from rank 2, and `price` null on the product's results row. That is the "See all buying options" state at fetch time; check whether a cheaper price elsewhere (below) could be the trigger.
- The tool reads the listing at one moment for one marketplace and delivery address; the Buy Box can rotate between sellers within a day. Re-check after the six-hour cache before reporting a change, and report "held the Buy Box at 14:00 UTC", not "owns the Buy Box".
- **Prices across other stores** (to see whether a cheaper offer elsewhere could be the trigger) are `shopping_get_sellers` and the [google-shopping](../../google-shopping/SKILL.md) skill.
- For a watch: the host stores each run per ASIN (date, rank-1 seller, price, the brand's own price, number of offers) and flags a new rank-1 seller, a rank-1 price below the floor the user set, or Amazon.com appearing. Manifold keeps no history and has no scheduler. A daily check of 20 ASINs is 40 credits.

## Pricing reads that hold up

- Compare prices on the same day and say the date: `amazon_search_products` caches a day and prices move with coupons, Lightning Deals and Prime-exclusive prices (visible in `offers[]` as badges, not as a separate price).
- Compare like-for-like children: each child has its own price. On `amazon_get_product`, `regular_price` is the struck-through list price and `discount_percent` the cut from it; no field gives a range across variations.
- Tariff and fee changes in 2025 and 2026 moved shelf prices in many categories; a year-old price comparison is stale.

## Without the connector

Open the listing in a private window with a delivery postcode in the target market, read who the featured offer is "Sold by" and "Ships from", and open "Other sellers on Amazon" for the rest. Repeat at different times of day to see rotation.
