# Messaging and pricing

How each competitor describes itself and what it charges, side by side: the promise it leads with, the buyer it names, the proof it leans on, the offer it pays to advertise, and its pricing model. The messaging comes from the tools; the prices come only from the pricing page itself, read by the host or pasted by the user. It ends in the table that [positioning](positioning.md) and the `seo` group's [comparison pages](../../seo/references/comparison-pages.md) build on.

## Inputs to settle first

- **Competitors**: two to five domains. Default: the direct competitors from [find competitors](find-competitors.md).
- **The user's site**: include it as the last row, so the gaps show.
- **Pages**: the homepage and the pricing page (default `/pricing`; try `/plans` if that fails), plus one landing page per competitor if the user names one.
- **Pricing text**: whether the host can open web pages (a browser or fetch tool), or whether the user will paste each pricing page. Ask before step 2, since no manifold tool reads prices.
- **Market**: `country` for the ad libraries if not the United States.
- **Budget**: a default run costs about 4 x (3 + 3 + 1 + 2 + 1) = 40 credits for four competitors, and 10 more for the user's own row; the pages are free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the pages.** `seo_get_page` (free, rate limited) on each homepage and pricing page. Take `title`, `meta_description`, `h1[]` and `h2[]`. The `h1` is the promise, the meta description the one-sentence pitch, and the `h2[]` the proof and the feature pillars. On the pricing page, the headings often name the plans (Free, Starter, Pro, Enterprise): that is the packaging, not the price. A `status` of 404, or a `final_url` on a demo or contact page, means there is no public pricing.
2. **Read the prices.** No manifold tool reads page body text or prices. If the host has a browser or fetch tool, open each pricing page and take the plan names, the price of each, the billing unit (per seat, per usage, flat), the free plan or trial, the limit or feature that separates the tiers, and the annual discount. Otherwise ask the user to paste the pages. If neither is possible, fill the price columns with "not read"; never estimate a price from memory, from an ad or from a review site.
3. **Read the ads.** `ads_get_advertiser_ads` with `active_only: true` on `facebook` (the brand name), `linkedin` (the company name) and `google` (the domain), 1 credit a page each. Only Facebook applies `active_only`; on the other libraries keep the rows whose `active` is true or whose `last_shown` is recent. Read `headline`, `body`, `cta` and `destination_url`: ads carry the offer the competitor pays to test (a free trial, a demo, a discount, a template) and the angle (a pain, an outcome, a comparison). Google rows carry no creative text without `details: true` (25 credits), so call `ads_get_ad` with the ad URL (1 credit each) on the two or three that have run longest instead. The full ad teardown is [paid-ads competitor ads](../../paid-ads/references/competitor-ads.md).
4. **Read the bios.** `linkedin_get_company` (1 credit) for its `bio`; `leads_get_company` (1 credit) for `description` and `keywords[]`; and the profile of the one or two social accounts the competitor uses most (1 credit each: `instagram_get_profile`, `tiktok_get_profile`, `twitter_get_profile` or `youtube_get_channel`) for their `bio`. A bio is the shortest statement of positioning a company writes.
5. **Code each competitor.** From steps 1 to 4, fill in: the category word it uses for itself, the buyer it names, the main promise, the proof (numbers, customer logos, awards in the headings), the main call to action (trial, demo, sign up, contact sales), and the offer in its ads. Mark the words a competitor repeats across its page, ads and bios: that is the message it has committed to. Then list the claims every competitor makes (table stakes) and the claims nobody makes.
6. **Deliver** a table with one row per competitor and the user last: category claimed, buyer named, promise (the `h1`, quoted), proof, call to action, offer in ads, pricing model (free plan, trial, billing unit, entry price) with the source of each price (read by the host, pasted, or "not read") and the date, and the URL of each page read. Below it, two lists: the table-stakes claims, and the open claims no competitor makes.

## Judgment

- When an `h1` is generic ("Work better, together"), the `h2[]` and the meta description carry the real claim. Quote those instead.
- The homepage is the brand's promise; the ads are the offers it is testing. An ad still running after 90 days is most likely the message that converts for them, and it outranks the homepage as evidence.
- Hidden pricing is a finding, not a gap: it means a sales-led model with a demo call before any price. Say so; do not guess the number.
- Compare prices on one example, not across units: the price for the user's typical customer (say, a 10-seat team or 5,000 contacts a month) on each competitor's plan that fits.
- Prices change. Date every price, and never carry one over from an earlier conversation or a model's memory.
- `seo_get_page` fetches the raw page. A site behind a bot wall, or one that renders its headings with JavaScript, can come back with empty headings; the host's browser or the user's paste is the fallback.
- Quote short. A headline or a tagline is evidence; a copied paragraph is not needed.
