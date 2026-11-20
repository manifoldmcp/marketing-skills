# Field notes

What Manifold has seen in its own runs, United States, English. Small samples: read them as evidence of how the engines and tools behave, not as market shares. Public apps only. Newest first.

## Store tools on habit trackers (16 October 2026)

Every `apps_*` tool once or twice on public habit-tracker apps, both stores, after the 16 October change that put an app's best ranks first in `apps_get_keywords` and turned Play listing HTML into plain text. 15 paid calls, 113 credits; one repeat call came back cached and free, and one refused input (a wrong category id) cost nothing.

| Call | Inputs | Rows | Credits |
|---|---|---|---|
| `apps_get_top_charts` | top_free, Health & Fitness, both stores | 100 + 100 | 2 |
| `apps_get_app` | Streaks, Habitica (App Store); Habitica, Loop Habit Tracker (Play) | 4 | 4 |
| `apps_get_keywords` | Streaks (App Store, 100); Habitica (Play, 50) | 150 | 18 |
| `apps_get_shared_keywords` | Streaks and Habitify (App Store, 50) | 45 | 8 |
| `apps_get_competitors` | Streaks (App Store, 20); Loop (Play, 20) | 40 | 12 |
| `apps_get_reviews` | Streaks newest; Loop newest; Habitica most relevant (Play) | 300 | 3 |
| `apps_search_apps` | "habit tracker" (Play, 5); "habit" (App Store, 5) | 10 | 66 |

Every call returned inside the client's wait; latency was not timed. Charges matched the estimates to the credit.

**Listings (`apps_get_app`, 4 apps plus 10 search rows):**

- Play `installs` is an exact count (Habitica 8,369,496; Loop 7,909,650), not the store's bucket.
- Play gave developer email and address on both apps; App Store gave neither, and `developer_website` was empty on every App Store row although Streaks' page links its site. The vendor sends no website, email or address for App Store apps, so the tools document the three as Play only. One Play search row's contact address was a personal webmail address on a company listing; we did not record it.
- App Store oddities, each checked against the live page: Streaks' `category` was null (the page says Health & Fitness); `genres` was empty on all App Store rows; Streaks' `description` began with its promotional text; a restaurant app with no subtitle came back with its category, "Food & Drink", as `subtitle`; `price` and `currency` were null on free apps; `version` carried the word "Version"; screenshots were 512-pixel thumbnails. Fixed after this run: a subtitle that is a category name is now null with the name in `category` (and `genres`), `genres` carries the store's categories, a free app's `price` is 0, and `version` is the bare number. Still true, because the vendor sends nothing better: Streaks' `category` is null, the promotional text sits in `description` (the tool description says so), and `currency` is null on free App Store apps.
- Play oddities: `content_rating` and `size` empty on all 7 rows, `version` on 6 of 7. The vendor's Play listing has no age rating or size at all; the tool description now says so.

**Keywords (`apps_get_keywords`, `apps_get_shared_keywords`):**

- The own name now leads: Streaks' first eight rows were rank 1 brand terms ("streaks app", volume 52). Habitica's first 50 Play rows were all rank 1, out of 5,523.
- App Store volumes were small: "habit" 980, "habit tracker" 177, "best habit tracker app" 79, and 74 of Streaks' 100 rows at 0 or 1. Play volumes came in steps of 10 below 100, with "habitica" at 583.
- About a quarter of Streaks' rows were unrelated searches sharing the word "streaks" (Snapchat streaks, cleaning streaks off a fridge) or odd matches ("25th id commander"). Habitica's Play rows read like web searches ("habitica reddit", "habitica login", "goal setting software").
- `results_count` was never above 250 and was 245 to 250 on about 80 of 100 App Store rows; it was null on every Play row. The vendor counts at most about 250 App Store results and sends none for Play; the field's description now says so.
- Keyword data was dated 19 September to 3 October, two to four weeks before the call.
- Shared keywords returned 45 terms, exactly the `shared_keywords` count on Habitify's competitor row: intersections only, no row where one app is absent.

**Competitors (`apps_get_competitors`):** Streaks' App Store list (7,117 available) mixed habit trackers with WhatsApp (1.25 million keywords, 47 shared). Loop's Play list (8,106 available) was 20 habit and to-do apps, sharing 641 to 1,281 terms each. No data date is returned.

**Reviews (`apps_get_reviews`):**

