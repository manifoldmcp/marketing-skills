# Google search ads

A keyword list for search ads: the terms worth paying for, what a click costs, how they group, the negatives that keep the wrong clicks out, and what competitors' search ads say. It ends in tables the user loads into Google Ads; nothing here bids or builds a campaign. How Google's automation changed in 2025 and 2026 is in [platforms](platforms.md#google-fewer-keywords-more-automation-more-controls).

## Building the list

- **Seeds:** three to five: the category ("payroll software"), the problem ("run payroll"), the audience ("payroll for restaurants"), competitor names if the user will bid on them. For a local business the city goes in the keyword and location targeting does the rest.
- `seo_search_keywords` per seed (10 credits for 100 rows). The default `mode` keeps the seed in every keyword; `mode: "related"` adds the words buyers use without it. It already returns `volume`, `cpc`, `bid_low`, `bid_high`, `competition`, `intent` and `trend`; do not call metrics after it, except to confirm with Keyword Planner below.
- **Keep** `intent` transactional and commercial, and informational terms only with a buying modifier (price, pricing, software, tool, service, near me, best, vs, alternative, for a named audience). High-volume informational terms rarely pay back at a small budget; they belong to [seo](../../seo/SKILL.md).
- **Group** into ad groups of five to twenty close terms: brand, competitor, category, audience, problem. One ad group is one message and one landing page.
- **Confirm** the shortlist with `seo_get_keyword_metrics` and `source: "google_ads"` (24 credits flat): Keyword Planner's own volume and bids, the figures the user's account will show. `kd` and `intent` are null in that source, and keywords over 80 characters come back empty.

## Fitting it to the budget

- Monthly spend divided by the average `cpc` is the clicks the account can buy; times the expected conversion rate (ask; WordStream's 2026 median across industries is 8.18%, but B2B software runs far lower) is the conversions.
- Smart Bidding steers on conversions per campaign. Under about 30 a month (a rule of thumb), plan one campaign of two to four ad groups on exact and phrase match with capped bids, and cut to the ad groups the budget can feed, highest intent first. Broad match and AI Max wait until conversion tracking clears that bar: they spend on expansion before the account knows what converts.
- Terms under about 20 searches a month rarely serve (Google marks them low search volume); keep them only inside an ad group with bigger terms.
- `trend` shows seasonality: put spend into the months volume peaks.

## Match types and automation in 2026

- Exact and phrase still give the most control, but neither is literal any more; both match close variants and intent. Read the search terms report weekly in the first month.
- AI Max adds broad-like expansion on top of the keyword list. In Smarter Ecommerce's 250-campaign sample, 80% of AI Max impressions came from campaigns' exact keywords and incremental conversions cost about 16% more than the core. Turn it on as a separate test with its own CPA target, after the core converts.
- Performance Max competes for the same queries when it runs beside Search. Add the brand and the Search campaign's core terms as negatives in PMax where the user wants Search to own them (campaign-level negatives, up to 10,000, since 2025).

## Negatives

From the candidate rows the user would never pay for: job and salary terms, free, course, template, login, what is, how to (unless the user sells the answer), cheap (for a premium product), other meanings of the seed, and competitor names if the user will not bid on them. Take each from the actual list, not from memory, so every negative blocks something real. Apply the same list to Search and, as a shared list, to Performance Max.

## Who bids, and what they say

- **Competitors' search ads:** `ads_get_advertiser_ads` with `platform: "google"` and `country`, reading text ads with `ads_get_ad`, as [ad libraries](ad-libraries.md#google) sets out. The library never says which keyword an ad bids on: match headlines to ad groups by their words. `seo_get_page` on the landing pages (free) shows where each competitor sends search clicks.
- **Who bids on a term:** do not rely on `seo_get_serp` for this. Its rows can include `type: "paid"`, but in six fetches in October 2026, including "car insurance quotes" (a $157 CPC term that always carries ads), none came back with a paid row ([field notes](field-notes.md)). An empty result is not an empty auction. The reliable source is the user's own Auction Insights report in Google Ads, which names the domains competing on their keywords.
- **Competitor terms** cost more and convert worse than category terms. Bidding on a rival's name is allowed; using its trademark in the ad text can be restricted ([brand bidding](brand-bidding.md)).

## Deliver

Three tables. Keywords: ad group, keyword, suggested match type, Keyword Planner volume, `cpc`, bid range, `competition`, intent, trend, landing page. Negatives: term, match type, reason. Competitor search ads: advertiser, headline, description, landing page, first shown, days running. Add the monthly clicks the budget buys at the list's average `cpc`, as a range.

A default run costs about 97 credits: three seeds (30), Keyword Planner on the shortlist (24), and three competitors' Google ads with ten read each (about 33), plus ten SERPs (10) if the user wants the organic picture beside it.
