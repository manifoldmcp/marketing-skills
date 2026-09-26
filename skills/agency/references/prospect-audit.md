# Prospect audit

A prospect audit opens a sales conversation. It is fast and cheap on purpose: a handful of calls that find three findings the prospect can check for themselves, each with a competitor beside the number. The full audit is paid work after they sign; this playbook points to it rather than doing it.

## Inputs to settle first

- **Prospect**: the domain, and what the business sells and where (a local dentist, a national online store, a SaaS).
- **Service**: what the agency sells (SEO, AI search, paid, all of it). It decides which finding leads.
- **Competitors**: one or two. Default: the top two real businesses, not publishers or marketplaces, from `seo_get_serp_competitors` with `limit: 10` (6 credits).
- **Buyer questions**: two questions a buyer would ask an AI engine ("best invoicing app for freelancers", "emergency plumber in Leeds"). Default: built from the prospect's top keywords in step 2.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 6 + 3 x 5 + 10 + 10 + 3 + 2 x 18 + 4 = 84 credits, under a dollar. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Size the prospect against the competitors.** `seo_get_domain_overview` on the prospect and on each competitor (5 credits each). Read `domain_rank`, `organic_traffic`, `organic_keywords`, `positions` and `top_pages`. The traffic gap to the nearest competitor is usually the headline.
2. **Find the quick wins.** `seo_get_ranked_keywords` on the prospect (10 credits). Keep the keywords ranking 4 to 20 with volume: they sit close to the traffic, and a page-level fix moves them. Then `seo_get_page` on the homepage and on the top page from step 1 (free): a missing or overlong title, no meta description, no `h1`, or empty `schema_types` is a finding anyone can see. Name three. The method for the full list is [quick wins](../../seo/references/quick-wins.md).
3. **Show the gap.** `seo_get_keyword_gap` of the prospect against the strongest competitor (10 credits). Sum the `volume` of the top 20 rows: "competitor.com ranks for these 20 searches, worth N a month, and you rank for none of them."
4. **Check site health.** `seo_run_technical_crawl` with `max_pages: 100` (3 credits), then `get_task` (free) after `poll_after_s`. Read `broken_links`, `non_indexable`, `duplicate_titles` and `onpage_score`. Meanwhile, `aeo_get_site_readiness` on the domain (free): a robots.txt or firewall rule that blocks the AI crawlers is a finding few prospects know about. The full technical audit is [audit](../../seo/references/audit.md).
5. **Check AI visibility.** `aeo_run_ai_answers` with the two buyer questions and `brands` set to the prospect and the competitors, on the default five engines (18 credits a prompt), then `get_task`. Count in how many of the ten answers each brand is mentioned and in how many it is cited (`mentions[]`). The full check, with prompt research, is [visibility check](../../ai-search/references/visibility-check.md).
6. **Check ads.** `ads_get_advertiser_ads` with `platform: "facebook"` and the prospect's page name, and with `platform: "google"` and its domain, for the prospect and the strongest competitor (1 credit each). If Google returns nothing for the domain, `ads_search_advertisers` with the brand name (1 credit) gives the advertiser id to use instead. Note whether each runs ads, how many are `active`, and the oldest `first_shown` still running: an ad that has run for months pays for itself. A full read of a competitor's ads is [competitor ads](../../paid-ads/references/competitor-ads.md), and of the competitor as a whole [teardown](../../competitors/references/teardown.md).
7. **Deliver** two tables and three lines. A scoreboard: domain, domain rank, estimated organic traffic, organic keywords, keywords in the top 3, AI mentions out of ten, active ads. A findings table: area (search, quick wins, site health, AI visibility, ads), finding, the number, the evidence (a URL, a keyword or a prompt), and the playbook that fixes it. Then the three findings to open the conversation with, one sentence each, with a number and a competitor. The host or the user sends it; nothing is sent from here.

## Judgment

- Three findings beat thirty. The audit's job is a meeting; the full audit and the plan are what the prospect pays for.
- Lead with what the prospect can verify in a minute: ask ChatGPT the buyer question, search the keyword, open the page. Give the prompt and the keyword so they can.
- Traffic is an estimate from a search index, not the prospect's analytics. Say "estimated", and never quote it as their number; they know the real one. The comparison with competitors is fair because every domain comes from the same source.
- A small local business often shows null or tiny traffic estimates. Use `seo_get_position` (6 credits each) on two or three "<service> in <city>" keywords instead, and show who ranks above them from `above[]`.
- One AI run is a snapshot, and answers change between runs. Give the date, and say that a second run can differ.
- A finding outside what the agency sells (no ads, when the agency does not run ads) goes to the bottom or out.
- Be fair. A prospect that already does well hears so, with the one gap that remains. An audit that invents problems ends the conversation.
- The person to send the audit to is a [leads](../../leads/SKILL.md) job: its [account brief](../../leads/references/account-brief.md).
