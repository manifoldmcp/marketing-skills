# Local rank grid

Where a business ranks on Maps across its town, measured as a grid of points, and how to track it without mistaking noise for change. Checked October 2026. Manifold has no grid tool on purpose: a grid is one `maps_search_places` call per point, composed here.

## Why a grid

Maps ranks from the searcher's position, and the pack can change completely within a few kilometres (see [ranking factors](ranking-factors.md#proximity)). A single check from the office, or from the city centre, flatters the business near its address and hides where it disappears. Rank trackers that report one number per keyword for a city are reporting one point.

## Building one

1. **Pick one to three searches** the way customers type them inside the town ("emergency plumber", "dentist", "nail salon"), without the town name: the grid supplies the location. Add a "service in town" search separately with `location` if out-of-town searchers matter.
2. **Lay out the points** around the business's address (or the centre of the service area): a 5 by 5 grid is the usual size. Spacing sets the question: about 1 km apart in a dense city, 2 to 3 km in a suburb, 5 km or more for a rural service area. In our downtown Columbus test at 1.5 km, the practice first at the centre stayed in the top 10 in every cell but held the top 3 in only 2 of 9, so even 1.5 km shows the edge of the ring in a dense city ([field notes](field-notes.md#the-maps-tools-live-16-october-2026)). Roughly, one degree of latitude is 111 km, and one degree of longitude is 111 km times the cosine of the latitude (about 70 km at 51° north, 89 km at 37° north).
3. **Call `maps_search_places` once per point** with `keyword`, `latitude`, `longitude` and `zoom` (14 for a neighbourhood, 13 for a wider suburb grid) and `depth` 20. It costs 1 credit whatever the depth (checked at 10, 20 and 100), so a 5 by 5 grid is 25 credits per search, and a 7 by 7 grid 49; ask for `depth` 100 at the cells where the business falls outside the top 20, since the price is the same and only the reply is longer. Results are cached 24 hours by keyword, point, zoom and depth: a re-run the same day is free and identical, so it shows nothing new. `rank` counts places only; ads take no number.
4. **Find the business in each point's `rows`** by `place_id` or `cid` (from `maps_get_place`), never by name: chains and similar names collide. Not found in the top 20 counts as 21 for averaging, shown as "20+". Keep the rank as Maps gives it, but when listing who beats the business, skip rows in other trades (9 of 100 in our test, from a finance broker to a charity) and individual practitioners listed at a rival's address.

## Reading it

- **Report three numbers per search:** the share of points where the business is in the top 3 (the pack), the share in the top 10, and the average rank with "20+" counted as 21. The top-3 share is the one that maps to calls.
- **Draw it as a grid** of rank numbers, coloured by band (1 to 3, 4 to 10, 11 to 20, 20+), with the address marked. The pattern says what to do: strong at the address and fading evenly is proximity (normal); weak even at the address is relevance or prominence (category, reviews, a filter or a spam competitor); strong on one side only usually follows where competitors sit.
- **For each weak cell, note who holds the top 3** there. The same two or three rivals across a district are the real competitors to study ([competitors](competitors.md)).
- **The Maps app and the pack on Google search differ.** `maps_search_places` reads Google Maps; the three-place pack on a search results page is a separate selection. For "dentist" in Columbus the pack, at the city and at the downtown ZIP, held three high-review practices 2 to 4 km out while Maps at the same centre held the three nearest: none in common. For "plumber" in Austin they shared two of three. The pack also moved overnight at both locations. Grid with Maps; check the pack separately with `seo_get_serp`, `location` set to the town or a ZIP or postcode ("43215,Ohio,United States"), 1 credit; drop sponsored pack rows (`snippet` starting "Sponsored", often no `url`), and do not read its organic rows for a bare local search: at the United States, Columbus and Seattle none was about the searched city. Put the town in the keyword for local organic results ([field notes](field-notes.md)).

## Tracking over time

- **Re-run the same grid, same points, same searches, every two to four weeks**, and monthly once stable. The server keeps no history: store each run in the host with the date, the points, the searches and every row's rank and `place_id`. If the host has a scheduler, set the run there; if not, say so.
- **Single-cell moves of one or two places are noise.** Practitioners and grid vendors report that the same scan minutes apart can differ slightly; in our test a point 11 m from another gave the same top three and the same top 14, with two pairs swapped by one place. Report a change only when the top-3 share or the average moves across several cells in the same direction, and on two runs in a row.
- **Annotate known events** next to the run: a category change, a review push, a new competitor, a Google update. A grid that moved after a category change two weeks earlier is the evidence the change worked; nothing else is.
- **Cost of a programme:** three searches on a 5 by 5 grid every two weeks is about 150 credits a month. Cut points before searches: a 3 by 3 grid (9 credits) shows the ring; 5 by 5 shows its edge.

## Without the connector

Search on a phone from several places in town (or ask staff or customers in different districts to send screenshots), logged out or in a private window, and note the top three at each place with the date. A browser's location override can stand in for travel. Say plainly that five hand checks are a sketch, not a grid.