- Newest order held on both stores. The newest 100 for Streaks (App Store) ran from 9 October 2026 back to 26 May 2025; for Loop (Play) from 15 October back to 10 September 2026.
- Habitica's 100 most relevant on Play ran from 2018 to September 2026 (5 from 2026), averaged 457 characters, and 84 carried a developer reply; ratings were 45 five-star, 24 four, 19 three, 9 two, 3 one, against a 4.7 average on the store.
- Loop's newest 100 were 91 five-star; `app_version` was empty on 16 and `helpful_count` set on 2. Several were not in English.
- No App Store row had `developer_reply` or `helpful_count`, as documented.
- `rows_available` was the app's rating count, not the reviews a call can fetch. It is now set only when a call reaches the end of what can be fetched, and null otherwise.

**Charts and search:** the App Store refused the category id "health-fitness" and took "health_and_fitness". A later check took `games_puzzle` on the App Store and `game_puzzle` on Play; the tool description now lists each store's ids, and a name such as "Health & Fitness" is turned into its id. Play chart rows had no `ratings_count` (the vendor sends none); App Store rows had no `developer`, and their ratings were unrounded (4.81174). `apps_search_apps` matched any word: "habit tracker" on Play returned Period Calendar, Moovit, Flo, Flightradar24 and adidas Running, in falling order of installs; "habit" on the App Store returned three habit trackers, a meditation app and Habit Burger Grill, in falling order of rating count. Its description now says both.

## App prompts on six engines (15 October 2026)

Five prompts ("best habit tracker app for iPhone", "best budgeting app", "best app to learn Spanish", "best meditation app for anxiety", "best running app for Android") once each on ChatGPT, Claude, Gemini, Perplexity, AI Overview and AI Mode with `aeo_run_ai_answers`: 30 answers, 261 citations, 100 credits. Models reported: gpt-6 (ChatGPT app), claude-sonnet-5-5, Gemini 3.5 Flash-Lite (app), sonar-reasoning-pro.

**Store pages are cited when the prompt names a platform, and rarely otherwise.**

| Prompt | Citations | To apps.apple.com or play.google.com |
|---|---|---|
| best habit tracker app for iPhone | 59 | 21 |
| best running app for Android | 47 | 6 |
| best meditation app for anxiety | 55 | 2 |
| best app to learn Spanish | 56 | 0 |
| best budgeting app | 44 | 0 |

| Engine | Citations | Store pages | Reddit or YouTube |
|---|---|---|---|
| ChatGPT | 49 | 10 | 0 |
| Claude | 26 | 0 | 0 |
| Gemini | 39 | 1 | 0 |
| Perplexity | 87 | 17 | 0 |
| AI Overview | 36 | 1 | 8 |
| AI Mode | 24 | 0 | 7 |

- Overall 11% of citations went to store pages, against AppTweak's 47.5% for ChatGPT on app prompts in May 2026. Our ChatGPT figure was 20%, all of it on the two platform-named prompts. The difference may be prompt wording, method (six of ChatGPT's ten store links were product-card links to a store search, such as a "Get on Google Play" search for Strava, not a cited listing) or the model; five prompts cannot settle it.
- **Lists dominate:** 173 of 261 citation titles read as lists ("best", "top N"). On the habit-tracker prompt all six of Claude's citations were "best habit tracker apps" pages, most published by app makers (habitbox.app, easyhabits.io, loggd.life) or app directories (iosapplists.com). Perplexity also cited third-party app trackers (Sensor Tower, appstoretracker.com, mwm.ai, apptracker.ai).
- **Perplexity cited App Store pages from other storefronts** for a US prompt (Turkey, India, Netherlands, United Kingdom) and Apple's editorial story and room pages.
- On generic prompts, ChatGPT linked the apps' own sites (ynab.com, monarch.com, headspace.com, calm.com), with publishers such as NerdWallet and Kiplinger beside them.
- ChatGPT's fan-out searches named the apps it already had in mind ("best Android running apps Strava Nike Run Club adidas Running Runna 2026 reviews"); Claude's were the prompt plus "2026". Reddit and YouTube appeared only on Google's AI Overview and AI Mode, as in [ai-search](../../ai-search/SKILL.md)'s own runs.
- Gemini answered the habit-tracker prompt partly from Google Shopping-style product cards (google.com citations naming apps by feature), so its brand counts on app prompts can be low.

**Still to test:** how `apps_get_keywords` volumes rank against Apple Ads Search Popularity for the same terms, which needs an Apple Ads account.
