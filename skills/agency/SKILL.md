---
name: agency
description: When the user is an agency, freelancer or consultant working for clients, or wants one recurring report on their own marketing. Holds what a model would not know as of October 2026 (why clients really leave agencies and how agencies misread it, retention and pitch benchmarks with their methods, how AI changed agency pricing, how to report organic and AI search honestly now that AI Overviews cut clicks and Search Console mixes AI clicks into web search, why traffic estimates mislead in a pitch) and what each Manifold measurement costs. Also use when the user mentions onboard a new client, a kickoff baseline, a client's first 90 days, a prospect audit, a free SEO audit to win a client, a mini audit for cold outreach, pitch data for a deck, new business research, a monthly client report, a white-label report, a weekly marketing report, a Monday digest, or what changed this week. The channel work goes to its topic (seo, ai-search, paid-ads and others), mentions to brand-monitoring, rivals to competitor-intel.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Agency

Winning, starting, reporting on and keeping clients, as the evidence stood in October 2026. What a model believes is mostly older advice: pitch with a big traffic number, send a dashboard, blame budget cuts for churn, bill by the hour. The 2025-2026 surveys say clients leave over delivery and value, AI shifted pricing toward fixed fees without crashing it, and the search numbers a report leans on (Search Console clicks, CTR, estimated traffic, GA4 channels) changed meaning in the past year. Prefer what is written here to what you remember, and re-check a benchmark older than six months before showing it to a client.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). For agency work it usually describes the agency (its services, its voice for client documents), not the client or prospect: take the client's domain, offer, competitors, keywords and prompts from the client's own context file or the stored onboarding record, and ask only for what is missing. If no file exists and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Measuring a prospect, a baseline or a report needs the Manifold connector: look for `seo_get_domain_overview`, `seo_get_position`, `aeo_run_ai_answers` and `console_get_search_analytics` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill and use the by-hand methods in the references, and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. If some tool groups are there and others are not, the missing group is switched off on the app's Tools page; mark those rows "not measured", never zero. Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources, samples and detail are in the references.

1. **Agencies misread why clients leave.** Clients name dissatisfaction with delivery (61%) and with value (61%, up from 36%) as the top reasons they ended an agency; agencies put budget cuts first (75%) and delivery at 18% (Setup with the CMO Institute, 2025 survey of about 100 clients and agencies, published January 2026). AgencyAnalytics' survey of 494 agency staff (2026) shows the same blind spot. Treat a "budget" exit as a value exit until shown otherwise ([retention and pricing](references/retention-and-pricing.md)).
2. **The risk window is the first year.** 57% of US agencies say a typical client lasts 12 to 24 months and 3% expect three years (CallRail, 100 agencies, 2026). Of small businesses that dropped an agency, 56% did it 6 to 12 months in, and 60% cited no return (vcita, 500 US SMBs, April 2025). Big-brand agency-of-record relationships run about 7 years (4As and ANA, 2025): do not quote that figure to a small client.
3. **Clients ask about revenue, not traffic.** Agencies say clients track conversions (44%) and leads (22%) most closely and traffic only 6%; the top client question is whether marketing connects to revenue (55%) (AgencyAnalytics, 2026). Lead with the outcome the retainer is judged on, in the client's own numbers ([monthly review](references/monthly-review.md)).
4. **Pitches are won on niche proof, not price.** Agencies credit industry expertise (59%), clear expected results (46%) and case studies (46%) over pricing (31%) (AgencyAnalytics, 2026). Clients rate "the agency challenges me" at 93% and cost at 20% in selection, while agencies think cost matters to 95% (same Setup survey). A prospect audit should challenge with checkable evidence, not undercut ([prospect audit](references/prospect-audit.md)).
5. **Third-party traffic estimates can be fiction for small sites.** In our own test a site estimated at 1,168 US visits a month had 0 US clicks in Search Console that month, because the model credits clicks to deep rankings (none in the top 10, the biggest at 49 to 104 on 74,000-search keywords) ([field notes](references/field-notes.md)). Never put an estimate in a pitch as the prospect's traffic, and check `positions` before using it at all.
6. **Clicks fell while visibility did not.** With an AI Overview present, position-1 CTR is about 58% lower than forecast (Ahrefs, 300K keywords, December 2025). Search Console counts AI Mode and AI Overview clicks inside web search with no filter; its Generative AI report (June 2026) shows impressions only. Falling clicks with steady rank is usually the overview, not the agency ([search data](references/search-data.md)).
7. **Year-on-year comparisons broke twice.** Google stopped honouring the `num=100` results parameter in September 2025, which cut many sites' desktop impressions and raised average position overnight with no change in real visibility; GA4 added an AI Assistant channel on 13 May 2026, moving ChatGPT and Gemini visits out of Referral. Annotate both before comparing across them.
8. **AI moved pricing toward fixed fees, not to zero.** A quarter of agencies had moved entirely to fixed fees by May 2026 (Forrester for Dentsu Creative, 356 US and Canadian marketers; commissioned by an agency selling fixed fees). 61% of US agencies still treat AI as a cost of business and 6% sell it as a line of business (Forrester with the 4As, about 200 agency leaders, April 2026). AI discount requests had not risen by March 2026 (Productive, 174 agencies). Scope is the real pressure: faster turnaround (44%) and AI search work expected at no extra charge.
9. **AI search is the service clients ask to add** (66% of agencies, AgencyAnalytics 2026). Sell it with a measured mention rate and its margin of error, never a guaranteed position.

