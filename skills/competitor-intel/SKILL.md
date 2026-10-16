---
name: competitor-intel
description: When the user wants to know who their competitors are, what a rival is doing, or how to beat one. Holds what a model gets wrong about competitive research in 2026 (how far off traffic and keyword estimates are, the DataForSEO traffic model change on 1 November 2026, what ad libraries show, why AI answers now set the shortlist, what battlecard surveys can prove, the legal lines) and what each Manifold tool costs and hides. Also use when the user mentions real competitors, competitors we have never heard of, the competitive landscape, alternatives to X, a competitor teardown or profile, what is X doing in marketing, competitor alerts, a sales battlecard, landmine questions, competitor messaging or pricing side by side, what people hate about X, why customers leave X, competitive strategy, how do we beat X, or a 90-day plan against the incumbents. Positioning and objections go to voice-of-customer, ad creative to paid-ads, market maps to market-research, comparison pages to seo.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Competitor intelligence

Who the user really competes with, what each rival does and can afford, what buyers and AI engines say about them, and how to turn that into a plan, a battlecard or a weekly watch, as the evidence stood in October 2026. A model tends to get three things wrong: it reads Similarweb, Semrush, Ahrefs or DataForSEO traffic as a visit count, it takes the domains sharing a site's keywords for its competitors, and it fills in a rival's prices, features and headcount from memory. Every number about a competitor needs a source and a date, or it stays out.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the product, domain, category, ICP, known competitors with their domains and handles, differentiation, proof points and the objections sales hears from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. After delivering, offer to write the competitors, their claims and the counters back into that file.
- **Connector.** The knowledge here needs no tools. Measuring a rival needs the Manifold connector: look for `seo_get_domain_overview`, `ads_get_advertiser_ads`, `aeo_run_ai_answers` and `reddit_search_posts` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill and use the by-hand methods in the references (the public ad libraries, the rival's own pages, AI engines asked in a private window), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. If some tool groups are there and others are not, they are switched off on the app's Tools page: say which, and mark those rows "not checked" so nobody reads a gap as a zero. Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Traffic estimates are off by about half for a typical site, and far more for small ones.** Median error against Search Console was 49.5% for Ahrefs and 68.4% for Semrush (Ahrefs, 1,635 sites, May 2022, vendor-run); against Google Analytics no provider was consistently accurate and ±100% errors were common, though trends correlated well (SparkToro, 641 sites, November 2022). On five small sites of our own, DataForSEO estimated 0.2 to 1,168 visits a month against 0 to 9 real US clicks. Compare sites with one tool; never quote an estimate as visits ([reading the numbers](references/estimates.md)).
2. **The traffic model behind Manifold changes on 1 November 2026.** DataForSEO's new calculation (announced 4 September 2026) accounts for AI Overviews and other SERP features; historical data keeps the old model. Manifold asks for the new model already, but on 15 October it changed no figure, so the switch can still land by that date. Re-take every `organic_traffic` baseline after it; a jump across it is the model, not the competitor.
3. **Search competitors are mostly not business competitors.** Of the 24 domains sharing the most keywords with pipedrive.com, four sell a CRM; four of the seven brands that four or more AI engines named as Pipedrive alternatives were not among them (our test, October 2026). A company named by several independent sources is a competitor ([finding competitors](references/finding-competitors.md)).
4. **The shortlist is set before sales sees the deal, increasingly by AI answers.** Buyers had four of their five final vendors shortlisted on day one of a formal evaluation, and the top-ranked vendor won about 77% of deals (6sense, 10,000+ B2B buyers, July 2026, vendor survey). All five engines we asked named the same core of four CRMs, put HubSpot first, and cited a competitor's own "Pipedrive alternatives" page. Measure that shortlist with [ai-search](../ai-search/SKILL.md).
5. **Ad libraries show what a rival pays to keep running, never what performs.** Long-running ads are the winner proxy. The EU's Digital Services Act (Article 39) makes platforms keep every ad shown in the EU for a year after it last ran, with dates, targeting and reach; elsewhere Meta's library shows mostly what runs now, and LinkedIn list rows mostly carry no dates (22 of 24 in our test). Reading the libraries is [paid-ads](../paid-ads/SKILL.md).
6. **Company data lags and the two headcounts disagree.** The firmographic record trailed LinkedIn's count by 22% for a company growing 87% a year, carried an old copy of the company's description, and had nothing on a 1,100-person company (our test). Funding is never in it; revenue is a modelled range.
7. **No controlled evidence shows battlecards raise win rates.** The reports are vendor surveys of associations (Crayon, 2026, sample size unpublished: 49.6% of teams saw competitive win rates rise). 83% of CI teams draft battlecard content with AI and 76% have received AI output they could not vouch for (Klue, 250+ CI and PMM staff, 2026). Every line needs a source a rep can show ([battlecards](references/battlecards.md)).
8. **Sellers misjudge why they lose, and nearly every deal is competitive.** About 60% of sellers are partly or wholly wrong about the loss reason (Anova, around 2014, still cited by Klue); 1.5% of 3,400+ interviewed deals had no competitor, 3.5 on average (Klue, December 2024, vendor). "We lose on price" from a CRM dropdown is a hypothesis.
9. **Complaints are a skewed sample.** Ratings are J-shaped because the very unhappy and very happy post (Hu, Pavlou and Zhang, MIS Quarterly, 2017), and Reddit search is a ranked sample. Compare themes, never claim a rival is failing ([complaints](references/complaints.md)).
10. **The legal risk is in how intel is gathered and used, not in looking.** Public pages, ad libraries and public posts are fair game (Meta v. Bright Data, January 2024). Posing as a buyer or using someone's access is not: Appian's $2.036B trade-secret verdict against Pegasystems (2022) was overturned in 2024 and sent to a new trial in January 2026, years of litigation either way. Fake or bought reviews, AI-written included, carry FTC civil penalties since 21 October 2024 ([ethics and law](references/ethics.md)).

**Does not hold up:** "Similarweb (or Semrush, or Ahrefs) says they get 50,000 visits"; "the top SERP competitors are our competitors"; "no ads in the library means no paid spend" (agencies and resellers advertise under other names); "the ad with the highest impressions range is their best"; "battlecards raise win rates by X%"; "the competitor charges $49" because an AI answer said so (engines disagreed on prices in our run).

**Unknown:** no independent accuracy test of traffic estimates since 2022, and none of DataForSEO's at all; whether the November 2026 model is more accurate; whether battlecards cause higher win rates; how often an AI answer changes a buyer's shortlist rather than reflecting it.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| Who are our real competitors? Who are we compared with? Who have we never heard of? | [Finding competitors](references/finding-competitors.md) |
| How big is X, how much traffic does it get, how many people? Can we trust these numbers? | [Reading the numbers](references/estimates.md) |
| Tear down one competitor: search, ads, social, AI, company, in one table | [Teardown](references/teardown.md) |
| How do competitors pitch and price themselves, side by side? What claims does nobody make? | [Messaging and pricing](references/messaging.md) |
| What do people hate about X? Why do they leave it, and where do they go? | [Complaints](references/complaints.md) |
| A sales battlecard against X, landmine questions, counters | [Battlecards](references/battlecards.md) |
| Watch our competitors every week and tell us what changed | [Monitoring](references/monitoring.md) |
| How do we beat X? A competitive strategy or 90-day plan | [Strategy](references/strategy.md) |
| What is legal and ethical? Can we pose as a buyer, scrape, or name them in ads? | [Ethics and law](references/ethics.md) |
| What has Manifold seen in its own runs? | [Field notes](references/field-notes.md) |

## Using Manifold

- **Size and search:** `seo_get_domain_overview` (5 credits, cached 7 days; `history: true` adds 12 months for 56). Above about ten domains, `seo_get_traffic_estimates` (50 plus 50 per 100 domains) is cheaper. `seo_get_ranked_keywords` (10) splits branded from non-branded traffic; `seo_get_keyword_gap` (10) shows where a rival meets buyers the user never sees.
- **Who competes:** `seo_get_serp_competitors` (10) finds search competitors only; `aeo_run_ai_answers` (18 a prompt on five engines, never cached) finds the shortlist engines hand buyers; "<rival> vs" in `seo_search_keywords` (10) finds who buyers compare; Reddit search (1 a call) finds who people switch to.
- **Paid:** `ads_get_advertiser_ads` (1 a page, cached 24 hours). `active_only` works only on Facebook; elsewhere read `last_shown`. Google needs the domain or an id from `ads_search_advertisers`; `details: true` there costs 25 a page.
- **Company:** `leads_get_company` (1, cached 30 days) and `linkedin_get_company` (1). Report both headcounts; take the current bio from LinkedIn. Its `running_ads` is always null (a Facebook field): read the library with `ads_get_advertiser_ads`.
- **Pages:** `seo_get_page` (free) returns title, meta description and headings, never body text or prices. Prices come from the host's browser, the user's paste, or "not read".
- **Credits:** say the estimate first, pass `max_credits` when the user sets a budget, and price any call free with `dry_run: true`. Typical jobs: finding competitors about 145, a teardown 78, a battlecard 32, a messaging table 56 for four rivals, complaints 53 for three, a plan 149, a weekly watch 41 a run for three rivals.
- **Never contact anyone, post, or pose as a buyer.** The server reads public data and keeps no state; the host stores baselines.

## Related skills

- AI answers about the user and its rivals, measured properly: [ai-search](../ai-search/SKILL.md). A rival's ads read creative by creative, and bidding on the user's brand: [paid-ads](../paid-ads/SKILL.md).
- Turning the gaps into positioning, and the user's own buyers' objections: [voice-of-customer](../voice-of-customer/SKILL.md). The whole market mapped and sized: [market-research](../market-research/SKILL.md).
- "X vs Y" and "X alternatives" pages and keyword gaps: [seo](../seo/SKILL.md). Answering switching threads: [reddit](../reddit/SKILL.md).
- Mentions of the user's own brand: [brand-monitoring](../brand-monitoring/SKILL.md). A growth plan with no rival in focus: [growth-strategy](../growth-strategy/SKILL.md). The same research for an agency's prospect: [agency](../agency/SKILL.md).
- Competitors on Amazon (listings, prices, the Buy Box): [amazon](../amazon/SKILL.md). In the app stores: [app-store](../app-store/SKILL.md). On Google Maps, for a local business: [local-seo](../local-seo/SKILL.md). Who undercuts a product's price across stores: [google-shopping](../google-shopping/SKILL.md).
