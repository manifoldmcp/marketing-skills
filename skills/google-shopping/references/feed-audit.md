# Feed and listing audit

What a product feed needs in 2026 for Google and for ChatGPT, what changed, and how to audit listings from the outside. Checked October 2026. Google's [product data specification](https://support.google.com/merchants/answer/7052112) and OpenAI's [feed reference](https://developers.openai.com/commerce/specs/feed) are the authorities; re-check them before quoting a field rule to a client.

## What changed in 2025 and 2026

- **The Content API for Shopping shut down on 18 August 2026.** Feeds pushed through it had to move to the Merchant API (generally available August 2025). A shop whose products fell out of Shopping and Performance Max around that date on a custom integration should check this first.
- **"Merchant Center Next" is now just Google Merchant Center** (July 2026), and the Shopping ads and free listings policies merged into one set of Shopping policies in September 2026, with no intended change of substance.
- **The April 2026 specification update** added a handling cutoff time, loyalty program and tier labels inside the shipping attribute, a video link attribute, and a product-level minimum order value; smaller images are auto-upscaled with a warning. In the UK, Switzerland and the EEA, pickup cost and minimum order value became required for in-store pickup products from 30 September 2026.
- **Six conversational attributes (May 2026, Google Marketing Live):** `question_and_answer` (up to 30 Q&A pairs), `related_product` (accessory, substitute, part of set and others), `document_link` (up to five PDFs such as manuals), `item_group_title`, `variant_option` and `popularity_rank` (a percentile within the catalogue). Optional, sent in a supplemental feed or the Merchant API, no effect on approval. Google documents no ranking effect; they give AI Mode facts to answer questions with ([Productsup](https://www.productsup.com/blog/google-introduces-six-conversational-attributes-in-merchant-center-here-what-you-need-to-know/), June 2026).
- **Loyalty:** member prices and shipping perks can show on listings (expanded March 2026); from September 2026 any merchant can connect a loyalty programme through the Merchant API, without being an advertiser.
- **AI performance insights** in Merchant Center shows which attributes are missing for the terms shoppers use in AI Mode and AI Overviews ([AI shopping answers](ai-shopping.md#google-ai-mode-ai-overviews-and-gemini)).

## Google feed checklist, in order of what breaks first

1. **Free listings on.** In Merchant Center the free listings surfaces must be enabled; AI Mode and AI Overviews product results draw on them. Feed vendors name it the commonest cause of a shop missing from AI Mode.
2. **Required attributes valid:** `id`, `title`, `description`, `link`, `image_link`, `price`, `availability`, plus `brand` and `gtin` where the product has them. Disapprovals and account warnings in Merchant Center's diagnostics come before any optimisation.
3. **GTIN correct.** It groups the user's offer with other stores' offers of the same product and enables the price benchmark. A wrong GTIN is worse than none.
4. **Price and availability match the landing page.** Mismatches are the classic cause of disapproval and misrepresentation flags; the misrepresentation policy was clarified in October 2025.
5. **Titles in shopper words:** product type, brand, model, and the attribute shoppers filter by (size, colour, capacity), front-loaded. Google's own AI guidance is to add shopper search terms to titles and descriptions.
6. **Attributes filled:** colour, size, material, age group, gender, product highlights, product details. Fill what AI performance insights says is missing, top terms first. Vendor claims of 37% more impressions from enrichment (8 accounts, no method) are directional at best.
7. **Shipping and returns** in Merchant Center, or as Organization-level structured data on the site, so delivery cost, speed and return window show on the listing.
8. **Images:** clean main image; lifestyle and video as extras.

## Structured data on the site

- Merchant listing markup (`Product` with `Offer`) lets Google read price, availability, shipping and returns from the page, and supports listings on the site's own pages in search. Merchant-wide return and shipping policies belong in Organization markup; offer-level return policies support only a subset (Google, July 2025).
- Loyalty programmes use the `MemberProgram` type at Organization level, with member prices as a separate price specification on the Offer (June 2025). Loyalty shipping and return perks are not eligible for enhanced display.
- Markup does not replace the feed for Shopping, and schema has tested as doing nothing for AI citations generally ([ai-search](../../ai-search/SKILL.md)). Use it for accuracy, not as a ranking lever.
- `seo_get_page` (free) shows the page's schema types and whether the markup is there.

## A ChatGPT feed

- **Required fields:** `item_id`, `title`, `description`, `url`, `brand`, `seller_name`, `image_url`, `availability`, `price`. Recommended: `gtin`, `mpn`, `sale_price`, shipping, `review_count` and `star_rating` (product reviews only; store ratings need separate setup), return fields. Raw reviews and Q&A lists are not part of the discovery data.
- **Flags:** `is_eligible_search` (default true) and `is_eligible_checkout` (off unless checkout onboarding is done). Eligibility does not guarantee display.
- **Format:** OpenAI's own format (JSONL in its examples) or, once OpenAI confirms it, a Google-style tab- or comma-delimited file. Update it when a sale starts or ends and when stock changes. Full snapshots, not deltas.
- **Who is already in:** Shopify stores through Shopify Catalog and Etsy shops, with no action. Others apply (waitlisted as of September 2026) or connect through a feed provider that speaks the Agentic Commerce Protocol. See [AI shopping answers](ai-shopping.md#chatgpt).

## Auditing listings from the outside

No Manifold tool reads the user's Merchant Center, feed file or diagnostics; ask for a feed export and the diagnostics page. What the tools show is what Google displays:

- **Title and specs as Google shows them:** `shopping_get_product` (1 credit) returns `title`, `description`, `images[]`, `rating`, `ratings_count` and `specs[]` (name and value, ungrouped). Compare the user's product with the top three competitors for the same query: missing specs, fewer images, no rating. Google builds the spec list from many sources (one shoe had 76 specs, two dozen of them "No" flags such as golf shoes, and the size of one variant), and the `description` and `url` are one store's, not necessarily the brand's: compare spec coverage, not wording.
- **Whether a product is grouped with other stores:** `shopping_get_sellers` returning only the user's own store for a product others sell suggests a GTIN or matching problem. The list is Google's first stores, up to 20 and often 5 to 10, so absence from it is not proof the user is ungrouped; check the user's own row's ids too.
- **Badges and delivery:** `tags[]` and `delivery` on `shopping_search_products` rows show discount badges ("15% OFF", "LOW PRICE") and delivery promises ("Free delivery by Thu", "Free delivery on $99+") competitors carry and the user lacks.
- Report a gap table per product (attribute or element, user, competitor best, fix) and leave the edits to the user.
