# Field notes

What Manifold has seen in its own competitive runs, United States, English. Small samples: read them as evidence of how the data behaves, not as benchmarks. Newest first. All runs on 15 October 2026, 59 credits in total.

## The traffic model flag (1 site, DataForSEO directly)

`domain_rank_overview`, `bulk_traffic_estimation` and `ranked_keywords` on buffer.com, United States, each with `use_new_etv` true, false and absent: all three returned the same `etv` to the last digit (756,486 visits for the domain; 27,512 for "buffer"). The new model was either already on for the account or not yet live; the tools now send true, so the figures move at most once.

## Traffic estimates against our own Search Console (5 sites, 25 credits)

`seo_get_domain_overview` (location United States) on five small sites we own, against their real US Google clicks in Search Console for 12 September to 11 October 2026 (`console_get_search_analytics`, grouped by country).

| Site | Estimated `organic_traffic` a month | Real US Google clicks, 30 days | Note |
|---|---|---|---|
| site B | 129 | 9 | 113 of the estimate sits on one article that was not among the ten US pages with impressions |
| site A | 1,168 | 0 to 1 (not in the top five countries; no US page had a click) | Real US average positions 31 to 75; `domain_rank` 0 despite 162 ranked keywords |
| site D | 8 | 5 | Closest of the five |
| site C | 0.2 | 4 | Underestimate |
| site E | null | 0 | No data |

- Errors ran an order of magnitude in both directions, as the published studies predict for small sites. The provider's ranking snapshot can be stale: `keyword_changes` showed 97 keywords lost for site A, yet it still credited 1,168 visits.
- For a small competitor, the estimate cannot tell 10 visits from 1,000. Use it to say "small", never to give a number.

## Search competitors against the AI shortlist (pipedrive.com, 25 credits)

`seo_get_serp_competitors` on pipedrive.com, `limit: 25` (24 rows, 7 credits), and `aeo_run_ai_answers` with "best Pipedrive alternatives" on the five default engines (18 credits), `brands` holding ten CRMs.

- **Search overlap is mostly platforms.** The top 24 by `shared_keywords` were YouTube, Reddit, LinkedIn, Indeed, HubSpot, Quora, Facebook, Salesforce, Medium, Zendesk, Wikipedia, Pinterest, Instagram, Mailchimp, Shopify, monday.com, Scribd, Forbes, Investopedia, Study.com, Google, Adobe, Microsoft and Coursera. Four sell a CRM (HubSpot, Salesforce, monday.com, Zendesk's sales product).
- **The AI shortlist is different and stable.** Named by all five engines: HubSpot (first on every engine), Zoho, Close, monday.com. By four: Freshsales, Salesforce, Attio. By three: Copper. By none: Folk and Zendesk. Claude alone added NetHunt, Streak, Salesflare, Vtiger, Nutshell and EngageBay. Of the seven brands named by four or more engines, Zoho, Close, Freshsales and Attio were not among the 24 search competitors; Zendesk was among them and named by no engine.
- **Competitors write the lists engines read.** Zapier's Pipedrive alternatives listicle was cited by all five engines. Attio's own "Pipedrive alternatives" page was cited by Perplexity and the AI Overview; Perplexity cited six monday.com pages. Only the AI Overview cited a Reddit thread and YouTube videos.
- **Engines state prices, and disagree.** Close was "from $49" on Gemini and Perplexity while Claude said its automation starts on a $99 plan; monday CRM was "from $9" on Gemini and the AI Overview and "from $12" on Perplexity. Prices in AI answers are not evidence.
- `named_brands[]` came back only for ChatGPT, as the tool documents; on the other engines read the `answer`.

## Company records (4 companies, 8 credits)

`leads_get_company` and `linkedin_get_company` on four CRMs.

| Company | Record `employees` (band) | LinkedIn `employees` | Note |
|---|---|---|---|
| Pipedrive | NoData | 1,115 | No record for a 1,100-person company |
| Close | 206 ("51-200") | 216 | The count and the band contradict each other |
| Attio | 161 ("51-200") | 206 | `headcount_growth` +87% over 12 months; the record lags a fast grower |
| folk | 62 ("11-50") | 66 | The record's description says 3,000 customers, LinkedIn's bio 5,000 |

- Descriptions in the company record are older copies of the LinkedIn text (Attio "AI-native CRM" there, "agentic CRM" on LinkedIn now). Take positioning from `linkedin_get_company` `bio`.
- `running_ads` and `ad_library_id` were null on all four LinkedIn records, including Pipedrive, which had 1,692 ads in LinkedIn's library the same day. The company page says nothing about ads; both fields are Facebook's, and the tool description now says so.

## LinkedIn ad library (Pipedrive, 1 credit)

`ads_get_advertiser_ads` on linkedin for "Pipedrive": 24 rows on the first page of 1,692 available. 22 of 24 rows had null `first_shown` and `last_shown` and null `destination_url`; only two document ads carried dates (13 to 15 October 2026), an impressions range and targeting. 20 of 24 pushed one product launch. On LinkedIn, the 30-day `last_shown` filter drops most rows; compare ids between runs instead, and use `ads_get_ad` for the dates of the few that matter.
