# An app store plan

When the user wants a plan rather than one check: more installs from store search and browse over 90 days, for an app that is live (or launching), on one or both stores. Checked October 2026.

## Order of work

The order follows what blocks what. Skip a step only when the baseline shows it is fine.

1. **Rating and reviews.** A rating below 4.0, or a complaint written into the AI review summary, caps conversion on every term, so fix the top complaint and set up the system rating prompt first ([reviews](reviews.md)).
2. **Text the store indexes.** Name, subtitle and keyword field on iOS; title, short and full description on Play, built from competitor keyword data ([keywords](keywords.md)). On iOS this ships with the next version, so it sets the release calendar.
3. **The first impression.** The first three screenshots, the icon and the subtitle or short description, rewritten to match the terms the app now targets, then tested where traffic allows ([listing audit](listing-audit.md)).
4. **Intent pages.** Custom product pages for iOS keyword clusters and custom store listings on Play for countries or keyword groups, once the default page is sound.
5. **Recurring visibility.** In-app events on iOS and promotional content on Play around real launches and seasons; AI answers and the lists they cite ([AI answers](ai-answers.md)).
6. **Paid, if there is budget.** Apple Ads on the terms that convert, with a holdout before claiming brand-campaign wins ([Apple Ads](apple-ads.md)).

## Baseline

From the user: store analytics for the past 90 days (impressions, product page views, installs, conversion by source, for search and browse apart), the keyword field, rating per main country, and any Apple Ads data. From Manifold, about 70 credits for one store: the listing and three to five rivals (`apps_get_app`, 6), competitors (`apps_get_competitors`, 10), keywords for the app and three rivals (`apps_get_keywords`, 40), shared keywords (`apps_get_shared_keywords`, 10), 300 newest reviews of the app (3) and its category chart (1). Add about 100 for an AI answers check on five prompts. Double the store calls for both stores.

## The 90 days

| Period | Typical work | KPI to re-measure |
|---|---|---|
| Days 1 to 30 | Top complaint fixed or answered, rating prompt at a success moment, review replies cleared, new keyword set and text drafted, first screenshot rewritten; iOS changes in the next release | Rating in the main countries; reply rate; release shipped |
| Days 31 to 60 | Screenshot or icon test running (where traffic allows), Play description and short description live, first custom product page or custom store listing, one in-app event or promotional content | Rank on the target terms against baseline; product page conversion by source |
| Days 61 to 90 | Second keyword iteration from what moved, the winning creative applied, more intent pages, AI answer re-check | Search impressions and installs from search; conversion against the category's peer benchmark |

Give each period an owner and three first actions for this week. The user ships every change in App Store Connect or Play Console; draft it, never submit it.

## Setting targets honestly

- Promise inputs and rank movement on chosen terms, not installs from a number of searches: store volumes are models ([keywords](keywords.md)).
- Ranking reacts over two to four weeks after an iOS release, and both stores reshuffle several times a year; a fall right after a store-wide algorithm change is not the user's change. Check a public change detector before blaming the release.
- On iOS, separate Apple Ads installs from organic before reading the search trend, since App Store Connect counts both under search.
- A test on a small app may never reach 90% confidence; set targets on the trend, and say so.
- The keyword tools cover the US store in English. Plans for other markets need rank checks in each storefront by hand, or a store tool the user already pays for.
