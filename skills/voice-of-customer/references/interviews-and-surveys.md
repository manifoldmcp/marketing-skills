# Interviews and surveys

When public posts are not enough, and how to ask customers directly without wasting them. Manifold has no interview or survey tool: this is the method the user runs, and what to expect from it.

## When to go beyond public posts

Public text is strong for needs, pains and language ([sources](sources.md#public-posts-against-interviews)). Ask people directly when the question needs the person: who signs and with what budget, what they did the last time the problem came up, why a deal was lost, how a niche or enterprise buyer thinks (they rarely post), or how many people hold a view (only a sampled survey answers that). Mine public posts first: they make interviews shorter and sharper.

## How many interviews

| Finding | Source |
|---|---|
| A voice-of-the-customer study typically starts with 20 to 30 experiential interviews | Griffin and Hauser (1993), as summarised by Timoshenko and Hauser (2019) |
| Empirical studies reached saturation at 9 to 17 interviews or 4 to 8 focus groups, for homogeneous groups and narrow objectives; multi-country or meta-theme studies needed more | Hennink and Kaiser, systematic review of 23 studies (Social Science and Medicine, 2022) |
| Code saturation (the range of issues) came at about 9 interviews; meaning saturation (understanding them) needed more, around 16 to 24 | Hennink, Kaiser and Marconi, 25 interviews (Qualitative Health Research, 2017) |
| Stopping when two more interviews add under 5% new themes happened at a median of 6 to 9 interviews and held 62% to 79% of all themes; stopping at no new themes took 11 to 12 and held 69% to 87% | Guest, Namey and Chen, bootstrap of three datasets of 40 to 60 interviews (PLOS ONE, 2020) |

Use it so:

- **Per segment, not in total.** Twelve interviews across four personas is three per persona: not saturated anywhere.
- **Plan 12 to 15 per homogeneous segment** when the goal is the range of needs; 20 to 30 when product decisions depend on the long tail.
- **Report the stopping rule and the curve** (new themes per interview), not "we reached saturation".
- In Griffin and Hauser's data (Marketing Science, 1993), two one-hour interviews found about as many needs as one two-hour focus group (51% against 50% of 230 needs); groups add how people argue with each other, not more needs.

## Interviewing well

Ask about past behaviour, not opinions or the future: "tell me about the last time you chased an invoice" beats "would you use a tool that..." (people are poor predictors of what they will do, and polite about ideas). Ask what they tried, what it cost, what they use now. Do not pitch. Record and transcribe with consent; code transcripts the same way as posts in [pain points](pain-points.md).

## Surveys

- **Response rates:** in-app surveys averaged 27.5% response (24.8% complete) across 1,382 surveys and 6.1 million responses; mobile apps 36.1%, web apps 26.5%; NPS surveys 21.7%, CSAT 26.3%; a centre-screen modal in a web app 42.6% (Refiner, vendor benchmark, 2025, surveys with at least 100 views and 50 responses). Email and link survey benchmarks published in 2025 and 2026 carry no method; treat any single figure as a guess.
- **Rate is not representativeness.** A meta-analysis of 59 studies found the response rate a weak predictor of nonresponse bias (Groves and Peytcheva, Public Opinion Quarterly, 2008). A 40% response from the happiest customers is worse than 10% from a fair cross-section: compare respondents with the customer base on plan, tenure and usage.
- **Survey to count, interview to discover.** Write closed questions from needs already found; an open "what's your biggest challenge" at scale gets short, vague answers that need coding.
- **Synthetic answers are no substitute** for the survey ([synthetic respondents](synthetic-respondents.md)); a model can pilot the wording.
- **Product-market fit question** ("how would you feel if you could no longer use it", with "very disappointed" over 40% as the bar) is a heuristic from Sean Ellis's comparison of startups, never published with its data. Track it over time rather than against 40%.

The user sends surveys and runs interviews, never the agent: no Manifold tool contacts anyone.

## Without the connector

The whole method works by hand, slowly:

1. Ask the user for what they already hold: sales call notes, support tickets, churn reasons, NPS comments, reviews of their product. These beat any public source.
2. Search Reddit, review sites and YouTube in a browser with the problem phrasings from [pain points](pain-points.md#collect); read the threads with the most replies; log each statement with its link, author and date in a sheet.
3. Read 1-star to 3-star reviews of the top three competitors on G2, Capterra, Trustpilot, the app stores or Amazon, remembering that ratings are polarised ([sources](sources.md#who-is-talking)).
4. Cluster, count distinct authors and rank as in [pain points](pain-points.md#count-and-rank).

Say plainly that a hand-collected sample of a few dozen posts finds the common themes and misses the rare ones, and give its size.
