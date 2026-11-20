# Store keyword research

Which store searches to target and where each word goes. Store search is its own channel: people type short, functional queries ("budget", "habit tracker", "pdf scanner") and brand names, and the volumes are modelled, never counted. Checked October 2026.

## Where demand numbers come from

- **Apple publishes only a relative score.** Apple Ads shows a Search Popularity of 5 to 100 per term and storefront; it is an index, not a count, and a wide band of long-tail terms sits on the floor of 5. Apple does not publish its method. Anyone with an Apple Ads account can see it free, which makes it the best demand signal for iOS.
- **Google publishes nothing for Play search.** Play Console's acquisition report shows the search terms that brought visitors to the user's own listing, which is real data but only for terms the app already gets traffic from.
- **Every third-party volume is a model.** Vendors convert Apple's score with their own curves, or estimate from autocomplete and rank positions, and they disagree with each other. No independent accuracy test exists. Manifold's `volume` comes from DataForSEO Labs, which does not document how it is derived. On the App Store it runs small ("habit" 980, "habit tracker" 177, most long-tail terms 0 in our October 2026 test); on Play it came in steps of 10 (10, 20, 30) with a few larger values. The two stores' scales are not comparable.
- **So: rank terms against each other within one source, never quote a volume as searches.** Two terms an order of magnitude apart are genuinely different; two within a factor of two are a tie. Do not compare a store volume with a Google web volume from [seo](../../seo/SKILL.md): different engines, different models.

## Finding the terms

The tools work from apps, not from seed words: there is no store keyword search, only the searches an existing app already ranks for.

1. **The competitor set first.** `apps_get_competitors` on the user's app (or on the category leader, for an app not yet launched) returns apps ranking for the same searches, with `shared_keywords`, `avg_position` and `top_3`. Name them with `apps_get_app`. Drop the giants that rank for everything (a weather or browser app sharing ten generic terms is not a competitor) and keep apps with a high share of overlap ([competitors](competitors.md)).
2. **Their keywords.** `apps_get_keywords` on three to five close competitors (10 credits each for 100 rows). Rows come best rank first, then volume, so the first rows are the app's own name and the terms it owns; page with `cursor` to reach the terms it ranks 10 to 50 for, where the opportunities sit. A big app can fill whole pages with first places (Habitica's first 50 Play rows were all rank 1, of 5,523), and its full list costs hundreds of credits; take a few hundred rows and sort them by volume yourself. A term two or more competitors rank top ten for is proven relevant to the category; a term one competitor ranks for is a lead to check.
3. **The gap.** `apps_get_shared_keywords` with the user's app first and the competitors after it (2 to 20 apps) shows each term every app ranks for, with each rank side by side, sorted by volume. It returns only terms all the apps rank for, so it shows the near wins (the user at 4 to 15, a rival above) but never the gap. The gap is the terms in a competitor's `apps_get_keywords` list that are missing from the user's own list: compare the two lists.
4. **The user's own buyers' words.** Review text (`apps_get_reviews`) and the problem language from [voice-of-customer](../../voice-of-customer/SKILL.md) give the functional phrases people use ("split bills", "offline maps") that a competitor list misses.

## Choosing

Score each candidate on relevance first, then demand, then how winnable the results page is.

- **Relevance is a gate, not a weight.** A term people search for that the app does not answer pulls impressions that do not convert, and installs and conversion feed ranking on both stores. If the screenshots would not answer the search, drop it.
- **Clean the list first.** Store keyword lists mix in terms that only share a word with the app: a habit app called Streaks ranked for Snapchat-streak questions and for cleaning streaks off a fridge, plus misspellings ("habeit", "hubit") and rival brand names. On Play the terms read like web searches ("habitica reddit", "habitica login", "goal setting software"), so check a Play term in the store's own autocomplete before building copy on it.
- **Winnable:** do not rank by `results_count`: the App Store counts at most about 250 results, so it sat at 245 to 250 for about four terms in five, and Play sends none. A figure well under 250 marks a thin search; otherwise read the top of the results, and do not trust any difficulty score: if the top five all have tens of thousands of ratings and the user has hundreds, the term is a later goal. Third-party difficulty scores are vendor-specific and unvalidated.
- **Long tail first for a small app.** A new app rarely ranks for a one-word head term. Two- and three-word phrases with modest volume, where the top apps are weak, are where early installs come from, and those installs lift the app on the head terms later.
- **Brand terms are separate.** A competitor's brand name is off limits in Apple metadata (guideline 2.3.7) and risky on Play under the metadata policy; reach it with Apple Ads instead ([Apple Ads](apple-ads.md)). The user's own brand should rank first; if it does not, that is the first problem.

## Placing the words

**App Store (160 characters plus secondary localisations):**

- The name carries the brand and the single most valuable term; the subtitle the next most valuable phrase, written to convert, since it shows in search.
- The keyword field holds everything else: single words, comma-separated, no spaces, no repeats of words in the name or subtitle, no plurals of words already used, no "app". Words combine across all three fields within a localisation, so single words in the field build phrases with words in the name.
- Use the secondary localisations the storefront also indexes ([stores](stores.md#apples-app-store)) for overflow terms in the market's language; words do not combine across localisations.
- Keywords assigned to custom product pages must be in the keyword field already ([stores](stores.md)).
- Changing the name, subtitle or keyword field needs a new app version. Change in batches, one release at a time, and wait two to four weeks before judging; ranking reacts to installs that follow, not to the edit alone.

**Google Play (title, short description, full description):**

- The title carries the brand and the main term within 30 characters. The short description is a sentence of value in 80 characters, with the next term in it naturally.
- The full description covers each important theme in plain sentences. The common practitioner rule is to use the main term a handful of times across 4,000 characters, never as a list; Google's policy calls repetition and keyword lists a violation, and Ask Play now answers from this text, so it must read well.
- Play text can change without a new build, which makes it the faster place to test a term, but each change resets what Google has learned; do not change it weekly.

## Re-measuring

Keep the term list, the date and the rank per term. `apps_get_keywords` on the user's app every two to four weeks shows rank movement (cached 7 days, so a second call within a week returns the same data, free). The vendor's keyword data was already two to four weeks old on the day of our test, so a rank change shows up late; date each pull by the `data_as_of` it reports, not the day it was run. Only US English is covered by the keyword tools; for other markets, rank must be checked by hand in that storefront.

## Without the connector

By hand: type each candidate into the store's search on a device set to the target country, note the autocomplete suggestions (they reflect real query frequency, roughly), the top ten apps and their rating counts, and where the user's app appears. Apple Ads' Search Popularity, free with an account, is the best iOS demand number available to anyone. Say plainly that this gives an order, not volumes.
