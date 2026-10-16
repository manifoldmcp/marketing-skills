# Messaging and pricing

How each competitor describes itself and what it charges, side by side: the promise, the buyer it names, the proof, the offer it pays to advertise, and the pricing model. The messaging comes from the tools; prices come only from the pricing page itself. This table is what positioning work and comparison pages build on. Checked October 2026.

## Where each claim lives

| Source | Read | Tells you |
|---|---|---|
| Homepage (`seo_get_page`, free) | `h1[]`, `meta_description`, `h2[]` | The promise, the one-line pitch, the feature pillars and proof. When the `h1` is generic ("Work better, together"), the `h2[]` and meta description carry the real claim |
| Pricing page (`seo_get_page`, free; `/pricing`, then `/plans`) | Headings | The plan names and packaging, not the prices. A 404, or a `final_url` on a demo page, means no public pricing |
| Active ads (`ads_get_advertiser_ads`, 1 a page per library) | `headline`, `body`, `cta`, `destination_url` | The offer it is paying to test now. Google rows carry no text: `ads_get_ad` with the row's `url` (1 each) on the two or three longest-running ads beats `details: true` at 25 a page |
| Bios (`linkedin_get_company`, `leads_get_company`, one or two social profiles, 1 each) | `bio`, `description`, `keywords[]` | The shortest positioning statement it writes. The LinkedIn bio is current; the company record's copy can be old |
| AI answers (`aeo_run_ai_answers`, "What is <rival> and who is it best for?", `engines: ["chatgpt", "gemini"]`, 4 a prompt) | `answer` | The pitch buyers hear about it, which can differ from the one it writes |

Default cost: about 14 credits a competitor, 56 for four plus 14 for the user's own row.

## Reading it

- **Ads outrank the homepage as evidence.** The homepage is the promise; an ad still running after 90 days is the message that converts for them.
- **Repetition is commitment.** Words a competitor repeats across its page, ads and bios are the message it has committed to and will defend.
- **Engines can pitch a rival differently from how it pitches itself.** Where an engine's category, buyer or price level differs from the rival's own `h1`, buyers hear the engine's version first. In our October 2026 run, engines gave different entry prices for the same products (Close "from $49" on two engines, automation "from $99" on another; monday "from $9" and "from $12"). Never carry an AI-stated price into the table.
- **Then two lists:** the claims every competitor makes (table stakes, which the user must match but cannot win on) and the claims nobody makes (open ground). Copying the leader's message makes the user the cheaper copy.

## Prices

- No Manifold tool reads prices or body text. If the host has a browser or fetch tool, open each pricing page and take plan names, price per plan, billing unit (per seat, usage, flat), free plan or trial, the limit that separates tiers, and the annual discount. Otherwise ask the user to paste the pages. If neither, write "not read"; never estimate a price from memory, an ad, a review site or an AI answer.
- Compare on one example, not across units: the cost for the user's typical customer (a 10-seat team, 5,000 contacts) on each rival's plan that fits.
- Hidden pricing is a finding: a sales-led model with a call before any price. Say so; do not guess.
- Date every price. Prices change, and an old one on a battlecard loses a rep's credibility in one call.
- `seo_get_page` fetches raw HTML: a bot wall or JavaScript-rendered headings come back empty. The host's browser or the user's paste is the fallback.

## Deliver

One row per competitor with the user last: category claimed, buyer named, how engines describe it, promise (the `h1`, quoted with its URL), proof, call to action, offer in ads, pricing model with entry price, the source of each price (read by the host, pasted, or "not read") and its date. Below it, the table-stakes claims and the open claims. Turning the open claims into positioning is [voice-of-customer](../../voice-of-customer/SKILL.md); comparison pages are [seo](../../seo/SKILL.md).
