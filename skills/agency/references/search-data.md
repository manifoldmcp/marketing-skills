# Search data for clients

How to put organic and AI search in front of a client in 2026, when clicks no longer track visibility, Search Console blends AI clicks into web search, and two changes broke year-on-year comparisons. The deeper search mechanics live in [seo](../../seo/SKILL.md) and [ai-search](../../ai-search/SKILL.md); this page is what the client sees. Checked October 2026.

## Clicks fell while visibility did not

- **Position-1 CTR is about 58% lower than forecast when an AI Overview shows** (Ahrefs, 300,000 keywords, Search Console CTR, December 2023 against December 2025). Its April 2025 study found 34.5% fewer clicks; the two measure different things, but the direction is the same.
- **Being cited in the overview softens it, not cures it.** On 3,119 informational queries across 42 Seer clients, Q3 2025 organic CTR was 0.70% where the client was cited in the overview, 0.52% where it was not, and 1.45% with no overview (Seer Interactive, November 2025; Seer notes the cited brands may simply be stronger, and some of its own figures are inconsistent). CTR fell year on year even on queries with no overview.
- **So say it plainly.** When clicks fall and rank holds on the tracked keywords, check whether an overview now sits above the client (`seo_get_serp` with `ai_overview: true`, 2 credits a keyword) and whether it cites them. Show impressions and position beside clicks, and lead with the outcome metric (leads, conversions) agreed at onboarding. 42% of agencies say clients ask why organic traffic is falling, and 32% why impressions are up but leads are flat (AgencyAnalytics, 2026): answer before they ask.

## What Search Console can and cannot show

- **AI Mode and AI Overview clicks and impressions count inside the web search type, with no filter** (Google, June 2025). A follow-up question in AI Mode is a new query. There is no way to split blue-link clicks from AI clicks.
- **The Generative AI performance report (announced 3 June 2026, on all sites by 31 August 2026) shows impressions only**: how often a link to the site appeared in AI Overviews or AI Mode, by page, country, device and date. No clicks, no CTR, no queries, and two results from the site in one answer count as one impression. Third-party write-ups say it is not in the API, and Manifold's `console_get_search_analytics` cannot read it: the client or user exports it from the Search Console interface. Call it "times shown in Google's AI answers", never traffic.
- **Web search splits into text and multimodal** (September 2026): searches with an image (Lens, Circle to Search) can be filtered, with no query data. For most clients a footnote; for a shop with visual products perhaps not.
- **Totals depend on grouping.** Google aggregates by property for country and date groupings and by page for page groupings, and small sites lose rows to privacy filtering. Take the period total from a country or date grouping and say so ([field notes](field-notes.md)).
- **Data lags about two days and keeps 16 months.** A report run on the 1st for last month is missing its last days; run it on the 3rd or later.

## What GA4 can show

- **GA4 added an AI Assistant default channel on 13 May 2026.** Visits referred by assistants such as ChatGPT, Gemini, Copilot, DeepSeek and Grok get the medium ai-assistant; Google's own AI Overviews and AI Mode stay in Organic Search. Google publishes the referrer list as a spreadsheet; check whether an engine the client cares about (Perplexity, Claude) is on it before calling it absent.
- **It is a real number but a floor.** Apps that strip the referrer send visits to Direct. Report AI Assistant sessions and their key events as "at least".
- **The client's conversions are the number that matters.** `analytics_get_report` with sessions, `keyEvents` and revenue by `sessionDefaultChannelGroup`, this month against last and against the same month last year (free).

## Two breaks in the data

Annotate both on any chart that crosses them, and never present the step as a change:

1. **About 10 to 12 September 2025.** Google stopped honouring the `num=100` parameter that rank trackers used to load 100 results at once. Bot-driven desktop impressions at deep positions disappeared: many sites saw impressions fall and average position improve overnight with no change in what people saw. Compare impressions and average position only on one side of the break, or use clicks.
2. **13 May 2026.** GA4's AI Assistant channel began. Before it, those visits sat in Referral, so a year-on-year table shows Referral down and AI Assistant new. Sum the two for a like-for-like comparison.

## AI visibility

- Give a **mention rate per engine** over prompts times engines, with its margin of error (on 25 answers a share can swing 20 points on noise), and a three-run trend. Method and noise rules are in [ai-search](../../ai-search/SKILL.md). Never present a rank in an answer as a fact: lists rarely repeat.
- Keep **mention and citation apart**: being named and being linked are different outcomes, and only citations can send clicks.
- Keep **brand prompts apart from category prompts**; brand prompts flatter the share.
- **Never sell a guaranteed position.** 66% of agencies say clients ask for AI search work (AgencyAnalytics, 2026); the honest product is a readable site, correct facts, presence on the sources engines cite, and a measured rate.
- **Tie it to outcomes where possible**: AI Assistant sessions and key events in GA4, and branded search impressions in Search Console, which rise when more people hear the name.

## Estimates

`seo_get_domain_overview` traffic is a model, not the client's analytics, and a 10% move can be the model. Use it for competitors, where the client's own data does not exist, and label it every time. For the client, Search Console clicks lead.
