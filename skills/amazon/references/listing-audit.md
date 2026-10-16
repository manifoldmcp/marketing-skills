# Listing audit

The rules a listing must meet as of October 2026, what helps it rank and convert, and how to audit one against the listings that beat it. Amazon changes these rules often and category style guides add their own: check Seller Central's product title requirements and the category style guide before telling a user a field is wrong.

## The rules now

**Title.**
- 75 characters including spaces in every category except media (books, music, video), from 27 July 2026, in every Amazon store except Saudi Arabia, Egypt, Türkiye and the UAE (announced on Seller Forums, updated 10 June 2026). Amazon's stated reasons: titles should display in full on mobile and match other retailers.
- Titles still over the limit are rewritten gradually by Amazon's AI; listings stay live, brand owners can review, change or approve the recommendation within 14 days, and applied changes show in View Change History. A seller without Brand Registry may not get the review window. Check every ASIN's title after the deadline: a silent AI rewrite can drop the term that sold it. The rewrite is slow and uneven: on 16 October 2026 a third of the top organic titles on two US searches were still over 75 characters, big brands among them ([field notes](field-notes.md)). Do not copy a rival's long title as proof the cap is not enforced; the user's own long title is the one at risk of an unreviewed rewrite.
- Still in force from 21 January 2025: no `! $ ? _ { } ^ ¬ ¦` unless part of the brand name; no word more than twice (articles, prepositions and conjunctions excepted; Amazon treats singular and plural as the same word); no promotional phrases ("best seller", "free shipping"). Hyphens, colons and brackets are allowed where they do a job.
- What fits in 75: brand, product type, the one or two attributes that decide the click (size, count, key material or compatibility). Everything else goes to Item Highlights.

**Item Highlights.** Up to 125 characters, searchable, shown with the title in results and on the detail page; Amazon's Enhance Listings tool suggests them and moves overflow from the title. Amazon's FAQ, as reported, says Item Highlights and the title are both search inputs with neither prioritised. Some sellers hit error 100476 ("attribute currently unsupported") through feeds and the API in the first weeks; if the field will not save, say so rather than assuming it is live.

**Bullets and description.** Amazon's assistant answers from the detail page, reviews and Q&A, so bullets that answer real buyer questions (fit, dimensions, compatibility, what is in the box, who it is not for) feed both the shopper and the assistant. Unsupported claims a review contradicts hurt.

**Backend search terms.** Under 250 bytes in the US (bytes, not characters: accented letters and symbols take more; Amazon's Seller Central news, August 2018). Amazon's announcement, as quoted on its seller forums, says an entry over the limit is rejected whole, and sellers report the field then drops from indexing. Spaces separate terms; no commas, no repeats of words already on the page, no brand names including competitors', no ASINs, nothing offensive or untrue.

**Structured attributes.** Material, intended use, audience, size and the category's other fields feed matching (see [search ranking](search-ranking.md#semantic-matching-cosmo)). An attribute left blank is a search the listing cannot match. These fields are not visible on the public page; the user checks them in Seller Central.

**Images.**
- Main image: pure white background (RGB 255, 255, 255), the product only, filling about 85% of the frame, no text, logos, badges, borders or props not included. At least 1,000 pixels on the longest side for zoom; most sellers use 1,600 to 2,000.
- AI-generated photorealistic people in images or video must carry the keyword `contains-synthetic-performer` in the file's XMP subject metadata, and Amazon shows a disclosure (reported July 2026, worldwide, listings and A+ content). AI images must show the exact product shipped. Exporting can strip the metadata; check the file before upload.

**Variations.** Since the 2026 change, children that differ in function, materials or use no longer share reviews (see [reviews](reviews.md)). A wrong variation theme now costs ratings; fixing the theme restores sharing for children that qualify.

## Auditing a listing

1. **The listing:** `amazon_get_product` (2 credits) on the user's ASIN: title and its length, `description` (the About-this-item bullets), `specs[]` (look for the "Item Highlight" row, which is missing when the field is empty, and the "Best Sellers Rank" row), `images[]` count (less any "play-button" video entry), `variation_asins[]`, `rating` and `ratings_count`, `is_available`. The attribute rows in `specs[]` are what the page shows, not every attribute in Seller Central, and they can hold seller errors worth fixing. A+ content and backend fields are not returned; ask the user.
2. **What it ranks for:** `amazon_get_keywords` (10 credits for 100 rows, biggest search first; pull more before calling a low-volume core term missing). Core terms from [keyword research](keyword-research.md) that it does not rank for, or ranks only as sponsored, are the gaps; confirm a gap on the results page before reporting it, since the index lags about two weeks.
3. **What beats it:** `amazon_search_products` on two or three core terms (2 credits each), then `amazon_get_product` on the top three organic rivals. Compare side by side: the words in the first 75 characters, image count, rating and count, price, badges (Amazon's Choice, Best Seller, coupons in `offers[]`), and what their bullets answer that the user's do not.
4. **Rules check:** title length and banned characters, repeats, promotional words, main image rules as far as the image file shows them.

Deliver a table per field: now, issue, fix, and the evidence (the rival or rule behind it). Rank fixes: a broken rule or a suppressed field first, then the title's first 75 characters, then missing use cases and attributes, then images, then everything else. About 35 credits with 300 keyword rows. Never edit the listing; the user applies changes in Seller Central, ideally one field at a time so a rank change can be traced, and not in the days before a big sales event.

## Without the connector

Open the listing and the top three organic rivals on the right marketplace in a private window and fill the same table by hand. The title, bullets, images, rating, price and badges are all visible; attributes and backend terms are only in the user's Seller Central.
