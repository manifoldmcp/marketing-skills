# The Google Business Profile

How to audit a profile, what changed in 2025 and 2026, and the rules that get profiles suspended. Checked October 2026. Google rarely announces Business Profile changes; most dates below come from practitioners who spotted them (Sterling Sky's change log, Search Engine Roundtable, Google's Product Experts). Re-check anything here before telling a client a feature exists.

## What changed

| When | Change | What it means |
|---|---|---|
| June 2025 | Countries and states banned as service areas; 20 areas, about two hours' drive, no radius | Clean up oversized service areas |
| 3 November 2025 | Q&A API retired; the Q&A section came off profiles from December 2025 | Old Q&A content is frozen or gone; nothing to seed |
| March 2026 | Ask Maps (Gemini) in Google Maps, US and India, mobile first | Answers about the place come from the listing, reviews and website; owners cannot write or correct them |
| April 2026 | Review policy bars staff review quotas and asking for reviews that name staff | Retire staff contests built on named reviews ([reviews](reviews.md#what-is-allowed)) |
| April 2026 | Google's 2025 spam report: 292 million reviews and 13 million fake profiles removed; a spike of spam reviews pauses new reviews on the profile | A sudden burst of reviews can freeze a profile |
| September 2026 | Owners have four days to reject a user-suggested edit; email alerts for detected spam review spikes; automatic hours in testing; reading all reviews on Maps needs a signed-in account | Check the profile weekly; check edits by hand when signed in |
| September 2026 | The local knowledge panel turning into an AI Overview style listing, in testing | What the panel says may be written by Google, not the owner |
| October 2026 | WhatsApp buttons in the local panel; dietary attributes for restaurants | New fields to fill where they apply |

Business chat (messaging on the profile) ended in July 2024; do not plan around it.

## The audit

Read the profile with `maps_get_place` (2 credits) by `place_id`, `cid` (the `cid=` in a Maps link) or `name` with the town, and the same for the top three competitors from the [rank grid](rank-grid.md). Compare, field by field. Three rows need the profile itself, read by hand: in 10 live profiles in October 2026 (dentists, plumbers, a restaurant) `services` and `people_also_search` were always empty and `attributes` held only wheelchair access items, a gap upstream of Manifold, not in the profiles ([field notes](field-notes.md#the-maps-tools-live-16-october-2026)). Never tell a client their services or attributes are missing on that evidence.

| Check | Field | What good looks like |
|---|---|---|
| Claimed | `is_claimed` | true. An unclaimed profile can be edited by anyone and cannot reply to reviews |
| Status | `status` | Not temporarily_closed or closed_forever, among the worst negatives. "open" and "closed" only mean open or shut at the moment of the call, by the listed hours; null means no hours |
| Primary category | `category` | The term most customers search; the same as or more specific than the top competitors'. Read it here, not from a Maps search row, which can show the category that matched the search instead |
| Secondary categories | `additional_categories` | Only services actually offered; compare with the leaders' |
| Name | `name` | The real-world name; no added city or service words |
| Hours | `hours` | Set for every open day, holiday hours current; a day with no row is read as closed |
| Services | On the profile, by hand | Google's predefined services for the category filled, plus custom ones in customers' words, with prices where the trade publishes them |
| Attributes | `attributes` for accessibility; the rest on the profile, by hand | Every true attribute set |
| Description | `description` | What the business does and where, in plain words; no links or offers |
| Website | `website`, `domain` | The location's own page (not the homepage for a chain), working, with the same name, address and phone |
| Photos | `total_photos` | Real, recent photos of the place, team and work, against the competitors' counts |
| Reviews | `rating`, `reviews`, `rating_counts` | Count and recency against the competitors; see [reviews](reviews.md) |
| Review topics | `topics` | The 10 themes customers mention most; topics missing that the business wants known are prompts for review requests |
| Booking | `book_online_url` | Present where the trade books online |

`maps_get_place` does not return review text, review dates, owner replies, posts, products or the edit history. Read those on the profile by hand (signed in, since September 2026) or in the owner's dashboard. A profile with no Google record returns NoData: the business may be unlisted, merged into another listing or filtered.

**Filters and duplicates.** Two profiles in the same category at the same address (a practice and one of its practitioners, two brands of one owner) are filtered: one disappears from the pack. Practitioner listings are common: around downtown Columbus most unclaimed dentist listings were individual dentists at a practice's address, several ranking in Maps' top 20 beside their own practice. Look for duplicates by searching the name and phone with `maps_search_places`; merging or removing duplicates is done in the owner's dashboard or through Google support.

## Service-area businesses

- A business that serves customers at their premises (plumbers, cleaners, mobile groomers) and does not receive them at its address must hide the address and list service areas: up to 20 cities, postcodes or counties, within about two hours' drive, never a country or state ([Google's help](https://support.google.com/business/answer/10514137)). A hybrid business with a staffed storefront may show its address and list service areas.
- **Rankings follow the verified address, hidden or not.** Hiding the address cut rankings and calls in Sterling Sky's test and re-adding it restored them about a month later (one home-service client, run 2023, published September 2026); when a service-area business moved towns, its rankings stayed at the old verified address until the new address was shown (Whitespark, one business, October 2024). Neither tested the service-area list itself; the practitioner consensus is that it does not affect rankings. In Manifold's rows a service-area business has `address`, `city` and `postcode` null but still a `latitude` and `longitude`: Google keeps a point for it, and the grid ranks it from there (three of Austin's Maps top ten for "plumber" in October 2026).
- **So a service-area business ranks near its base.** To win a far town it needs a real staffed location there (with signage, verifiable), or organic rankings from a page about that town on the site ([seo](../../seo/SKILL.md)). Virtual offices and mail drops are the top suspension risk.
- A home address shown on a service-area profile, or a storefront switched to service-area without hiding the address, were the most-cited triggers in practitioner reports of the April 2026 suspension wave on home-service profiles. Google confirmed no wave; treat it as anecdote, and still fix the mismatch.

## Verification and re-verification

- Video is now the common route: one continuous, unedited clip recorded in the app, showing the location (street signs, the building, permanent signage), proof of management (unlocking the door, the till, a staff area), and for a service-area business the tools and branded vehicle instead of signage. Practitioner guides put review at a few days.
- **Edits that tend to trigger re-verification:** the name, the primary or additional categories, the address, and switching between storefront and service-area (SOCi, March 2025, after Google fixed a bug that had caused a spike of re-verifications). Make one such edit at a time and only when it is true.
- A suspended profile: read the notice for the type (a soft suspension leaves the profile unverified; a hard one removes it), fix the cause first, then appeal once through Google's appeal form with evidence (a business licence, a utility bill, photos of signage). Practitioners advise against editing a hard-suspended profile while the appeal runs. Reinstatement is the owner's to file; never submit an appeal or edit on their behalf.

## Multi-location businesses

- One profile per real location, each linked to its own location page, with its own hours and phone. Overlapping service areas between a chain's profiles are a negative and a suspension risk in the survey.
- Audit the chain from `maps_search_businesses` with `name` set to the brand around each market (4 credits plus 12 per 100 rows returned) to find missing, duplicate, closed or unclaimed locations, then `maps_get_place` on the outliers. The listings run weeks behind live Maps, and a `closed_forever` duplicate can linger there for years (one was dated January 2024), so confirm each finding live.

## Without the connector

Open the profile in Google Maps, signed in, and fill the audit table by hand from what the listing shows; do the same for the three businesses in the pack for the main search. Note the date. The owner's dashboard shows what a visitor cannot: pending edits, verification status and performance.
