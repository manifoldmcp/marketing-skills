# Field notes

What Manifold has seen in its own pulls of keyword and company data. Small samples, read as evidence of how the data behaves, not as market sizes. Newest first.

## Keyword volume, Labs against Keyword Planner, with AI volume (15 October 2026)

Twenty US English keywords (head terms, long tail, problem phrases, new AI-era terms, two consumer products) through `seo_get_keyword_metrics` twice: the default Labs source with `ai_volume: true` (11 credits), then `source: "google_ads"`, Google Keyword Planner's own figures (24 credits). Then eleven of them, English and German, at `location: "Germany"` and `language: "de"` (6 credits).

**Labs is Keyword Planner's buckets, a month behind.** Of the 18 keywords both sources answered, 10 had the same `volume` and the other 8 differed by exactly one bucket step (165,000 against 201,000; 480 against 390; 12,100 against 9,900). Sixteen of the twenty Labs `trend[12]` windows ended in August 2026, the Keyword Planner ones in September, which explains every difference: the headline is a 12-month mean and the window had moved. Every value was a Keyword Planner bucket (90, 110, 320, 480, 720, 1,000, 1,300, 1,600, 4,400, 5,400, 8,100, 12,100, 14,800, 60,500, 90,500, 135,000, 165,000, 201,000). Paying 24 credits for `google_ads` buys the latest month, not a different measurement.

**Keyword Planner knew nothing about two new terms Labs did.** "generative engine optimization" and "ai visibility tool" came back all null from `google_ads` and at 4,400 and 1,600 from Labs. A null is no data, not zero, and a new category's words are where the sources disagree most.

**Generic English software terms jumped in 2026, in Google's own data.**

| Keyword (Keyword Planner, US) | Lowest month | May 2026 | Sept 2026 | Headline `volume` | Median month |
|---|---|---|---|---|---|
| crm software | 74,000 (Feb, Mar) | 550,000 | 550,000 | 201,000 | 150,000 |
| project management software | 40,500 (Dec) | 550,000 | 550,000 | 201,000 | about 113,000 |
| email marketing software | 3,600 (Dec) | 450,000 | 550,000 | 135,000 | about 38,000 |

The same step appeared in Germany on the English terms ("email marketing software" 210 to 720 a month until February 2026, then 5,400 to 14,800; "project management software" 880 to 14,800) but not on the German ones ("projektmanagement software" held between 880 and 2,400). Long-tail, problem and product terms did not move ("applicant tracking system", "ai sdr", "running shoes for flat feet"). The cause is unknown: Google has said nothing, and automated queries are one candidate. Until it is explained, read `trend[12]`, and on a generic head term with a step change size from the months before the step or the median month, and say why.

**Germany buys software in English and loanwords.** "crm system" 14,800 and "crm software" 8,100 against "kundenverwaltung software" 90; "applicant tracking system" 1,300 against "bewerbermanagement software" 320; "newsletter software" 480. "ki protokoll meeting", a literal translation of "ai meeting notes", had no data at all. A literal translation would have sized this market at almost nothing.

**English keywords take the country's language code.** The same English keywords at `location: "Germany"` with `language: "en"` were rejected as `InvalidTarget` (no charge). Pass English keywords with the market's own `language`; the volume is the country's searches for that string.

**`ai_volume` bears no stable relation to search volume.** As a share of Google volume it ran from 0.07% ("email marketing software", 62 against 90,500) through 1.2% ("crm software"), 12.6% ("standing desk") and 34% ("hubspot alternative") to 77% ("ai meeting notes", 768 against 1,000). On the head terms `ai_trend` fell by about 40% to 60% between October 2025 and June 2026, a period in which ChatGPT's use grew, then recovered in September; "mcp server" rose eightfold while its Google volume stayed flat. Several small rows doubled in September 2026 alone. It moves with Google's People Also Ask data, which is what it is built from, not with what people ask assistants.

## Counting a local vertical abroad (15 October 2026)

`leads_search_companies` with `industries: ["Medical Practices"]`, `keywords: ["dental"]`, `locations: ["Germany"]` and `limit: 25` (4 credits). `rows_available` was 2,277. Of the 25 rows fetched, at most three could be dental practices in Germany; the rest were dental labs, depots, equipment makers, a brush factory, a marketing agency, a professional network, and one practice in Puyallup, Washington, filed under Germany. Industries are matched as keywords, so a category word pulls in everyone who sells to the category. For local and small-business verticals the account index cannot size the market; use public business statistics instead (see [sizing](sizing.md#counting-accounts-bottom-up)).

Credits spent on these notes: 45.
