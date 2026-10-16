# Local prospect lists

For an agency or freelancer selling to local businesses: a list of businesses in an area with a visible gap the agency can fix (no website, few reviews, an unclaimed profile, a weak Maps position), and the evidence to open with. Checked October 2026. The audit sent to one prospect and the pitch belong to [agency](../../agency/SKILL.md); finding the owner's email and the rules for contacting them belong to [outbound](../../outbound/SKILL.md).

## Pick the gap first

The filter is the offer. Match it to what the agency sells:

| Agency sells | Filter on | Why it is a lead |
|---|---|---|
| Websites | `website` null, or a website on a social or booking domain | The profile lists no site of its own |
| Review programmes | `max_reviews` (20 is the line 47% of consumers will not cross) or `max_rating` 4.2; the bounds drop businesses with no reviews, so run a second search without them to catch those | Below what buyers accept |
| Profile management | `claimed` false, then drop individual practitioners | Anyone can edit it and the owner cannot reply to reviews |
| Local SEO | Weak grid position against a review count that should win | Shows a fixable gap, not just a small business |

## Building the list

1. **Centre and radius.** `maps_search_businesses` takes `latitude`, `longitude` and `radius_km` (5 by default). Use the town centre and a radius that matches the agency's reach.
2. **Category ids.** `categories` takes Google's category ids in snake case ("dentist", "plumber", "hair_salon", "pizza_restaurant"), up to 10, and matches a primary or an additional category ("dentist" also returned cosmetic dentists, pediatric dentists and a doctor with Dentist as a second category). An id that is not Google's exact one can come back empty; if it does, try the plainer id, or run without `categories` and read `category` on the rows.
3. **Filters.** `claimed`, `min_rating` and `max_rating`, `min_reviews` and `max_reviews`, `name` (text in the name, for a brand or chain), `order_by` rating or reviews. A review or rating bound silently excludes listings with no reviews: around downtown Columbus `max_reviews` 20 returned 47 dentists, none with zero reviews, while the unclaimed search alone found 88 with none ([field notes](field-notes.md#the-maps-tools-live-16-october-2026)).
4. **Size before paying.** The call costs 4 credits plus 12 per 100 rows returned (16 for a full 100; 47 rows cost 10). The estimate that `max_credits` is checked against assumes a full `limit`; the charge follows what comes back. Run it with `limit` 1 first (5 credits): `rows_available` gives the total, and paging is by `cursor`. Results are cached 7 days.
5. **Confirm the shortlist.** The rows come from a listings database, not live Maps: `updated_at` says when the vendor last refreshed each row. In our test almost every row was 2 to 10 weeks old, and the few dated 2023 or 2024 were all marked `closed_forever`; drop those. Even fresh rows can disagree with live Maps: one dentist was claimed with a toll-free phone in the database and unclaimed with a local phone live. Before outreach, confirm each shortlisted business with `maps_get_place` (2 credits): still trading, still without a site, still unclaimed. A business that closed or built a site since is a wasted and embarrassing email.

## Reading the rows

- **`website` null means the profile lists no website,** not that the business has none: some have a site they never linked. Check by searching the name and town (`seo_get_serp`, 1 credit) before pitching "you have no website". A website on facebook.com, instagram.com or a booking platform is a stronger lead than null: the owner wants a web presence and has settled for a page they do not own.
- **`is_claimed` false** is an unverified profile; it may be a new business, a closed one, one whose owner never claimed it, or, very often in healthcare and law, an individual practitioner's listing at a practice that is itself claimed. Of 100 unclaimed dentist rows around downtown Columbus, nearly all were named individuals, 88 had no reviews and 13 were closed: a name that is a person, an address shared with a claimed practice, a website on the practice's or a hospital's domain, or "Practitioner service location" in `additional_categories` marks one. Pitch the practice, not the practitioner.
- **`status` is not whether the business trades.** "open" and "closed" are its hours at the moment the data was read; only `temporarily_closed` and `closed_forever` mean what they say, and many rows have no `status` at all.
- **`reviews` and `rating`** are a snapshot; a business with 15 reviews and a burst of new ones is not the lead it looks like. `maps_get_place` gives `rating_counts`; review dates need a look at the profile.
- **Chains and franchises** fill category searches. Drop rows whose `name` or `domain` matches a national brand: their marketing is bought centrally.
- **Use the gap as the opener.** "Your profile lists no website and the three dentists above you in Maps for 'dentist' near Short North all link one" is checkable in a minute; "your SEO is weak" is not. A two-point grid (the business's address and the town centre, 2 credits) adds a ranking fact.

## Contact and conduct

- Phone, website and address on the rows are business contact details the business published. The owner's email is found from the website's domain (`leads_get_domain_emails`), with the verification rules in [outbound](../../outbound/SKILL.md); many sole traders are individuals under UK and EU law, which changes what consent cold email needs.
- Never send, call or message from here. Deliver the list and the draft openers; the agency sends.
- Never pitch a fix that breaks Google's rules (keywords in the name, a virtual office address, review gating or bought reviews), even when a prospect asks for it.

## Without the connector

Search the category in Google Maps around the town, open each listing, and note the name, website (or none), review count, rating and whether the profile shows "Own this business?" (unclaimed). It is slow; twenty businesses take about an hour.
