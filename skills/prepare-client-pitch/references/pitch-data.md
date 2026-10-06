# Pitch data

A pitch deck needs a few numbers that tell one story: where the prospect stands against its competitors, and where the growth is. This reference pulls them in one pass and hands back tables ready to chart, one per slide, each with its source and date.

## Inputs to settle first

- **Prospect**: the domain, and the service being pitched (SEO, AI search, paid, all of it).
- **Competitors**: three. Default: the top three real businesses from `seo_get_serp_competitors` with `limit: 10` (6 credits). Choose competitors of a similar size: a pitch that sets a regional chain against a national brand loses the room.
- **Category**: the words buyers search with, for the share-of-search slide. Default: the prospect's top keywords from its overview.
- **Buyer questions**: five prompts for the AI share-of-voice slide. Default: "best <category>" variants and the prospect's main use cases.
- **Market**: `location` and `language` if not the United States and English.
- **Deadline**: when the deck is due. AI answers are tasks that take minutes; everything else returns at once.
- **Budget**: a default run costs about 6 + 4 x 61 + 10 + 10 + 3 x 10 + 4 x 10 + 5 x 18 + 8 = 438 credits, or about 215 without the 12-month history. Say so before starting; pass `max_credits` if the user gave a budget. Calls repeated from a [prospect audit](prospect-audit.md) this week (the competitor list, a keyword gap) are cached and free.

## Steps

1. **Traffic over a year.** `seo_get_domain_overview` with `history: true` on the prospect and each competitor (61 credits each). `history[]` gives 12 months of `organic_traffic` and `organic_keywords`: the line chart of the deck. The same call gives the scoreboard: `domain_rank`, `organic_traffic`, `organic_keywords` and `positions`.
2. **Share of page one.** `seo_search_keywords` with the category as `seed` (10 credits). Keep the 10 keywords with the most volume that a buyer would search. `seo_get_serp` on each (1 credit each). For every brand, count the keywords where it is on page one and sum their volume: that is its share of search.
3. **The gap.** `seo_get_keyword_gap` of the prospect against each competitor (10 credits each). Report the number of gap keywords (`meta.rows_available`), the summed `volume` of the rows returned, and three examples a buyer would recognise.
4. **Authority.** `seo_get_backlink_summary` on each domain (10 credits each): `referring_domains` and `dofollow_share`, beside the `domain_rank` from step 1.
5. **AI share of voice.** `aeo_run_ai_answers` with the five buyer questions and `brands` set to all four brands, on the default engines (18 credits a prompt), then `get_task`. For each brand, count the cells (prompt by engine) where `mentions[]` has it mentioned, and where it is cited, out of 25.
6. **Paid presence.** `ads_get_advertiser_ads` with `platform: "facebook"` and with `platform: "google"` for each brand (1 credit each), resolving Google advertisers as [the libraries](../../create-paid-ads-plan/references/ad-libraries.md#the-libraries) set out. Count the running ads (`active: true` on Meta; `active` is null on Google, so `last_shown` within the last 7 days there) and note the oldest `first_shown` still running.
7. **Deliver** one table per slide, each with a one-line headline, the source tool and the date:
   - Traffic trend: month, then one column per brand (estimated organic visits).
   - Scoreboard: brand, domain rank, referring domains, estimated organic traffic, organic keywords, keywords in the top 3, running ads.
   - Share of search: keyword, volume, then each brand's position (blank when not on page one), and a total row with each brand's share.
   - Gap: competitor, keywords they rank for that the prospect does not, the summed volume, three examples.
   - AI share of voice: brand, mentions out of 25, citations out of 25, and the split by engine.

   If the host has a slide or chart tool, offer to pass the tables to it. Nothing is sent from here.

## Judgment

- One story per deck. Lead with the slide that makes the case for the service being sold; the rest is backup.
- Every figure is an estimate from a third-party index, measured the same way for every brand. Label it so, with the date, and never set it beside the prospect's own analytics as if the two were one number.
- A seasonal dip in the 12-month line is not a decline. Check the competitors' lines for the same dip before calling it one.
- Share of page one is the top 10 for one location and device on one day: fine for a pitch, not for a report.
- Five prompts is a small sample. Say "in 25 answers", not "ChatGPT never recommends you".
- Do not promise the numbers will move by a date. Show where the gap is and what closing it is worth.
- After the win, the same set becomes the baseline: [onboard-client](../../onboard-client/SKILL.md) reuses it, and the search calls the two share are cached for a week.
