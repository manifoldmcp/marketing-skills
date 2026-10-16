# Reviews

How reviews work on Amazon in October 2026, how a seller may and may not get them, and what the law now says. No Manifold tool reads Amazon review text; ratings and counts come with the listing and results rows.

## Why they matter more now

- The assistant's picks were almost never below 4.0 stars and had a median of about 3,000 ratings (Amalytix, November 2024), and Amazon says it answers from reviews among other sources (see [search ranking](search-ranking.md#the-assistant-rufus-now-alexa-for-shopping)). A listing under 4.0 is unlikely to be recommended by it, whatever its rank.
- Amazon shows AI-written review summaries ("Customers say") on many listings. Recurring complaints surface there even when the average is high.

## Review sharing split by variation (2026)

- Amazon announced on 7 January 2026 that children of one parent stop sharing reviews when they differ in function, features, performance, materials, specifications or use case. Colour or pattern, size with the same function, pack count, and some scent and fitment variations still share. The rollout ran by category from 12 February to 31 May 2026, with 30 days' email notice per category.
- Effects: a family with thousands of pooled ratings can show a different rating and count per child in search; a new child that used to inherit the parent's stars now starts near zero; a parent built from unlike products to pool reviews loses the benefit. Fixing a wrong variation theme restores sharing for children that qualify. Amazon's Seller Assistant can say which children are eligible.
- When comparing ratings across listings after mid-2026, compare child to child.

## What a seller may do

- **Request a Review.** Amazon's button in Seller Central, or the automated equivalent, sends a neutral standard request after delivery. Asking for a positive review, asking only happy customers, or offering anything in return breaks Amazon's policy.
- **Vine.** Amazon's programme for brand owners (Brand Registry, FBA, fewer than 30 reviews on the parent). Up to 30 units per parent ASIN; in the US 1 to 2 units free, 3 to 10 units $75, 11 to 30 units $200 (Amazon's Seller Central news, for enrolments from 19 October 2023; unchanged in 2026 by the sellers' guides we found; check the Vine page before enrolling). Reviewers keep the product and are not asked for a positive review; reviews are labelled. Budget the units, inbound shipping and FBA fees as well as the fee. Vine buys honest early reviews, not good ones: a weak product gets its weak reviews faster.
- **Inserts and follow-up** may thank the buyer and point to support. They may not ask for a positive review or steer unhappy buyers away from reviewing.
- **Fix the product.** The fastest lasting rating gain is the defect the 1 to 3 star reviews name. Mining them is in [voice-of-customer](../../voice-of-customer/SKILL.md).

## What is illegal, not just against policy

- **The FTC's Trade Regulation Rule on Consumer Reviews and Testimonials** (16 CFR Part 465, in force since 21 October 2024) bans fake or AI-invented reviews, buying or selling reviews, incentives conditioned on sentiment, undisclosed insider reviews (staff, family), company-run "independent" review sites, suppressing negative reviews by threats, and buying fake followers or views. Civil penalties up to $53,088 per violation, counted per review. A company is liable for what agencies and third parties do on its behalf.
- **Enforcement has started.** The FTC sent its first warning letters under the rule to about ten companies on 22 December 2025, asking for a named owner and a remediation plan. We found no civil penalty under the rule as of October 2026.
- **Amazon enforces too.** Amazon says it blocked "hundreds of millions" of suspected fake reviews in 2025 (Trustworthy Shopping Experience report, 2026) and sues review brokers and the sellers who use them; it reported over 275 million blocked in 2024. Sellers caught lose reviews, listings or the account.
- **Not allowed whatever a vendor promises:** review clubs, rebate-after-review schemes, "free product for honest feedback" groups outside Vine, review swaps, paying for reviews of a rival's product, and messages asking happy buyers only.

## Never

Write, post, buy or solicit a review on the user's behalf, or contact a reviewer. Deliver what to fix and which permitted channel to use.

## Reading ratings in Manifold data

- `rating` and `ratings_count` on `amazon_get_product` and on `amazon_search_products` rows are ratings, which include star-only ratings without text, not written reviews. The results rows carry Amazon's rounded count (5,200 for 5,255); take exact counts from the listing.
- Colour children still pool: three colours of one Oral-B model showed the same 165,200 ratings in October 2026, while each child's `bought_past_month` differed. A child with its own small count after the 2026 split is a variation that differs in function or materials.
- Ratings are J-shaped: the very happy and very unhappy rate most. A 4.4 with 300 ratings and a 4.6 with 30 cannot be ranked against each other with confidence.
- Review text, review dates and verified-purchase flags are not available through Manifold; read them on the listing or ask the user for an export.
