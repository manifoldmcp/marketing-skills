# Competitor apps

Who the user's app competes with in the stores, where each wins, and what their listings, charts and releases show. Checked October 2026. Business competitors in general (web traffic, ads, pricing, positioning) belong to [competitor-intel](../../competitor-intel/SKILL.md); this page is the store side.

## Store competitors are not business competitors

- `apps_get_competitors` ranks apps by how many store searches they share with the user's app. It answers "who takes our search traffic", not "who do buyers compare us with". Large general apps share generic terms with everything, and an app in a neighbouring category can share many searches while selling to nobody the user sells to. In our test the App Store list for Streaks (a habit tracker) had WhatsApp ninth, with 47 shared terms out of 1.25 million it ranks for; the Play list for Loop Habit Tracker was all habit trackers. Rows come sorted by `shared_keywords`.
- Read the columns together: `shared_keywords` (overlap), `avg_position` (how strongly it ranks on the shared terms), `top_3` (how many of its terms rank in the top three) and `keywords` (its whole footprint). A competitor with a high share of its `keywords` in the overlap is a direct rival; one with a vast footprint and a small overlap is a giant passing through.
- Cross-check with what buyers say: who the user's reviewers mention switching from (`apps_get_reviews`), who the store lists under `similar_apps[]` on the user's listing (`apps_get_app`), and who AI engines name for the category ([AI answers](ai-answers.md)). An app named by two or more of these is a competitor.
- The tool returns ids only. `apps_get_app` names each one at 1 credit. Ten competitors named and profiled is about 20 credits.

## The comparison

1. **Listings side by side.** `apps_get_app` on each: name and subtitle (what term each leads with), rating and count, price model, `version` and `updated_at` (release pace), `update_notes` (what they ship), screenshots (the benefit each opens with). The table shows the claim nobody makes and the term nobody owns.
2. **Keywords side by side.** `apps_get_shared_keywords` with the user's app first, then up to 19 rivals: per term, every app's rank, sorted by volume. It returns only the terms all the apps rank for (the count matches `shared_keywords` on the competitor row: 45 for Streaks and Habitify), so mark where the user ranks below a rival it beats on rating; terms where the user is absent need each app's `apps_get_keywords` list compared ([keywords](keywords.md)).
3. **What their users dislike.** `apps_get_reviews` with `sort` newest on each rival's 1 to 3 star reviews: the complaints are the user's positioning, and a spike after a version shows a release that went wrong ([reviews](reviews.md)).
4. **Charts.** `apps_get_top_charts` in the category (1 credit per 100 rows) for `top_free`, `top_grossing` and `new_free`, with the store's category id in lower case with underscores (`health_and_fitness`, not health-fitness; games are `games_puzzle` on the App Store and `game_puzzle` on Play). The tool description lists every id, a store's category name such as "Health & Fitness" is turned into its id, and a wrong id is refused free. Play chart rows carry `developer` but no `ratings_count`; App Store rows the reverse. Top free is download velocity over recent days; top grossing is revenue; neither is quality. An app rising in `new_free` is the one to watch. App Store charts return at most 100 rows, Play 200. Charts move daily: one pull is a snapshot, so date it.

## Estimating a rival's size

No Manifold tool returns downloads or revenue for an app. Play's `installs` from `apps_get_app` is an exact lifetime count (7,909,650 for Loop, where the store page shows a bucket), the best size figure the tools give; it counts installs ever, not active users, and Google moved install counting from devices to accounts in June 2026, so figures across that date do not compare. The App Store gives no installs. Rating count grows with installs but depends on how hard the app prompts, so compare it only between apps of the same type. Chart position over several pulls is the closest the tools come to velocity. Say which of these an estimate rests on; never quote a third-party download estimate as a fact (they are models, as in [competitor-intel](../../competitor-intel/SKILL.md)).

## Watching rivals

A monthly pull per rival is cheap: `apps_get_app` (1), newest reviews (1), keywords (10). Changes worth reporting: a new name or subtitle (a term they are now chasing), a new first screenshot (a new message), a rating drop of 0.2 or more, a jump in `ratings_count` (a review prompt campaign or a launch), a release cadence change. The server keeps no history; the host stores each pull with its date.

## Data notes

- The keyword and competitor tools cover the US store in English only. A rival that is strong in another market will look weak here.
- `apps_get_competitors` leaves the user's own app out of the rows.
- `similar_apps[]` is what the store recommends next to the app, which is also where a visitor goes instead of installing.
