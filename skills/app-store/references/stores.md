# How each store ranks and indexes

What Apple and Google say about search, what practitioners have measured on top of it, and what changed in 2025 and 2026. Checked October 2026. Neither store publishes its weights, and both reshuffle rankings several times a year (AppTweak's change detector flagged Play on 19 June and 21 August 2026), so read any claim about weights as a practitioner's inference.

## Apple's App Store

**What Apple documents** ([App Store search](https://developer.apple.com/app-store/search/), [App Review Guidelines](https://developer.apple.com/app-store/review/guidelines/), last updated 8 June 2026):

- Ranking is text relevance (the app name, subtitle, keyword field and primary category) plus user behaviour (downloads, ratings and reviews "and more"). The secondary category is indexed too.
- **Name 30 characters, subtitle 30, keyword field 100**, comma-separated with no spaces after commas. Apple says not to repeat words already in the name, subtitle or category, not to add plurals of words already used, and to skip "app", "game" and filler words.
- **Promotional text does not affect ranking.** Apple says so; it is for conversion and can change without a new version.
- **Never put competitor or popular app names, trademarks, celebrities or prices in metadata** (guideline 2.3.7). Apple names improper keywords as a common rejection reason and "may modify inappropriate keywords at any time".
- **The description is not in Apple's list of ranking text.** Practitioner tests agree it is not indexed. Write it for the reader and for AI engines that quote listings ([AI answers](ai-answers.md)), not for App Store search.

**What practitioners have measured:**

- **Words combine across the name, subtitle and keyword field** within one localisation, so "budget" in the name and "planner" in the keywords can rank for "budget planner". A word counts once wherever it appears; repeating it adds nothing.
- **Each storefront indexes more than one localisation.** The US store also indexed Spanish (Mexico), Russian, Chinese (Simplified and Traditional), Arabic, French, Portuguese (Brazil), Vietnamese and Korean in AppTweak and Phiture's fake-keyword tests (2022, not re-run publicly since). That is extra keyword space for a US app, but words do not combine across localisations, and deleting a secondary localisation's keywords can drop US rankings.
- **Screenshot captions: unproven.** Appfigures reported in June 2025 that caption text started to affect rankings. ConsultMyApp then tested 64 caption phrases on 8 large apps: 36 did not rank, 27 were explained by existing metadata, 1 was unexplained. Apple has not confirmed it. Captions matter anyway, because up to three screenshots show in search results.

**2025 and 2026 changes:**

- **App Store tags** (iOS 26, 2025). Apple's LLMs generate feature tags from the App Store Connect metadata, with human review; they appear in search results and as tappable entities on the search page and product page. The developer can only deselect tags, not add them. US store only, en_US metadata only ([App Store Connect help](https://developer.apple.com/help/app-store-connect/manage-app-information/manage-app-tags)). The practical lever is a description and metadata that state the app's features in plain words, so the right tags get generated.
- **Custom product pages rank in organic search** (from July 2025). Keywords from the keyword field (not the name or subtitle) can be assigned to a custom product page, which then replaces the default page in search for those terms. Assigning adds no new keywords; each keyword combination belongs to one page. The limit rose from 35 to 70 pages per app in October 2025. App Store Connect reports search impressions per page.
- **AI review summaries** (iOS 18.4, April 2025, US English first, refreshed at least weekly). A paragraph above the reviews, positive first then negative, written from the reviews themselves. A recurring complaint ends up in the summary every visitor reads first ([reviews](reviews.md)).
- **More ads in search** (from March 2026): ads below the top slot as well as at it ([Apple Ads](apple-ads.md)).
- **iOS 27 (announced at WWDC, June 2026):** a product page header and a search results visual shown before the screenshots, and an asset library that lets creative change without a new build. Guidelines and specs were still unpublished in early autumn 2026; check App Store Connect before planning around them.
- **Ratings prompts on first launch** were reportedly rejected under the 2026 guidelines (AppTweak's news log, July 2026; not in Apple's text). Ask after a success moment instead ([reviews](reviews.md)).

**In-app events** ([Apple](https://developer.apple.com/app-store/in-app-events/)): name 30 characters, short description 50, long description 120; up to 10 published and 15 approved at once; each up to 31 days, promoted up to 14 days before. The event card shows in search for people who already have the app (others see screenshots) and can be found by searching for the event. Apple does not say event text is indexed for ranking; practitioners believe the name and short description are. The download lifts published are single-client vendor case studies.

## Google Play

**What Google documents** ([store listing best practice](https://support.google.com/googleplay/android-developer/answer/13393723), [metadata policy](https://support.google.com/googleplay/android-developer/answer/9898842)):

- **Title 30 characters, short description 80, full description 4,000.** The short description shows in search results.
- **No keyword field.** Google tells developers to write the full description "in everyday language", not as a keyword list, and treats repetitive or unrelated keywords as a policy violation.
- **Banned in the title, icon and developer name:** emoji, repeated special characters, all caps unless it is the brand, store performance claims ("#1", "Popular", "Top"), prices and promotions, and anything implying a Play programme ("Editor's choice", "New"). Unattributed testimonials are banned in the description.
- Google documents nothing about ranking weights. The consensus among practitioners is that the title, short description and full description are all indexed, the title most strongly, and that installs, retention, ratings and app quality (Android vitals) weigh in. From 1 March 2026, apps failing the excessive wake-lock vital may be left out of prominent discovery surfaces, and since March listings can show the warning.

**2025 and 2026 changes** (most from AppTweak's dated news log; Google's own posts where noted):

- **AI review summaries** ("Users are saying", Gemini) rolling out from late October 2025 on apps with enough reviews.
- **Guided Search** (from September 2025): broad queries show previews of narrower sub-queries, with generated highlight cards from listing text and reviews.
- **Ask Play** (from December 2025, confirmed at I/O in May 2026; English, outside the EEA): a Gemini chat on the listing that answers from the description, reviews and the developer's website. Vague marketing copy produces vague answers. AI results can sit above organic results for conversational queries.
- **The Gemini app recommends Android apps** with an install card (July 2026), outside Play entirely.
- **Custom store listings:** up to 50 per app, each with its own title, icon, descriptions and graphics, targetable by country, by pre-generated search keywords (since 2024, from keywords that brought at least one visitor in 30 days), and from August 2026 by purchase status for some developers.
- **Install counting moved from device-based to account-based** (June 2026), so install figures across that date are not comparable.
- Reported but unconfirmed: a longer short description and portrait-first screenshots in testing (August to September 2026).

## How the stores differ, for the work

| | App Store | Google Play |
|---|---|---|
| Indexed text | Name, subtitle, keyword field, categories (captions unproven) | Title, short and full description |
| Where keywords go | 160 characters across three fields, plus secondary localisations | Natural language across the description, no stuffing |
| Changing text | Needs an app version, except promotional text | Any time, no review of a new build |
| Testing | Product page optimisation (icon, screenshots, previews) | Store listing experiments (text and graphics) |
| Rating shown | Per storefront; can be reset with a new version | Weighted to recent ratings, per country and device |
| AI layer | Review summaries, tags | Review summaries, Ask Play, Guided Search |
