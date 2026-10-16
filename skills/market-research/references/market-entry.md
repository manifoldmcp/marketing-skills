# Entering a new market

Whether a new country, language or segment is worth entering, and how: demand there in the words buyers use, who already wins it, how many companies fit, which search engines and assistants buyers use, and what entry needs that no tool can see. It ends in a scorecard and a go, wait or no-go call. Checked October 2026.

## Where buyers search, by market

Google is not the search engine everywhere, and Manifold's search data (every `seo_*` tool) is Google's. In the markets below, a Google-only reading covers a minority or a part of searches; say so on the scorecard.

| Market | Search, September 2026 (StatCounter, all devices, unless noted) | What it means |
|---|---|---|
| South Korea | Google 48%, Naver 43%. InternetTrend, a Korean panel, puts Naver at 63% for 2025 and 64% for the first half of 2026 | Naver favours its own Blog, Cafe and Knowledge content and runs its own AI Briefing summaries; ranking there means publishing on Naver |
| Russia | Yandex 70%, Google 28% | Yandex has its own ads and webmaster tools; Western ad platforms and payments are restricted |
| China | Baidu 47%, Bing 22%, Haosou 12%, Google 1.5% | Mainland hosting needs an ICP filing; Google, ChatGPT and most Western platforms are unavailable |
| Czech Republic | Google 79%, Seznam 15% | Seznam is smaller but still worth a check for consumer markets |
| Japan | Google 59% and Bing 31% on all devices, but Google 85% and Yahoo! 12% on mobile | Yahoo! Japan serves Google's results, so Google covers both; read StatCounter by platform, since its all-device figures can swing on desktop |
| Elsewhere | Google about 90% worldwide (StatCounter, 2026) | Google data is a fair read |

StatCounter counts page views on sites using its tracker, and panels like InternetTrend count differently, so the two can disagree by twenty points; quote the source and month with any share.

## Which AI assistants buyers can use

OpenAI does not serve mainland China, Hong Kong or Russia (its supported-countries list, checked October 2026). There, buyers use domestic assistants (ByteDance's Doubao, DeepSeek, Alibaba's Qwen and others in China; Yandex's Alice in Russia), which no Manifold engine reads. In Korea, Naver's AI Briefing answers inside Naver search. An `aeo_run_ai_answers` run with that `location` shows what ChatGPT, Claude, Gemini or Perplexity would say there, not what local buyers see. Elsewhere, run the buyer questions in the local language with the target `location` and `language`.

## Local language or English

- **Translation is not localisation.** Buyers often use the English term, a loanword or a word that is not the literal translation. In Germany "crm system" had 14,800 searches a month against 90 for "kundenverwaltung software", and a literal translation of "ai meeting notes" had no data at all ([field notes](field-notes.md)). Translate the seeds, have a native speaker check them, and price the home market's top English keywords in the target country too.
- **Pass English keywords with the country's own language code.** `seo_get_keyword_metrics` with `location: "Germany"` and `language: "de"` prices English keywords for Germany; `language: "en"` there was rejected (no charge). Supported pairs follow the provider's location list; an unsupported pair returns `InvalidTarget` free.
- **Consumers buy in their own language.** 76% of online shoppers prefer product information in their own language and 40% never buy from sites in other languages (CSA Research, "Can't Read, Won't Buy", 8,709 consumers in 29 countries, 2020). That is consumer data; CSA's B2B edition exists but its figures could not be checked here. Many B2B software buyers search in English, which is a reason to test English demand before paying for a translation, not proof that an English site will sell.
- **Watch the 2026 spike on English head terms.** English software terms in Germany jumped in March 2026 while the German terms did not ([field notes](field-notes.md)). Compare a local term with its English twin on the same months.

## The scorecard

Set the target beside the home market, from the same sources, and compare ratios, not raw numbers: a smaller country has less of everything.

| Metric | Tool | What it means |
|---|---|---|
| Category search demand, local language | `seo_search_keywords` with the target `location` and `language` | Buyers searching in their own words |
| English search demand there | `seo_get_keyword_metrics` on the home market's top keywords at the target | Whether the market buys in English |
| Demand per ICP company | Search demand over the company count | Comparable across countries of different size |
| ICP companies (B2B) | `leads_search_companies`, `rows_available`, target and home | The index's coverage differs by country: treat the ratio as rough |
| Who holds page one | `seo_get_serp` on the five biggest keywords | Local domains in the local language holding page one is the strongest sign of a hard market; home competitors ranking with English pages is the opposite |
| Local competitors' traffic there | `seo_get_domain_overview` with the target `location` | How big the local players already are, and what the user already gets there |
| What AI engines answer there | `aeo_run_ai_answers` in the local language | Whether the user or a home competitor is already named |
| Who pays to acquire there | `ads_search_ads` with the target `country` | An ad still running 90 days after `first_shown` there means paid acquisition works in that market |
| Local communities | `reddit_search_subreddits` in the local language | Whether the category is discussed locally (Reddit is thin in many non-English markets) |

About 90 credits per country. Mark any row a missing tool group could not fill "not measured".

**A segment instead of a country** keeps the home `location` and swaps the filters: the segment's `industries`, `employee_ranges` and `keywords` on `leads_search_companies`, segment words on the search keywords ("for enterprise", "for law firms"), and the segment's own competitors.

## What the tools cannot see

List these as open questions on every entry call: local payment methods and invoicing, tax registration and VAT, data residency and privacy law (GDPR and national rules), contracts and terms in the local language, support hours and language, regulated categories (health, finance), and for China the ICP filing and hosting. Any one can turn a go into a wait.

## The call

- **Go:** demand per company at or above home, page one not held by entrenched local players, and no open question that blocks. Then run the channel work with the target `location` and `language`.
- **Wait:** demand is there but an open question blocks, or demand rides on English head terms showing the 2026 step change.
- **No-go:** little demand in either language, or local players own page one, the AI answers and the ads.

One market at a time: two at once halve the effort each gets.

## The first 90 days after a go

| Period | Work | Measure |
|---|---|---|
| Days 1 to 30 | Open questions closed; local pages for the top keywords in the words buyers use; pricing in local currency | Pages indexed in the target; the blocking questions answered |
| Days 31 to 60 | One acquisition channel tested there (search, outbound or paid), chosen from the scorecard | Leads or sign-ups from the target against home per visitor |
| Days 61 to 90 | Re-run the scorecard rows that move (page one, AI answers, ads) and decide to scale or stop | Rankings and AI mentions there; cost per customer against home |

The channel plans themselves belong to [seo](../../seo/SKILL.md), [ai-search](../../ai-search/SKILL.md), [paid-ads](../../paid-ads/SKILL.md) and [outbound](../../outbound/SKILL.md); the launch sequence to [growth-strategy](../../growth-strategy/SKILL.md).
