# Mapping a market

The companies that serve a market, grouped by segment: who serves which buyer, how crowded each segment is, who advertises there, which brands AI engines name, and where a segment has few players. It maps vendors, not buyers. The user's own direct competitors, and any one of them in depth, belong to [competitor-intel](../../competitor-intel/SKILL.md).

## Segment by the buyer

Cut the market the way buyers search and ask: by who they are ("<category> for dentists", "<category> for enterprise"), not by vendors' feature lists. Default to three or four segments by buyer size, vertical or use case, and write each as two or three phrases a vendor in it would use about itself ("crm for real estate agents", "real estate crm"). Those phrases drive every source below.

## Four sources, because each misses players

| Source | Finds | Misses |
|---|---|---|
| Google results per segment phrase (`seo_get_serp` with `depth: 20`, 2 credits) | SEO-led vendors; list articles whose headings name more (`seo_get_page`, free) | Sales-led vendors with little content |
| The leaders' search rivals (`seo_get_serp_competitors`, 10 credits each) | Vendors sharing the leaders' keywords, with `organic_traffic` | Drop publishers, marketplaces, review sites and directories: these are search rivals, not business ones |
| Ad libraries (`ads_search_ads` on facebook and linkedin, 1 credit a page) | Who is investing in the segment now | LinkedIn rows carry no `active` flag; keep those with `last_shown` in the past 30 days. `active_only: true` works on Facebook only |
| AI answers (`aeo_run_ai_answers`, one "best <category> for <segment>" prompt each, 27 credits a prompt) | The names buyers hear when they ask | Count cells naming each brand, and read the `answer` text for brands not passed in `brands` |

Then merge into one row per company, size them with `seo_get_traffic_estimates` in one call (about 75 credits for 50 domains), place each by what its homepage says (`seo_get_page`: `title`, `h1[]`, `meta_description`; a company can sit in two segments), and fetch `employees` and `founded_year` with `leads_get_company` (1 credit) for the ten that matter most. A four-segment map costs about 225 credits; say so first.

## Reading the map

- **The vendor count compares segments; it is not a census.** It holds only vendors that rank, advertise or get named, so the long tail is missing from every segment equally.
- **Traffic sizes SEO-led vendors well and sales-led ones badly.** An enterprise vendor can have modest traffic and most of the revenue; fetch the company record where it matters.
- **Concentrated or fragmented.** The same few names in every engine and every ad library means a concentrated segment. Many vendors and no consistent AI pick means a fragmented one, which is an opening. AI answers are noisy run to run (a stable core, a shuffled order), so read a brand named in most cells as a leader, not its position.
- **A gap is a hypothesis.** A segment with few or weak players needs a demand check ([demand signals](demand-signals.md)) before anyone calls it an opportunity: few vendors can mean few buyers.

## Deliverable

Two tables. The map, one row per segment: leaders (by AI mentions, then traffic), vendors found, active advertisers, the approach most take, and the gap. The players, one row per company: domain, segments, `organic_traffic`, AI mentions (cells out of those run), advertising now, employees and founded year where fetched. Name every source that was not checked: a signal not checked is not a signal absent.

## Without the connector

Search each segment phrase in a private window and read page one and the list articles; search the Meta and LinkedIn ad libraries by the segment phrase; ask two or three AI assistants the "best <category> for <segment>" question in fresh sessions. Say that hand samples miss more players than tool pulls.
