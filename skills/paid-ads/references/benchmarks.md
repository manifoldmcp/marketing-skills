# Benchmarks

What clicks and impressions cost on each platform, from the largest datasets published in 2025 and 2026, and how to use a benchmark without misleading anyone. Checked October 2026. Every figure here is a median or average across other advertisers; the user's own account is the only number that decides anything.

## The numbers

| Platform | Figure | Source, sample, period |
|---|---|---|
| Google Search (with Microsoft) | CPC $5.42, CTR 6.64%, conversion rate 8.18%, cost per lead $66.69 | WordStream/LocaliQ, 13,474 US campaigns, April 2025 to March 2026, medians (May 2026) |
| Google Search by industry | CPC from $1.63 (arts and entertainment) to $9.87 (legal); cost per lead from $26.84 to $131.63 (legal) | Same report |
| Meta | Median CPM about $13.50 to $15 | Synter, 80 accounts, H1 2026 ($13.51); Triple Whale's overall Meta median ($15.06), cited in its 2026 TikTok benchmarks |
| Meta, 2025 trend | CPM up about 20% in 2025, CTR up about 13% | Triple Whale, about 35,000 ad accounts, full-year 2025, as summarised by a third party |
| TikTok | Median CPM $4.08 (-28% YoY), CTR 0.61%, conversion rate 1.56% (-37%), CPA $17.07, ROAS 1.51 | Triple Whale, 5,900 brands, August 2025 to July 2026 (published October 2026) |
| LinkedIn | CPC about €6, CPM about €34; 121% company-level ROAS | Dreamdata, its B2B customers, August 2024 to July 2025 (March 2026); CPC and CPM from a summary of the report |
| ChatGPT | Launch CPM $60, later reported near $25; CPC bids recommended at $3 to $5; US CTR 0.73% | OpenAI pilot terms as reported (February to May 2026); Similarweb panel, March to September 2026. See [AI answer ads](ai-answer-ads.md) |
| Google, with an AI Overview on the page | Paid CTR 15% to 16%, against 21% to 26% without one | Seer Interactive, 2025 |

## What the numbers say

- **Google CPCs keep rising (up about 3% to $5.42) while cost per lead fell for the first time in about five years.** Conversion rates rose with automation. The WordStream sample is small and mid-sized businesses on its own platform; enterprise and SaaS keywords run far higher (in our data, "crm software" shows a $37 average CPC and "car insurance quotes" $157).
- **Meta got dearer in 2025, TikTok cheaper in 2026.** A Meta impression costs three to four times a TikTok one, but TikTok's conversion rate and CTR fell over the year. Compare cost per result, not CPM.
- **LinkedIn clicks cost several times Meta's,** and the payback runs over months (272 days first touch to revenue in Dreamdata's data). Judge it on pipeline over two quarters, not 30-day CPA.
- **ChatGPT's CTR is about a ninth of Google Search's.** Its CPM at launch was three times Meta's. It is a test channel, not a benchmark-driven one, in 2026.

## Reading benchmarks honestly

- **Name the source and its sample in every deliverable.** Triple Whale's data skews to Shopify ecommerce brands, WordStream's to smaller US advertisers on its platform, Dreamdata's to B2B SaaS on its attribution model, Synter's to a few dozen accounts. None is "the market".
- **Medians hide a tenfold spread.** Synter's Q1 2026 Meta CPMs ran from $3.46 at the 10th percentile to $57.79 at the 90th. A user 40% off the median may be normal for its vertical, country or season.
- **CPM and CPC move with season.** Q4 auctions are the dearest of the year on Meta and Google; a test started in November reads high.
- **Vendor round-ups recycle the same few datasets.** Ten blog posts quoting a $14 Meta CPM are usually one source. Trace a figure to its dataset before quoting it, and drop it if there is none.
- **The decision number is the user's allowable cost per result,** not a benchmark. With none given, a rule of thumb: allowable cost per customer = first-year revenue per customer x gross margin ÷ 2 (about a six-month payback). Benchmarks only say whether reaching it is plausible.

## Search prices from Manifold

- `seo_search_keywords` and `seo_get_keyword_metrics` return `cpc`, `competition`, `bid_low` and `bid_high` per keyword. `bid_low` and `bid_high` are Google's top-of-page bid range; `cpc` is a modelled average from past auctions, and they do not always agree (in our data "hubspot alternative" showed a $118.68 `cpc` above a $63.63 `bid_high`). Quote the bid range as what a top-of-page click costs, and treat any one figure as a range.
- `competition` (0 to 1) is how crowded the auction is, from Google; it is not keyword difficulty. In our data "crm software" showed 0.04 with a $37 CPC, so read it beside the bid range, never alone.
- `source: "google_ads"` on `seo_get_keyword_metrics` (24 credits flat, up to 1,000 keywords) returns Keyword Planner's own figures, the ones the user's Google Ads account will show.
- Keyword data caveats beyond ads (volumes, intent labels, trends) belong to [seo](../../seo/SKILL.md).
