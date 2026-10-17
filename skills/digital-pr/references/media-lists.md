# Media lists

A media list for one story: the reporters who already cover it, the article that proves each one's beat, and an address that still works. The list is built around a story and a beat, never around a masthead. Checked October 2026.

## What makes a list good

- **Every row carries evidence.** The reporter's own article on the topic, from the past year, with its title and URL. A row with no article goes to the bottom or off the list.
- **Small.** Ten to twenty-five reporters for one story. BuzzStream's 31M-email analysis found campaigns sent to 51 to 100 people earned the most links per campaign and those sent to over 1,000 the fewest (see [stories](stories.md#benchmarks)).
- **Current.** Reporters move often, and media databases lag. Outdated or wrong journalist data is the top complaint practitioners have about their media-list tools (61% in BuzzStream's 2025 survey, 63% in 2026). Check that the reporter still writes for the outlet before the row stays: a byline in the past three months, or their own profile.
- **Tiered by fit, not by fame.** A trade publication that covers the exact beat beats a national outlet whose reporter wrote one related story. Relevance beats domain rank here.

## Where the reporters come from

1. **Coverage competitors earned.** `seo_get_backlinks` on each competitor's domain with `limit: 500` (10 credits plus 1.5 per 100 rows, about 18). Keep rows whose `domain_from` is a publication and whose `page_title` reads as an article, not a directory, a tool list or a press-release wire. Each is a story that already named a rival; note what earned it (data, funding, a founder's view, a free tool). The link index misses coverage without a link, so this finds a subset.
2. **Recent coverage of the topic.** `seo_get_serp` with `depth: 20` (2 credits) on three to five headline-style queries, each with `after:` and the date a year ago (Google's operators pass through). Without the date filter page one is evergreen statistics pages. Use the words headlines use ("<topic> study finds", "<topic> report", "<topic> survey shows", "<topic> data"): in our test a plain "<topic> survey" query returned only vendor and tool pages ([field notes](field-notes.md)). Keep the publication results; `published_at` dates them.
3. **Rate the publications in one call.** `seo_get_domain_ratings` with every publication domain (7 credits plus 2.5 per 100). Use the floors in [link-building](../../link-building/SKILL.md) as a guide, but keep a small trade outlet that covers the exact beat.
4. **Find the person.** The article's byline is the best source: if the host can open the URL, read it. Otherwise `leads_get_domain_emails` with `department: ["communication"]` (editors and writers at publications sit there), 31 credits a domain that returns addresses, whatever the `limit`. For a named reporter not in the domain results, `leads_get_email` with the name and the outlet's domain (7 credits on a hit, 1 on none). For a beat with no reporter found, `linkedin_search_posts` on the topic (1 credit a page) shows who posts about it, and `linkedin_get_profile` (1 credit) shows their headline.
5. **Verify only what will be sent.** `leads_get_email_status` on one address per reporter (1 credit). The full contact steps and how to read each status live in [link-building](../../link-building/SKILL.md); personal-data rules in [outbound](../../outbound/SKILL.md).

A default list of 15 publications and 10 verified reporters costs about 2 × 18 + 5 × 2 + 10 + 15 × 31 + 10 = 531 credits. Say the estimate first, and pass `max_credits` when the user gives a budget.

## Reading the contact data

From our own pulls (see [field notes](field-notes.md)):

- **At a large outlet, most rows can be stale.** Of ten editorial rows `leads_get_domain_emails` returned for TechCrunch, only three had a source on the outlet's own site seen this year (its About page); the rest came from scraped "tech journalist email" lists, some from 2014, and several named reporters known to have left years ago. Confidence was 82 to 85 on every row, so confidence says nothing about whether the person is still there.
- **Read `sources[]`, not `confidence`.** Keep a row when a source is the outlet's own page or a byline with a recent `last_seen_at`. A row whose only source is a third-party list, or a search-results URL, needs the byline check before it stays.
- **`accept_all` on the domain means no address there can be proven.** All ten TechCrunch addresses were `accept_all`. Mark them unproven; verification cannot settle them, and the pattern (`domains[].pattern`) is a guess until a reply comes back.
- **Trade and niche outlets often return only generic or commercial addresses.** MarTech returned only `newsletters@` and `promotions@`, neither of which reaches an editor; SaaStr returned nothing in communication. For these, the byline, the outlet's contact or tips page, or the editor's LinkedIn is the route.
- **A generic `press@` at a national outlet goes nowhere; at a small blog it is often the editor.** Keep generic addresses only for small publications.
- **Syndicated copies of one article** (the same title on many domains) are one story: keep the original publisher.

## The deliverable

A table: reporter, publication, role, the article that shows the beat (title, URL, date), domain rank, email, verification status, where the contact came from, and the angle (one line tying the user's story to that article). Sort by fit. Nothing is sent from here; the user, or an email tool the host has, sends.

## Without the connector

The method works by hand. Search Google News for the topic and the competitors' names over the past year, open each article, note the byline, the outlet and the date, and check the reporter's author page on the outlet for a byline in the past three months. Find the address on the author page, the outlet's staff or contact page, or the reporter's own site or social bio; many reporters publish a preferred address. Say plainly that the list was built by hand and that addresses are unverified.