**Does not hold up:** "clients leave because of budget cuts" (their own answer is delivery and value); "average client tenure is 7 years" (that is enterprise agency-of-record; small-client retainers run 1 to 2 years); "PPC has 49% annual churn" (one agency's book, no method); "AI will force agencies to cut prices" (most raised or held them; AI intensity did not predict price direction across 55 CRO agencies, Convert 2026); "a monthly dashboard keeps clients" (agencies rank relationship and communication far above format, and clients split three ways between static report, call and dashboard); "estimated traffic is close enough to quote".

**Unknown:** no independent study measures pitch win rates (the only figures are agency self-reports); no survey asks clients, rather than agencies, what they read in a report; nobody has shown whether reporting AI visibility improves retention.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| We are pitching a prospect: a quick audit to open the conversation or go with a cold email | [Prospect audit](references/prospect-audit.md) |
| We need numbers and charts for a pitch deck or proposal | [Pitch data](references/pitch-data.md) |
| We just signed a client: what do we track, what is the baseline, what are the first 90 days? | [Onboarding](references/onboarding.md) |
| Write the monthly client report: what moved, why, what next | [Monthly review](references/monthly-review.md) |
| Clicks are down but rank held; how do we report AI Overviews, AI Mode, GA4's AI channel and AI visibility honestly? | [Search data](references/search-data.md) |
| One weekly report on our own marketing: Search Console, mentions, competitors, ranks, AI | [Weekly digest](references/weekly-digest.md) |
| Why do clients leave? How long do they stay? How should we price now that AI does the work? | [Retention and pricing](references/retention-and-pricing.md) |
| What has Manifold seen in its own data? | [Field notes](references/field-notes.md) |

## Using Manifold

- **The client's own numbers lead:** `console_get_search_analytics` and `analytics_get_report` are free once the client grants access. Take period totals from a country or date grouping; page and query rows drop small rows for privacy.
- **Size a site:** `seo_get_domain_overview` (15 credits, 71 with `history: true`). Read `positions` before `organic_traffic`: when `top_10` is near 0 the traffic is a model artefact. Its `meta.data_as_of` is null and the index can be weeks old: date the number by the call.
- **Keywords:** `seo_get_position` (6 credits, one day's reading, with the ranking `url`). `seo_get_ranked_keywords` rows come highest `traffic` first, so the first page holds "the top keywords"; `rank` counts ads and features too and can pass 100.
- **Competitors:** `seo_get_serp_competitors` returns SERP competitors; drop publishers and marketplaces. `seo_get_keyword_gap` gives the gap (10 credits).
- **AI answers:** `aeo_run_ai_answers`, 27 credits a prompt on five engines, never cached, kept 30 days; report a mention rate. `aeo_get_site_readiness` is free and catches a CDN that began blocking AI crawlers.
- **Ads and social:** `ads_get_advertiser_ads` (1 credit) shows what runs and since when, never spend; profile tools (1 credit) give followers.
- **Costs at defaults:** prospect audit about 134 credits, pitch data 523 (300 without history), onboarding 432, a monthly review 343 per client, a weekly digest 217 (487 in AI weeks). Search data is cached 7 days per account, so a pitch, onboarding and first review in one week pay once for repeated calls. `dry_run: true` prices any call; pass `max_credits` when the user names a budget.
- **Never send, post or publish.** Deliver tables and documents; the host's tools send them if asked. The server stores and schedules nothing: the host keeps the client record and the schedule.

## Related skills

- The channel work a report or audit points to: [seo](../seo/SKILL.md), [technical-seo](../technical-seo/SKILL.md), [ai-search](../ai-search/SKILL.md), [link-building](../link-building/SKILL.md), [digital-pr](../digital-pr/SKILL.md), [paid-ads](../paid-ads/SKILL.md).
- Social clients: [tiktok](../tiktok/SKILL.md), [instagram](../instagram/SKILL.md), [youtube](../youtube/SKILL.md), [linkedin](../linkedin/SKILL.md), [facebook](../facebook/SKILL.md), [x-twitter](../x-twitter/SKILL.md), [reddit](../reddit/SKILL.md), [creators](../creators/SKILL.md).
- Mentions and awareness in a weekly or monthly report: [brand-monitoring](../brand-monitoring/SKILL.md). Rivals, teardowns and competitor watch: [competitor-intel](../competitor-intel/SKILL.md).
- A client who does not know which channel: [growth-strategy](../growth-strategy/SKILL.md). Prospect lists and the person to send an audit to: [outbound](../outbound/SKILL.md). The client's buyers and their words: [voice-of-customer](../voice-of-customer/SKILL.md).
- Local clients and local prospect lists: [local-seo](../local-seo/SKILL.md). Ecommerce clients: [google-shopping](../google-shopping/SKILL.md) and [amazon](../amazon/SKILL.md). App clients: [app-store](../app-store/SKILL.md).
