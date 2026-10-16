# Listing audit

A check of one app's store listing against what each store indexes, what converts, and what competitors do, ending in a ranked list of changes. Checked October 2026.

## Read the listing

`apps_get_app` (1 credit, cached a day) per store, with `location` set to the market that matters: listings, ratings and screenshots differ by storefront. It returns `title`, `subtitle` (App Store only), `description`, `screenshots[]`, `rating`, `ratings_count`, `installs` (Play only, an exact lifetime count rather than the store's bucket), `version`, `updated_at` and `update_notes`. It does not return the App Store keyword field (Apple never shows it publicly), in-app events, custom product pages, preview videos or the icon. Ask the user for the keyword field and their App Store Connect or Play Console numbers; the audit is half-blind without them.

What the fields held in our October 2026 test ([field notes](field-notes.md)), and how to read them:

- **App Store `description` starts with the promotional text** when the app has one (Streaks' Halloween line sat above its description), so do not count that first paragraph as description copy.
- **App Store `subtitle` is null when the app has none.** The store prints the category in that place; the tool moves it to `category`, so a null subtitle is a missing one, the first thing to fix.
- **App Store `category` can still be null** (Streaks, whose page says Health & Fitness); read it off the page then. `genres` holds the categories the store sent. A free app's `price` is 0, and `version` is the bare number. The App Store sends no `developer_website`; open the page for the link.
- **Play rows** had `content_rating` and `size` empty on all seven we read and `version` empty on six (the vendor does not send them); `updated_at` is a date only.
- **Screenshots** on the App Store come as 512-pixel thumbnails, enough to read the order and the captions, not to judge fine detail. Play links are full size.

Run the same call on the three to five closest competitors ([competitors](competitors.md)) so every finding has a benchmark beside it.

## The checks

Rank findings by what they cost: anything that blocks indexing or breaks policy first, then conversion, then polish.

**Text (ranking):**

- **App Store name and subtitle:** both near 30 characters, the brand plus the most valuable term in the name, a converting phrase with the next term in the subtitle. No word repeated between them or with the keyword field. No competitor names, prices or "free" claims that are not specific to the field (guideline 2.3.7).
- **Keyword field** (from the user): fills close to 100 characters, no spaces after commas, no plurals of words already used, no "app", no words already in the name, subtitle or category. Terms the competitor keyword data shows are missing ([keywords](keywords.md)).
- **Play title and short description:** 30 and 80 characters used, the main term in the title, no banned claims ("#1", "Best", "Top", "New", prices, emoji, all caps).
- **Play full description:** plain sentences covering the important themes, the main terms present naturally, no keyword blocks, no anonymous testimonials. The first lines carry the value, because people and Ask Play read the top.
- **App Store description:** not indexed, but it feeds the AI tags and AI answers. Features stated in plain words, not slogans ([AI answers](ai-answers.md)).

**Creative (conversion):**

- **The first three screenshots carry the decision.** They show in App Store search results, and older scroll studies (2018 and 2019, secondhand) found few visitors swipe much further. Each opens with one benefit in a short caption, in the words people search with, at a size readable as a thumbnail.
- **Screenshots match the searches the app ranks for.** A budgeting app ranking for "bill reminder" with no bill reminder screen loses that traffic at the page.
- **Up to date:** screenshots showing an old interface, the old device frame, or features since removed. Compare `updated_at` and `update_notes` with the screenshots.
- **Video:** App Store previews autoplay muted in search for people without the app; the first seconds must work without sound.
- **Icon:** legible at the smallest size, distinct from the competitors next to it in results. Judge it beside the competitors' icons, not alone.

**Trust:**

- **Rating and count against the competitors.** Below 4.0 is where conversion falls most in the evidence available (NP Digital, 49 companies, 2024: a 4.0 app indexed at 0.83 of a 5-star app's downloads, 4.5 at 0.96; a vendor dataset, read it as direction). On Play the shown rating is weighted to recent ratings and differs by country and device, so a bad release shows quickly and recovers quickly.
- **The AI review summary** above the reviews (both stores, where eligible): read it on a device. A recurring complaint written into it is a listing problem now ([reviews](reviews.md)).
- **Release cadence:** `updated_at` older than six months next to competitors updating monthly reads as abandoned to people and to Play's quality signals.

## Benchmarks: compare like with like

Conversion figures differ by denominator, channel, store and month, and most "2026 benchmarks" online recycle older data.

| Source | Measure | Figure |
|---|---|---|
| AppFollow, US App Store, August 2026 | Search installs per unique impression, by top-level category | 2.6% (Games) to 12.4% (Food & Drink) |
| AppFollow, same | Browse installs per unique impression | 0.1% to 2.5% |
| AppTweak, US, 2025 | App Store page view to install (average) | 8.56% |
| AppTweak, US, 2025 | Google Play page view to install (average) | 16.15% |
| AppTweak, US, 2025 | App Store impression to install (average) | 3.8% |

Use them for direction only. The best benchmark is App Store Connect's own peer-group percentiles (25th, 50th, 75th) for the user's category, and Play Console's peer comparison: same metric, same market. Neither store's numbers are readable with Manifold; ask the user for them.

## Testing a change

- **App Store product page optimisation:** up to three treatments of the icon, screenshots and previews against the original, one test at a time, up to 90 days, with a chosen share of traffic. Apple labels a treatment better or worse at 90% confidence and warns when a test will likely end inconclusive. An alternate icon must ship in the binary first. Text cannot be tested this way; a name or subtitle change is a before-and-after, with everything else that changed that month confounding it.
- **Play store listing experiments:** text and graphics, with retained first-time installers (kept the app at least a day) as the recommended target, read on a 90% confidence interval. Run a full week at least, so weekdays and weekends both count.
- **Small apps rarely reach significance.** A test needs thousands of visitors per arm to detect a few points of lift. For an app with a few hundred page views a week, test bold changes (a new first screenshot concept, not a colour tweak), or make the change and watch the conversion trend, and say it is not a test.
- **Judge on value, not installs.** A screenshot that wins installs can attract people who never subscribe. Where the user can see trial starts or revenue per variant, use them.
- **Custom product pages and custom store listings** are for matching a page to an intent (a keyword cluster, an ad, a country), not a test of the default page.

## Hand back

A table: finding, evidence (the field, the competitor beside it, the number), the fix written out (new subtitle, new first caption), effort, and expected effect (ranking, conversion or trust). Then the three changes to ship in the next release. Draft the copy; never submit to App Store Connect or Play Console on the user's behalf.
