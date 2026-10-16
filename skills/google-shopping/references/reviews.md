# Ratings and review mining

The rules for showing stars on Google, what review collection is legal, and how to mine Shopping reviews of the user's and competitors' products. Checked October 2026.

## Two kinds of stars

- **Product ratings** (stars on a product): from a reviews feed sent by the merchant or an aggregator, or from Google Customer Reviews. They need at least 50 reviews across all the merchant's products before any show; the feed must be refreshed at least monthly; product identifiers must match the product feed and review URLs must be on the Merchant Center domain. Onboarding takes weeks ([Google, product ratings eligibility](https://support.google.com/merchants/answer/14549080)). A per-product minimum of 3 is a third-party claim Google does not state.
- **Store ratings** (stars on the store): about 100 eligible, unique reviews in a rolling 24-month window, calculated per country, and an average of at least 3.5 for star assets to show with text and Shopping ads ([Search Engine Roundtable](https://www.seroundtable.com/google-merchant-center-google-store-ratings-42125.html), 22 September 2026, summarising Google's help page). Reviews age out, so collection has to be continuous.
- **Google pools sources.** Ratings combine Google Customer Reviews, merchants' and third-party providers' reviews; providers must send all reviews including negative ones, and some may have paid reviewers. A product's rating on Google can differ from the one on the user's site.
- **The store widget** (September 2025) puts the Google store rating, shipping and returns on the user's own site in three tiers (Top Quality store, store rating, generic). Google claims up to 8% higher sales within 90 days for sites using it, its own figure, unverified.
- ChatGPT's feed carries `review_count` and `star_rating` for products and separate store rating fields; A WARC summary of Profound's research (September 2026) names ChatGPT's offer tags (such as "Best price") and median review count as the two strongest predictors of product card rank; observational, and the original figures were not published in Profound's own post.

## Collecting reviews legally

- **The FTC's Consumer Review Rule** has been in force since October 2024, and the FTC sent warning letters to about ten companies on 22 December 2025, with penalties of up to $53,088 per violation. Banned: fake or bought reviews, incentives tied to a positive rating, undisclosed insider reviews, a company-run review site presented as independent, and suppressing or gating negative reviews, including tools that ask only 4- and 5-star customers to post publicly.
- Google requires all reviews, positive and negative, in a reviews feed. Asking every buyer after delivery is allowed; asking only happy ones is not.
- Never write, buy or seed reviews for the user, and never suggest it.

## Mining reviews

The job: what buyers of a product (the user's or a competitor's) praise and complain about, in their own words.

1. **Pick the products:** the user's top sellers and the two or three competitor products that win the same queries ([ranking](ranking.md#who-wins-a-product-search)).
2. **Pull reviews:** `shopping_get_reviews` with `product_id`, `gid` and `data_docid` from `shopping_search_products`. `limit` defaults to 100 (5 credits), up to 1,000 (50); billed on reviews returned and rounded up (50 cost 3); cached 7 days.
3. **Read the shape first:** `rating`, `ratings_count` and `stars[]` (count per star). Then rows: `rating`, `title`, `text`, `source` (the store the review came from) and `published_at`. Quote the star distribution from `stars[]`, never from the rows.
4. **Code themes,** separately for 1 to 2 stars and 4 to 5 stars: what fails (assembly, motor, durability, fit), what delights, what buyers compare it with, and what they say the listing got wrong. Count reviewers per theme, quote two or three lines each with the source store.
5. **Turn it into work:** listing copy and Q&A that answer the top doubts (`question_and_answer` in the feed, see [feed audit](feed-audit.md)), product fixes for the user, angles against a competitor's weak spots.

**Reading it right:**

- **Reviews are pooled across stores and countries.** On US queries, about 30 of 100 reviews of an espresso machine came from Canadian, Australian, Swiss and UK stores, and half of 50 reviews of a running shoe from the brand's Australian and New Zealand stores; one source was a store's demo site ([field notes](field-notes.md)). Read `source` before calling a theme American, and drop sources that are not real stores.
- **The rows lean negative.** One-star reviews were 12% and 10% of the rows on two products against 7% and 5% in their `stars[]`. The rows are Google's selection, not a random sample: rank themes by how often they recur, and take satisfaction from `stars[]`.
- **`published_at` is good to the month.** Google shows relative dates; every row comes back with the crawl's day of month and time of day, and everything a year old or more lands exactly one year back (a quarter of one product's rows). Do not build a weekly trend on them, or read a pile at twelve months as a spike.
- **`ratings_count` is ratings, not texts.** A product with thousands of ratings may return far fewer written reviews.
- **Check `stars[]` against `ratings_count`** before quoting a distribution: one product's star counts summed to 99% of it, another's to 125%. Quote shares of the `stars[]` total, and treat a missing star level as missing data, not zero.
- **Review sets are J-shaped:** extremes get written. Frequency ranks themes against each other, never market-wide satisfaction. For the method of ranking pains, see [voice-of-customer](../../voice-of-customer/SKILL.md).
- **Amazon reviews are not available** through any Manifold tool; app reviews are `apps_get_reviews`. Reviewer names are personal data: leave them out of deliverables.

## Without the connector

Open the product in the Shopping tab, open its reviews, sort by lowest rating and by newest, and note 30 to 50 reviews with their store and star rating.
