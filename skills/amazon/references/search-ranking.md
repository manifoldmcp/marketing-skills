# How Amazon search ranks products

What Amazon has said about ranking, what its research shows, how the shopping assistant changed discovery, and where the folklore runs ahead of the evidence. Checked October 2026.

## What Amazon says

- **The named inputs are broad.** Amazon's seller guidance, as quoted across the industry, names degree of text match, price, availability, selection and sales history as what decides where a product appears. Amazon publishes no weights and has never used the name "A10"; A9 was the name of an Amazon search subsidiary, and "A10" is a seller-tool label for whatever the ranking does now. Any article giving A10 weights, or saying rankings move "within hours", is guessing.
- **Purchases are an explicit objective.** Amazon's ranking papers train on implicit feedback (clicks, add-to-carts, purchases), and its 2026 Search Relevance job postings describe models that jointly optimise purchase, relevance and personalisation. That is the documented basis for the common advice that conversion and sales velocity matter: a listing that converts the traffic it gets earns more of it. How much each signal weighs, and over what window, is not public.
- **Availability gates everything.** An out-of-stock ASIN cannot convert, and practitioners consistently report rank falling with stock-outs. Inventory planning is ranking work.
- **Results are personalised and vary by marketplace, device and address.** One results page is a sample. `amazon_search_products` reads a logged-out page for one country.

## Semantic matching: COSMO

- COSMO (Yu and others, SIGMOD Companion 2024, Amazon) is a commonsense knowledge graph built by language models from shopping behaviour, then filtered by human annotation: triples such as product type, relation, context ("slip-resistant shoes", used for, "pregnant women"). The paper says it is deployed in Amazon search. Vendor write-ups cite a 60% relevance gain and a 0.7% sales lift in an A/B test; those figures are not confirmed in the paper excerpts we could read, so do not quote them.
- What it means for a listing: Amazon matches intent ("desk for a small apartment", "gift for a runner") to products whose attributes and copy say who and what they are for. Exact-match keyword stuffing matters less than it did; the use case, audience, material and fit, stated plainly in the title, Item Highlights, bullets and the structured attribute fields, matter more. Blank attribute fields are the cheapest gap to close (see [listing audit](listing-audit.md)).

## The assistant: Rufus, now Alexa for Shopping

- **Timeline.** Rufus launched in 2024. Amazon said more than 300 million customers used it in 2025 and that it drove nearly $12B of incremental annualised sales (Q4 2025 results, February 2026); the attribution is Amazon's own, counting a sale for seven days after a Rufus interaction. On 13 May 2026 Amazon renamed it Alexa for Shopping, merged it with Alexa+, and put conversational queries into the main search bar, results pages and product pages. On the 29 April 2026 call Amazon reported active users up 115% year on year.
- **Its picks do not follow the rankings.** Autopilotbrand captured 12,810 recommendations for 1,963 non-branded best-of questions (May to June 2026, one US account, reported by Marketplace Pulse, 16 July 2026). 36.1% of picks ranked in the organic top 10 for the bare category search, 40.9% never appeared on the visible results page, and only 14.3% were advertised on that page (83% of those also ranked organically). On long-tail questions 80.2% of picks were outside the top 10. Picks showed a median "bought" signal of about 3,000 against 900 for surrounding listings. One account, one snapshot, observational.
- **What the picks have in common.** In Amalytix's study of 1,300+ Rufus picks across 500 top US search terms (November 2024), none was rated below 4.0 stars, the median was 4.5 with 2,991 ratings, and about 94% were FBA and 98% in stock. Correlations, not rules, and before the rename.
- **What Amazon says it reads.** Amazon's help page says the assistant answers from product detail pages, reviews, community Q&A and information from across the web. A listing that answers the questions buyers ask (dimensions, compatibility, who it suits, what it does not do) gives it material; claims the reviews contradict work against it.
- **Ads are in it.** Existing sponsored campaigns serve inside Alexa for Shopping automatically, and sponsored prompts are billable (see [ads](ads.md)).

So there are now two shelves: the ranked results page and the assistant's short list. Rank still matters for the page; for the assistant, rating, review volume, complete attributes and a listing that answers questions matter more than position.

## Folklore to drop

- **"Repeat the keyword."** Titles cap repeats at two and length at 75 characters; backend terms gain nothing from repetition.
- **"Ranking is all about PPC sales."** Ads bring traffic that can convert into organic rank, but no public evidence separates the two, and the assistant's picks barely track ad presence.
- **"External traffic is rewarded with rank."** Amazon pays a Brand Referral Bonus on sales from outside traffic (see [fees](fees.md)); no Amazon source says it lifts organic rank.
- **"New listings get a honeymoon period."** Widely repeated, never measured. New listings can reach the top 10 on a head term (one with no ratings ranked third for "standing desk" in Manifold's read of 16 October 2026, [field notes](field-notes.md)); one snapshot says nothing about how long they stay or why.

## What Manifold can show

`amazon_search_products` shows who holds a results page now, organic and sponsored, with ratings and badges; `amazon_get_keywords` shows the searches an ASIN appeared on when Labs last crawled them (about two weeks earlier in our calls; take positions from the results page, see [reading the data](data.md)). The current Best Sellers Rank is a row in `specs[]` on `amazon_get_product`. Neither shows the assistant's picks: no Manifold tool queries Alexa for Shopping. To see them, ask the assistant the buyer's question on the right marketplace in several wordings, ideally from an account with little shopping history (Amazon says it draws on past orders and Alexa conversations), and log the products named. Report it as a small, personalised sample.
