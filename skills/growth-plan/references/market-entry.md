# Market entry

Whether a new country or segment is worth entering, and how: the demand there in the local language, who already wins it, how many companies fit the ICP, and what AI engines answer there. The search and answer tools take `location` and `language`, which is what makes this work; the rest is setting the target beside the home market.

## Inputs to settle first

- **Home market**: where the user sells now. Default: United States, English.
- **Target**: a country with its language (for example `location: "Germany"` and `language: "de"`), or a segment (enterprise, a vertical, a company size).
- **Keywords**: the category in the target language. Translate, then have a native speaker check: buyers often use a different word from the literal translation, or the English one.
- **Competitors**: the user's home competitors, and any local ones the user knows.
- **ICP filters**: industry, headcount and keywords, for the company counts.
- **Budget**: about 20 + 6 + 5 + 15 + 36 + 8 + 2 = 92 credits for one country. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Demand there.** `seo_search_keywords` with the translated seed and the target `location` and `language` (10 credits each; two seeds). Then `seo_get_keyword_metrics` with the home market's top 20 keywords, in English, at the target `location` (about 6 credits): in many B2B markets buyers search in English. Set each beside the home market's volume.
2. **Who wins there.** `seo_get_serp` for the five biggest target keywords with the target `location` and `language` (1 credit each). Mark each result: the user, a home competitor, a local competitor (a local domain, the local language), or a publisher. Then `seo_get_domain_overview` with the target `location` on the user's site and on the two strongest local competitors (5 credits each): the traffic the user already gets there, and what the local players get.
3. **What AI engines answer there.** `aeo_run_ai_answers` with two buyer questions in the target language, the target `location` and `language`, and `brands` set to the user and the competitors (18 credits a prompt), then `get_task`.
4. **Companies there (B2B).** `leads_search_companies` with the ICP filters and `locations` set to the target country, then again with the home country (4 credits each). Compare `meta.rows_available`: the ratio is the size of the new market against the one the user already sells into.
5. **Who pays to win customers there.** `ads_search_ads` with `platform: "facebook"`, the category in the target language and `country` set to the target's code (1 credit): who advertises there, since when, and in which language. `reddit_search_subreddits` with the category in the target language (1 credit): whether local communities discuss it.
6. **A segment instead of a country.** Keep the home `location` and swap the filters: `industries` and `employee_ranges` on `leads_search_companies`, segment words on the keywords ("for enterprise", "for law firms"), and the segment's own competitors. The steps are the same.
7. **Deliver** a scorecard: metric (category search demand in the local language, English search demand, ICP companies, AI mentions of the user, local competitors on page one, advertisers), home value, target value, ratio, and what it means. Add the local competitors (domain, estimated traffic there, the keywords where they rank, ads running) and a go, wait or no-go call with the reasons. For a go, name the channel playbooks to run with the target `location` and `language`, for example the [SEO strategy](../../seo/references/strategy.md), the [AI search strategy](../../ai-search/references/strategy.md) and, for B2B, the [lead list](../../leads/references/lead-list.md).

## Judgment

- A smaller country has less of everything. Compare ratios (demand per ICP company, the share of page one held by local players), not raw volume.
- Local competitors on local domains holding page one is the strongest sign of a hard market: buyers there prefer local vendors, or the language is a moat. Home competitors ranking there with English pages is the opposite sign.
- Translation is not localization. If the local-language keywords show no volume and the English ones do, the market may buy in English; check that before paying for a translation.
- Company counts come from one provider, and its coverage differs by country. Compare the target with the home market from the same source and treat the ratio as rough.
- The tools cannot see what else entry needs: local payment methods, invoicing and tax, data residency, support hours, the law. List them as open questions for the user.
- One market at a time. Two countries at once halve the effort each one gets.
