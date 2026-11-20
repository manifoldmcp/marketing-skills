# Local competitors

Who the business really competes with on Maps and in local AI answers, and how to compare against them. Checked October 2026. The local competitor is whoever holds the pack where the customers are, which is often not the rival the owner names.

## Finding them

- **From the grid:** the businesses that hold the top 3 in the cells where the user is weak ([rank grid](rank-grid.md)). Count how many cells each holds; the two or three that recur are the set.
- **From Google itself:** the "People also search for" list beside the profile is Google's view of substitutes, useful as a cross-check. `maps_get_place` has a `people_also_search` field for it, but it came back empty on all 10 profiles read in October 2026 ([field notes](field-notes.md#the-maps-tools-live-16-october-2026)); read the list on the profile by hand until it fills.
- **From AI answers:** the businesses ChatGPT, Gemini and AI Mode name for the main searches ([AI answers](ai-answers.md)). They are often bigger, high-review businesses that do not hold the pack near the user's address.
- **From the owner:** ask, then check: an owner's named rival may not appear anywhere the customers look.
- For a count of every business in the category in town, `maps_search_businesses` with `categories` and a radius (5 credits with `limit` 1 for the total in `rows_available`). It is unranked; use it to size the field, not to pick the rivals. The total counts individual practitioners' listings too: 214 "dentist" listings within 5 km of downtown Columbus, two thirds of them unclaimed and mostly named dentists inside other practices.

## Comparing

Read each competitor with `maps_get_place` (2 credits each) and set them side by side with the user:

| Row | From | Why it matters |
|---|---|---|
| Grid top-3 share | Rank grid | The outcome |
| Primary and additional categories | `category`, `additional_categories` from `maps_get_place` | The top relevance factor; a rival in a more specific category often wins. A Maps search row can show the matched category instead of the primary |
| Distance to the weak cells | `latitude`, `longitude` | Proximity explains much of a rival's lead; it cannot be copied |
| Reviews and rating | `reviews`, `rating`, `rating_counts` | Count and recency drive prominence; the spread of 1 to 5 stars shows risk |
| Review topics | `topics` | What customers praise each for; a gap names a service to ask reviews about |
| Services and attributes | On each profile, by hand (`services` and most `attributes` come back empty) | Completeness the user can match in an afternoon |
| Photos | `total_photos` | A completeness and conversion signal |
| Hours | `hours` | Open at the time of search is a top-five factor; late or weekend hours win those searches |
| Website and its page | `website`, then `seo_get_page` (free) | Title, headings and city relevance of the page the profile links to |
| Name | `name` | Keywords added to the name (see below) |

Review velocity matters more than the total, and no Manifold tool returns review dates: estimate it by reading the newest reviews on each profile by hand, or by re-reading `reviews` a month apart and taking the difference.

**What to conclude.** Rank the gaps by what the user can change: category, services, attributes, hours and the linked page this week; reviews over months; distance never. Say when a rival's lead is mostly proximity, so the owner stops chasing it with tactics.

## Spam in the pack

Fake listings, keyword-stuffed names and lead-generation profiles with no real premises are common in home services, legal, locksmiths and towing.

- **Signs:** a name that is a service plus a city ("Best Plumber Austin 24/7") or a referral service ("dentist finder") that does not match the website or signage; a listing with no phone, website, hours or reviews in a category it does not belong to; a residential or virtual office address on a storefront category; many near-identical profiles with call-tracking numbers; a website field pointing to an unrelated domain (in October 2026 the first place on Maps for "plumber" in Austin was an unclaimed "Plumber" listing named after a national bank and linking to the bank's site; the search page's pack left it out); a burst of five-star reviews from accounts with one review each.
- **The remedy is a report, not a copy.** Anyone can suggest an edit to the name on Maps; a business harmed by a competitor's misrepresentation can file Google's Business Redressal Complaint Form with evidence (photos of the real signage, the business registration). Removing a spammer above the user can lift the user without any change to their own profile. Filing is the user's decision and their action; prepare the evidence, never file it for them, and be sure: a mistaken report against a real business is a real harm.
- **Do not imitate it.** Keywords in the name rank, but a name that does not match the real-world name is a suspension risk, and edits to the name can trigger re-verification ([Business Profile](business-profile.md#verification-and-re-verification)).

## Without the connector

Search the main terms on Maps from three or four places in town and note who holds the top three at each. Open each profile and fill the comparison table by hand, dating it.
