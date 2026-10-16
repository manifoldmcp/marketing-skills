# Reading the numbers

How far to trust what the tools say about a competitor's traffic, keywords, size and spend. Every competitive deliverable rests on these numbers, and most of them are modelled. Checked October 2026.

## Traffic estimates

Third-party traffic is a model: for search tools, estimated search volume times an estimated click-through rate at an estimated position, summed over the keywords the tool has seen the site rank for. Each layer adds error.

- **Typical error is about half.** Ahrefs compared 1,635 random sites' US organic estimates with their Search Console clicks: median deviation 49.5% for Ahrefs, 68.4% for Semrush, with some sites within 5% and others off by more than 1,000% (Ahrefs, May 2022; vendor-run, so its own lead is not neutral). SparkToro compared four providers with Google Analytics on 641 sites over 12 months and concluded no third-party estimate was consistently accurate; errors of ±100% or more were common (November 2022). OWOX found Semrush and Similarweb 45% to 61% off on 787 sites over 100,000 sessions a month (January 2020 data).
- **Small sites are worst.** Screaming Frog's 25-site test (2016) found nearly every estimate at least 40% off for sites under 10,000 monthly organic visits; SparkToro found Similarweb weakest under 5,000 users and Ahrefs overestimating there. In our own five small sites the error was an order of magnitude in both directions ([field notes](field-notes.md)).
- **Trends beat levels.** Correlation with real traffic was 0.79 for Semrush, 0.72 Datos, 0.66 Similarweb (SparkToro, total traffic) and about 0.75 for Ahrefs against Search Console (organic only). The estimates rank sites in roughly the right order and move in roughly the right direction; they do not count visits.
- **What to do with it:** compare sites with one tool and one location, never mix tools; report "estimated organic traffic" with the tool name, never "visits"; calibrate where possible by comparing the user's own Search Console clicks with the tool's estimate for the user's site, and say the ratio is indicative only for sites of similar size and type (Ahrefs suggests this calibration; it is not validated).
- **Nothing independent since 2022.** No published test covers DataForSEO, the provider behind Manifold's `seo_*` tools, or any tool after AI Overviews changed click-through rates. Say so when a decision hangs on a traffic number.

## The DataForSEO change on 1 November 2026

DataForSEO announced on 4 September 2026 a new estimated-traffic model: a click-through curve that accounts for AI Overviews, featured snippets and ads, varies by search intent, and normalises volume with clickstream data. It becomes the default for every account on 1 November 2026; historical data keeps the old model, and DataForSEO published no accuracy figures for either. Manifold asks for the new model on every call, yet on 15 October 2026 asking for it, or for the old one, changed no figure for buffer.com ([field notes](field-notes.md#the-traffic-model-flag-1-site-dataforseo-directly)), so the switch may not have reached the account yet. So:

- `organic_traffic`, `paid_traffic` and `organic_traffic_cost` from `seo_get_domain_overview`, `seo_get_traffic_estimates` and `seo_get_serp_competitors`, and `traffic` per keyword from `seo_get_ranked_keywords`, can shift once, by 1 November at the latest, for every domain at once.
- A competitor's traffic "moving" across 1 November is the model. Re-take every stored baseline after it, and compare only figures taken on the same side of the date.
- `history[]` from `history: true` likely stays on the old model while the current month uses the new one; do not read the last step of that series as a trend in November or December 2026.

## Keywords and positions

- `organic_keywords` counts what the provider's crawl has seen ranking in the top 100, not everything a site ranks for; long-tail and local pages are undercounted, which is one reason estimates miss sites built on them.
- Separate branded from non-branded rows (`seo_get_ranked_keywords`, by whether the keyword holds the brand) before comparing. A known brand gets most of its traffic from its own name, which the user cannot contest.
- `keyword_changes` (new, up, down, lost) compares with the provider's previous update, not with the user's last run.
- Rankings in the provider's data can be stale: on one of our sites the provider credited 1,168 visits a month to pages whose real average position was 60 to 75.

## Company size

- `leads_get_company` and `linkedin_get_company` give two headcounts that disagree. The record lags fast growers (161 against LinkedIn's 206 for a company growing 87% a year), can mix a count with a band that contradicts it (206 employees, band "51-200"), and returns NoData even for large companies. Report both, with the source.
- `headcount_growth[]` is the best momentum signal the record carries, when it is filled.
- `revenue_min` and `revenue_max` are an estimated range; `revenue` is filled only where disclosed. `total_funding` and `funding_stage` are always empty: ask the user, or say funding was not checked.
- The record's `description` can be an old copy of the company's own text; take current positioning from the LinkedIn `bio` or the homepage.

## Ad spend

The libraries publish no spend outside political and some EU ads, and `impressions` and `spend` are text ranges where published. Count ads and read how long they run; never turn ranges into a budget. No ads found does not prove no spend: search ads can run under an agency's or reseller's advertiser name, and affiliates promote without ads. Say "none found". Reading the libraries in full is [paid-ads](../../paid-ads/SKILL.md).

## What the tools cannot see

Pricing (no tool reads page body text), product quality, the sales team, email marketing, funding, and the user's own win rates. Never fill any of these from memory or from an AI answer; write "not read" and say where to get it.
