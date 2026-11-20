# Reviews and ratings

Mining app reviews for what users love and hate, replying to them, and raising the rating within each store's rules. Checked October 2026.

## Why it matters more in 2026

- **Both stores now summarise reviews with AI**, above the individual reviews: Apple from April 2025 (US English first, refreshed at least weekly, positives then negatives), Google Play's "Users are saying" from late October 2025 on apps with enough reviews. A complaint repeated across many reviews ends up in the paragraph every visitor reads first. Developers can report an Apple summary through App Store Connect; neither can edit one.
- **Ask Play answers questions from reviews and the listing**, and Play reviews became searchable in June 2026 (AppTweak news log).
- Apple says ratings and reviews "can influence how your app ranks"; on Play, ratings and quality are widely held to weigh in. Either way the rating is the trust signal beside the icon in every search result.

## Mining

- `apps_get_reviews` (1 credit per 100, cached 6 hours) with `sort` newest for what changed recently, or most_relevant for the store's own pick of the useful ones. The App Store returns at most 500 per call; Play up to 1,000. Each row has `rating`, `text`, `app_version`, `published_at`; `title` is App Store only, `helpful_count` and `developer_reply` are Google Play only.
- **How far 100 reviews reach depends on the store and the app** (October 2026 test, [field notes](field-notes.md)): the newest 100 of Streaks on the App Store (27,000 ratings) went back 16 months; of Loop Habit Tracker on Play (63,000 ratings) 35 days. Play's most_relevant 100 for Habitica spanned 2018 to 2026, with only 5 from 2026: right for lasting themes, wrong for "what changed". Size the pull to the period asked about.
- **What the rows hold.** `app_version` is the version the reviewer had, so old versions turn up in new reviews; on Play it was empty on 16 of 100. Play `helpful_count` is empty, not 0, when nobody voted. `developer_reply` is the reply text only, without its date. App Store `published_at` is a date; Play's has the time. US Play reviews include other languages even with `language` en. `ratings_count` is the app's star ratings, most without text, not the reviews you can fetch; `rows_available` is set only when a call reached the end of what can be fetched (a short page, or 500 on the App Store), and null otherwise.
- **No reply data on the App Store.** App Store rows never carry `developer_reply`, so an iOS reply rate cannot be read with Manifold; on Play, Habitica had replied to 84 of its 100 most relevant reviews.
- **Set `location`.** Reviews are per storefront; the US reviews of an app popular in Brazil are a small, different sample.
- **Group by `app_version`.** A cluster of 1-star reviews on one version is a release problem, fixed by a release, not by copy. A complaint spread across versions is a product or expectation problem: the listing may be promising something the app does not do.
- **Code themes, then count people.** Group complaints and praise into themes, count distinct authors per theme, keep two or three verbatim quotes each. How often a theme is mentioned does not say how much it matters, and ratings are polarised (happy users rarely write); the method and its traps are in [voice-of-customer](../../voice-of-customer/SKILL.md).
- **For positioning, mine the competitors.** Their 1 to 3 star reviews say what the category fails at; their 5-star reviews say what people pay for. Read both for the user's top three rivals ([competitors](competitors.md)).
- Praise in users' own words is the best source of screenshot captions and subtitle phrasing. Paraphrase in public copy; quoting a named reviewer in metadata is unattributed testimony on Play and risky on both.

## Replying

- Google said in May 2019 that users who got a developer reply raised their rating by 0.7 stars on average; an academic study of 2,328 top free Play apps (Hassan et al., Empirical Software Engineering, 2018) also found replies can lead to higher ratings, mostly when the reply solved the problem. Neither is a promise per review.
- Across 92.9 million reviews of 20,700 apps on both stores (AppFollow, July 2025 to July 2026), only 24.5% of reviews got a reply, ranging from 9% in Social to 51% in Finance, and median reply time ran from about 43 to 204 hours by industry. A reply within a day already beats most of the market.
- **Which to answer first:** recent 1 and 2 star reviews describing a bug that is fixed (say which version), reviews with a question, and reviews with high `helpful_count` on Play (seen most). Then thoughtful positive ones.
- **How:** specific to the review, short, no defensiveness, a fix or a next step, no marketing, never the reviewer's personal details. On Play a reviewer is notified and can update; on the App Store the reviewer is notified and can edit. A templated reply pasted under fifty reviews reads worse than none.
- **Draft, never post.** Manifold reads reviews; it cannot reply. The user posts from App Store Connect or Play Console. Show each draft beside the review it answers.

## Raising the rating, within the rules

- **Apple:** use the system prompt (`requestReview` in StoreKit); custom review prompts are disallowed (guideline 5.6.1). The system shows it at most three times in 365 days to someone who has not rated, and does not show it at all in TestFlight. Do not call it from a button, since it may not appear. Forcing a rating to unlock anything is banned (3.2.2), and manipulating reviews can mean expulsion from the developer programme. Prompting on first launch or in onboarding was reportedly rejected in 2026.
- **Google Play:** use the In-App Review API. Ask no question before or during the prompt, not "Do you like the app?" and not "Would you rate us 5 stars?"; Google's quota limits how often it shows (calls less than about a month apart may show nothing). Any reward for a rating is an incentivised rating, and Google acts on both the app and the rating.
- **So "ask happy users first, send unhappy ones to support" is out on Play** and goes against the spirit of Apple's single system prompt. What remains, and works: ask right after a success moment (a goal met, a task finished, the third session) when a rating is likely to be good anyway, and fix the top complaint theme.
- **A reset:** Apple lets a developer reset the summary rating when releasing a new version, which wipes the good with the bad; worth it only when the old rating reflects problems the new version fixed. Play's rating is already weighted to recent ratings, per country and device, so it recovers on its own after a fix.

## Hand back

Themes ranked by people affected and severity, each with quotes, the versions it clusters on, and whether it is a product, release or listing problem; for listing problems, the copy change. A reply queue of drafts in order. A rating plan that names the success moment to prompt at. The user owns every post and every product decision.
