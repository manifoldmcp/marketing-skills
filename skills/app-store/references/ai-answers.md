# Apps in AI answers

People increasingly ask an assistant which app to get, and both stores now put AI between the search and the listing. Checked October 2026. Measuring and improving a brand in AI answers in general belongs to [ai-search](../../ai-search/SKILL.md); this page is what is specific to apps.

## How much it matters

- **Survey:** 58% of 1,000 US adults had used an AI assistant at least occasionally to find or choose an app or game; 10% first heard of their most recent download from one, about level with Google search (10.7%) and social media (10.4%). ChatGPT (39%) and Gemini (31%) lead (AppTweak with Prolific, August 2026, self-reported, ±3 points).
- Most still check the store before installing, so the listing still closes the install; the AI answer decides whether the app is on the shortlist at all.

## What engines cite for app questions

- **ChatGPT:** store listings were 47.5% of its cited sources on non-branded app prompts (App Store 37.6%, Google Play 9.6%), the open web 27%, and 22.6% of recommendations cited nothing (AppTweak, over 125,000 US responses to about 18,500 app and game prompts, May 2026; a vendor study, ChatGPT only). Sources were cited for 80% to 85% of the apps in positions one to four and under 25% by position eight. Store share varied by category, from 65.8% for weather to 29.3% for food and drink.
- **Our own run was different** ([field notes](field-notes.md)): on five prompts across six engines, 29 of 261 citations (11%) were store pages, and 27 of those came from the two prompts that named a platform ("for iPhone", "for Android"). On generic prompts ("best budgeting app"), no engine cited a store page; they cited publishers, the apps' own sites and listicles. Six of ChatGPT's ten store links were product-card links to a store search, not retrieved pages.
- **Listicles dominate the rest**, including app makers' own "best X apps" posts: 173 of the 261 citations in our run had list titles, and on the habit-tracker prompt all six of Claude's citations were "best habit tracker apps" lists, most published by app makers or app directories. On Google's surfaces this kind of self-ranking list now tends to backfire ([ai-search](../../ai-search/SKILL.md)).
- **Perplexity cites listings from other storefronts** (Turkish, Indian, Dutch App Store pages for a US prompt) and Apple's editorial story pages; a listing's text matters in every storefront it appears in.

## Inside the stores

- **Google Play:** Ask Play (Gemini on the listing, from December 2025; English, outside the EEA as of May 2026) answers from the description, reviews and the developer's website, and has been seen answering from the website instead of the listing. AI results can sit above organic results for conversational queries, and Guided Search previews narrower sub-queries. The standalone Gemini app recommends Android apps with an install card (July 2026).
- **Apple:** AI review summaries and LLM-generated tags (US) are built from the listing and reviews; iOS 27 adds personalised collections (reported, English first). Apple has not said its search uses an LLM for ranking.

## What to do

These are reasoned from how the systems read listings; no study has measured a before-and-after lift.

1. **Make the listing say what the app does, in plain words.** The first lines of the description should name the app's type, who it is for and its main functions ("a budgeting app for couples that splits bills and tracks shared goals"). Engines and store AI quote it, Ask Play answers from it, and Apple's tags are generated from it. Slogans give them nothing to quote.
2. **Keep the website, the listing and the app's own help pages consistent**, since Ask Play and the web engines read all three.
3. **Fix the review themes** that the store summaries and engines repeat ([reviews](reviews.md)).
4. **Get onto the lists engines cite for the category** (publishers first), and appear in the "for iPhone" and "for Android" answers, where store listings are cited most. Finding and approaching those pages is in [ai-search](../../ai-search/SKILL.md) and [link-building](../../link-building/SKILL.md).

## Measuring

`aeo_run_ai_answers` with the category prompts buyers ask (with and without "for iPhone" or "for Android"), the user and up to nine rival apps in `brands`, on the engines buyers use: 27 credits a prompt on the default five, 29 with `ai_mode`. Count mentions per engine and the share of citations to store pages against the apps' own sites and lists. Answers vary run to run; the noise rules and the by-hand method are in [ai-search](../../ai-search/SKILL.md).
