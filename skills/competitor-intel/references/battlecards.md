# Battlecards

One page sales can use in a deal against one competitor: its claims and the user's counter to each, where it wins, the questions that bring its weak spots up, the objections it plants, and quick facts, with a source on every line. Checked October 2026.

## What the evidence says about battlecards

- **No controlled study shows battlecards raise win rates.** The evidence is vendor surveys of competitive intelligence teams, reporting associations. Crayon's 2026 report (its ninth; sample size not published) has 49.6% of teams seeing competitive win rates rise over the past year against 6.3% falling, and rising win rates reported by 66% of teams that track KPIs against 24% that do not; Crayon itself says these are associations. Do not promise a win-rate lift; promise a card reps trust.
- **Most deals are competitive.** 57.5% of teams say more deals are competitive than a year ago, and seven in ten say at least half are (Crayon 2026); only 1.5% of 3,400+ interviewed deals had no competitor (Klue, December 2024, AI-coded interviews, vendor).
- **Sellers misread losses.** About 60% of sellers are partly or wholly wrong about why they lost (Anova's win-loss work, around 2014, still cited by Klue in 2026; old and unreplicated). A loss reason from the CRM is a hypothesis to check against buyer evidence, not a counter to build on.
- **AI-drafted cards need checking.** 83% of CI teams use AI for battlecard content, 76% have received AI output they could not vouch for, 73% name confident answers built on weak or unverifiable sources as the top problem, and 79% will not send AI output to sellers unreviewed (Klue, 250+ CI and PMM staff, 2026, vendor survey). The common failure is a wrong claim about the competitor's product. Never put a price, a feature or a customer count on a card from memory or from an AI answer.

## Building it

| Section | Source | Manifold calls |
|---|---|---|
| Their claims | Homepage and pricing headings, active ads' offers | `seo_get_page` (free); `ads_get_advertiser_ads` on facebook with `active_only: true` and on linkedin (2) |
| What the prospect hears from AI | "What are the downsides of <rival>?" and "<rival> vs <user>" | `aeo_run_ai_answers` with `engines: ["chatgpt", "gemini"]` (8) |
| Where it fails | Dated complaints and switch stories ([complaints](complaints.md)) | `reddit_search_posts`, `reddit_search_comments`, `reddit_get_comments` on the busiest threads (about 9) |
| What buyers compare | "<rival> vs" keywords and who owns the "<rival> vs <user>" result | `seo_search_keywords` (10), `seo_get_serp` (1); if the rival owns it, its `h2[]` are the arguments the prospect arrives with |
| Quick facts | Both headcounts, pricing model and entry price where read, active offers | `leads_get_company`, `linkedin_get_company` (2) |

About 32 credits. Reuse a [messaging](messaging.md) table or [complaints](complaints.md) run for the same rival instead of calling again.

## The card

- **Their claims, our counters:** each claim, the user's answer and the proof a rep can show in the call (a number, a customer, a demo). A counter without proof is marked "claim only"; a claim the user cannot answer stays with "concede".
- **Where they win:** what complaints, AI answers and the user's deal notes say the rival does better, and how to handle it (narrow the fit, reframe the cost, or walk away). A card without this section is not trusted by sales, and honesty about strengths is what makes the counters credible.
- **Landmine questions:** three to five questions that bring a weak spot up without naming the rival ("How do you handle X when Y?"), each tied to a dated quote. Questions, not attacks: a rep who runs down a competitor loses the buyer's trust.
- **Objections they plant:** what their pitch and the AI answers tell prospects about the user, with the reply. A wrong AI claim about the user is fixed at the source ([ai-search](../../ai-search/SKILL.md)).
- **Quick facts:** headcount from both sources, pricing model with source and date, active offers, who owns the "vs" search.

Every claim about the rival must be true and provable when said to a buyer: false statements about a competitor's product are false advertising or disparagement wherever they are made ([ethics and law](ethics.md)).

## Keeping it alive

Date the card. Refresh it when the rival changes pricing or homepage claims ([monitoring](monitoring.md) flags both) and at least each quarter; drop any weakness older than two years. A lost-deal reason the public evidence also shows goes to the top. Never send the card or contact anyone; hand it to the user.
