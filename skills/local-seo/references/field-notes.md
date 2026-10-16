# Field notes

What Manifold has seen in its own calls on local searches, United States, English. Small samples: read them as evidence of how the surfaces and the tools behave, not as benchmarks. Public businesses only. Newest first.

## Why the gaps are upstream (16 October 2026)

Five follow-up calls between 08:39 and 08:42 Eastern, 5 credits, to tell a fault in the tools' requests from a gap in the data.

- **`maps_get_place`.** RustyBrick, the business in DataForSEO's documented sample response (services, five related places and attributes in four groups), read live with the sample's own `location` "New York,New York,United States": `services` and `people_also_search` empty, `attributes` the two wheelchair items only. Katz's had already been read with the New York `location` rather than the country, with the same gaps. The vendor documents no parameter that picks which fields come back; location and language are the only inputs.
- **`seo_get_serp`, organic rows.** "dentist" at "United States" and at "Seattle,Washington,United States" was compared with the Columbus call below. Each pack was local (three downtown Seattle practices; Arizona, California and Colorado for the country), but no organic page in any of the three was about the searched city, and four of the nine were the same in all three: a Colorado practice's booking page, practices in Oviedo and Jacksonville, Florida, and a national directory. Every call's `check_url` carried the place in its `uule`, so Google did receive the location. "dentist columbus ohio" at Columbus returned nine Columbus-area pages (practices, a children's hospital, a directory search for a Columbus ZIP). Not tried: a GPS point instead of a location name, which `seo_get_serp` does not take.

## The Maps tools, live (16 October 2026)

"dentist" around downtown Columbus, Ohio, plus "plumber" in Austin, Texas, called on a Friday between 08:12 and 08:17 Eastern. 27 calls, 67 credits: 13 `maps_search_places` (13 credits), 3 `maps_search_businesses` (31), 10 `maps_get_place` (20), 3 `seo_get_serp` (3).

**A 3 by 3 grid, 1.5 km apart** (centre 39.9612, -82.9988, `zoom` 14, `depth` 20, 9 credits). Top three per cell:

| | West | Centre | East |
|---|---|---|---|
| North | Dennison Dental Care, Arch City Dental - Grandview, Victorian Village Dental Care | Dennison Dental Care, 4th St Dental Studio, Victorian Village Dental Care | US Dental and Medical Care, Dennison Dental Care, Victorian Village Dental Care |
| Centre | Hanger Clinic-Orthodontist, Urgent Dental Care, Capstone Dental Downtown | Capstone Dental Downtown, Trillium Dental, Capitol Square Dental | Columbus City Dental Clinic, Hoagland & Hoagland, Third Street Dental |
| South | Merion Village Dental, Gentle Family Dental Care, Third Street Dental | Third Street Dental, Rapid Dentist Finder, Merion Village Dental | US Dental and Medical Care, Third Street Dental, Merion Village Dental |

- 16 different businesses filled the 27 top-three slots across a square 3 km wide. Capstone Dental Downtown, first at the centre, was top three in 2 of 9 cells and top ten in all 9 (average rank 5.2). Merion Village Dental, with the most reviews in the set (1,118), was top three in the 3 southern cells and outside the top 20 in the 3 northern ones (average 10.7, counting 20+ as 21).
- Two of the cell leaders were thin listings: one with no reviews, phone, website or hours, unclaimed and filed under Dentist with a name that belongs to a prosthetics clinic (first in the west cell), and one called a "dentist finder" with one review, no website and no hours (second in the south cell). Thin and odd listings win cells on proximity alone; look at them before treating them as competitors.
- **Not every row is the searched trade.** At `depth` 100 around the centre (1 credit), 9 of the 100 places were not dental: a finance broker, an insurance company, an SEO agency, two government offices, a social services charity (fourth in the south-east cell), an imaging centre and two medical clinics. 41 of the 100 were unclaimed, most of them individual dentists listed at their practice's address (rated or not), often with "Practitioner service location" in `additional_categories`.
- **Noise is small at a point.** The same search 11 m from the centre (a new cache key) gave the same top three and the same set in the top 14, with two pairs swapped by one place. "dentist near me" at the centre gave the same top four as "dentist", with one new no-review listing at 5.
- `location` "Columbus,Ohio,United States" returned nearly the same list as the downtown point: a city name is one point at the centre.

**Maps and the search page's pack disagree.** For the same search, `seo_get_serp` at Columbus and at ZIP 43215 both showed Dennison Dental Care, Gentle Family Dental Care and Merion Village Dental (839 to 1,118 reviews, 2 to 4 km from downtown), while Maps at the centre and at the city showed Capstone, Trillium and Capitol Square: none of the three in common. The pack the day before (below) had Capstone in it at both locations, so the pack itself moved overnight. For "plumber" in Austin, the pack (sponsored entry dropped) was Maps' second, third and eighth places; Maps' first was a "Plumber" listing named after a national bank, unclaimed, linking to the bank's website, which the pack left out.

**Organic rows were not localised at city level either.** "dentist" at Columbus returned organic pages for Kettering, Circleville, Bellevue (Kentucky), Colorado and Oviedo (Florida), the same set as at ZIP 43215; "plumber" at Austin returned Yelp pages for Lake Charles, Marrero and West Odessa and directories for Sioux Falls and Fresno. Only the pack followed `location`; with the town in the keyword the organic rows were local (see above).

**`maps_search_businesses`, 5 km around the same centre, `categories` ["dentist"]:**

- `rows_available` was 214 (5 credits for `limit` 1). The category id matched a primary or an additional category.
- `max_reviews` 20 returned 47 rows (10 credits: you pay for rows returned, not for `limit`) and not one without reviews, while the unclaimed call returned 88 rows with no reviews: the review bounds drop businesses with zero reviews. Of the 47, 4 were closed; of the other 43, about 30 were individual practitioners at a larger practice or clinics inside a hospital or the university dental school, and about 13 were standalone practices.
- `claimed` false matched 142 of the 214 (66%). Of the first 100 (16 credits): 88 had no reviews, 69 no website, 13 were closed (5 `closed_forever`, 8 `temporarily_closed`), and nearly all were named individuals rather than practices. In healthcare an unclaimed listing is usually a practitioner's, not an unclaimed business.
- **Freshness.** `updated_at` fell between 4 August and 30 September 2026 for 45 of the 47 rows and 95 of the 100 unclaimed ones, so 2 to 10 weeks before the call, none in October. The 6 distinct older rows (May 2023 to August 2024) were all `closed_forever`: an old date marked a dead listing, not a stale live one.
- **The database disagrees with live Maps on details.** Of about 20 rows also seen in live results, one was claimed with a toll-free phone in the database but unclaimed with a local phone live, another claimed in the database and unclaimed live, and three review counts differed by one or two.

**`maps_get_place` on 10 profiles** (7 Columbus dentists, 2 Austin plumbers, Katz's Delicatessen in New York by `name`, 20 credits):

- Filled when the business had them: `rating_counts` (9 of 10; null with no reviews), `hours` (7), `topics` (7, at most 10, by mentions, once including a staff member's first name), `description` (5), `book_online_url` (3).
- **Empty in all 10: `services` and `people_also_search`,** including a restaurant with 56,000 reviews and a plumbing firm with 18,000. **`attributes` held only the wheelchair accessibility items** (7 of 10; empty for both plumbers and one thin listing), even for the restaurant, and `missing_attributes` was filled once. The business in the vendor's own sample response came back the same way (see above), so the gap is upstream; do not read an empty list as a gap in the profile.
- **`status` is open or closed at the moment of the call,** by the listed hours: at 08:13 on a Friday a practice whose hours end on Thursday read "closed" and one opening at 08:00 read "open". In Maps search rows 36 of 100 had no `status`; the three such profiles read had no hours. Only the database rows carried `temporarily_closed` or `closed_forever`.
- **A search row's `category` can be the matched category, not the primary one:** a firm listed as "Plumber" in the Austin Maps results was "HVAC contractor" with Plumber as an additional category on its profile. Read the primary category from `maps_get_place`.
- **Service-area businesses come back with `address`, `city` and `postcode` null** (3 of the Austin top 10) and a point for `latitude` and `longitude`.
- `rank` ran 1 to N with no gaps in every call; no sponsored places appeared in Maps rows. `rows_available` on `maps_search_places` was 20 to 22 at `depth` 20 and 101 at `depth` 100: it counts what was fetched, not the places in town.

## The local pack, place by place (15 October 2026)

`seo_get_serp` on "dentist" and "plumber" from different locations, desktop unless noted, 12 calls, 21 credits.

**The pack changes within a few kilometres.** "Dentist" with `location` set to three Columbus, Ohio ZIP codes 2 to 4 km apart:

| Location | Pack (sponsored entry dropped) |
|---|---|
| 43215 (downtown) | Capstone Dental Downtown, Dennison Dental Care, Gentle Family Dental Care |
| 43206 (south) | Southmoor Dental Group, Merion Village Dental, US Dental and Medical Care |
| 43201 (north) | Dennison Dental Care, 4th St Dental Studio, Grandview Dental Care |
| Columbus (city) | Capstone Dental Downtown, Gentle Family Dental Care, Merion Village Dental |

Eight different practices filled the nine ZIP slots; only one appeared in two ZIPs. The city-level pack matched none of the three ZIP packs exactly. The suburbs Upper Arlington (about 8 km out) and Westerville (about 25 km) each showed three practices that appear in no Columbus pack. "Plumber" in Austin and in Round Rock (about 30 km) shared none. A rank check at the city level is one more point, not an average.

**Implicit and explicit location differ.** "dentist" searched from Columbus and "dentist columbus ohio" searched from the United States shared one of three pack places.

**Other things the pack showed:**

- Desktop and mobile returned the same pack for "dentist" in Columbus.
- "plumber" and "plumber near me" in Austin returned the same top three; "near me" added a fourth, sponsored entry.
- One Austin plumbing listing in the pack had a website field pointing at a national bank's domain, the kind of mismatch that marks a listing to check before treating it as a competitor.
- Pack rows carry `rating` and a `snippet` with the address, hours, years in business and a review quote, but no review count.

**AI Overviews stayed off.** None of the 9 local searches run with `ai_overview: true` showed an overview except "how much does a plumber cost to fix a leaking pipe in austin", whose overview cited Angi, two cost directories and an Austin plumber's own published price guide.

**Data notes for this tool:**

- Sponsored pack entries come back as `type` local_pack rows (twice in 12 calls), with "Sponsored" at the start of the `snippet`, a title ending "My Ad Center", and the `url` null or a google.com ad link. Drop them before counting the pack: unlike the `type` field of `seo_get_ranked_keywords`, which excludes ads, `seo_get_serp` keeps them.
- At ZIP and suburb `location` values the pack localised, but the organic results did not: the same directory pages for Kettering, Ohio, Racine, Ohio, Bellevue, Kentucky and Oviedo, Florida came back for Upper Arlington, Westerville and all three ZIPs. Read only the pack at that level. A city-level location did no better the next day (see 16 October above).

## AI answers for local prompts (15 October 2026)

`aeo_run_ai_answers` on "dentist near me" and "best plumber in Austin TX", on chatgpt, gemini, ai_mode and ai_overview, four calls, 26 credits. Models: gpt-6 (ChatGPT app), Gemini 3.5 Flash-Lite (app).

**"Near me" needs a local `location`.** With `location` "United States", ChatGPT answered for Quincy, Massachusetts ("based on your estimated location"), Gemini for dentists in rural Colorado, and AI Mode with a mix from Colorado, California, Kansas and Idaho. With `location` "Columbus,Ohio,United States", Gemini and AI Mode named downtown Columbus practices, three of them in that location's pack. With ZIP 43206, Gemini named all three practices in that ZIP's pack plus two more within about 3 miles, giving distances; AI Mode named two of the three plus two practices with 1,800 and 1,900 reviews a few miles away.

**A city `location` failed on ChatGPT.** Two calls with "Columbus,Ohio,United States" returned no ChatGPT answer, with "rate limited or down" as the error, while ChatGPT answered normally with "United States" in the same minute. Gemini and AI Mode accepted city and ZIP locations. Not charged.

**AI picks are not the pack.** For "best plumber in Austin TX" none of the three engines named any of the three plumbers in Google's Austin pack. AI Mode named five businesses with 1,700 to 18,000 reviews; Gemini named two; ChatGPT named three and cited best-of list sites (Expertise, Best Pick Reports, a local awards site), retrieving 12 list and directory pages. One company appeared on all three engines.

**Sources:**

- Gemini cited only Google Maps listings: 16 of 16 citations across the four answers.
- AI Mode's `citations[]` were Google search viewer links (domain google.com), one to five an answer, not the underlying pages. Count AI Mode by the names in the answer.
- ChatGPT's retrieved pages for "dentist near me" were directories (Zocdoc, Healthgrades, Opencare, WebMD, Vitals, Yellow Pages, Birdeye). Neither yelp.com nor facebook.com was cited or named in any answer.
- AI Overview returned no answer on all three of its cells.

Earlier local runs on Facebook, Yelp and Reddit in Google and AI answers are in the [facebook](../../facebook/SKILL.md) skill.
