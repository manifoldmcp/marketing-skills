# Pitch data

The numbers for a pitch deck or proposal: where the prospect stands against three competitors and where the growth is, one table per slide, each with its source and date. Checked October 2026.

## One story

- **Lead with the slide that sells the service.** A deck for SEO leads with share of page one and the gap; for AI search with share of voice in answers; for paid with who advertises and for how long. The rest is backup.
- **Proof beats promise.** Clients ask first whether marketing connects to revenue (55% of agencies hear it regularly, AgencyAnalytics 2026), and agencies credit "clear return and expected results" (46%) for wins. Size what closing the gap is worth (searches a month, the competitor's estimated value of those clicks), never a date by which a number will move.
- **Choose competitors of a similar size.** A regional chain set against a national brand loses the room. `seo_get_serp_competitors` returns SERP competitors: drop publishers, marketplaces and directories.
- **Practical ideas win.** In the UK, the share of agency new-business staff who say they commonly win because the idea is practical, affordable and deliverable rose 8 points to 33% (JFDI and Opinium New Business Barometer, seventh edition, 266 new-business staff; reported second-hand, the full report is not public). A plan the prospect can picture running next month beats a grand vision.

## Inputs

- **Prospect, service, market** (`location` and `language` if not United States and English).
- **Competitors**: three. Default: the top three real businesses from `seo_get_serp_competitors` with `limit: 10` (6 credits).
- **Category**: the words buyers search with. Default: the prospect's top commercial keywords.
- **Buyer questions**: five prompts for the AI slide: "best category" variants and the main use cases.
- **Deadline**: AI answers are tasks that take minutes; everything else returns at once.

## The calls (about 523 credits, or 300 without the 12-month history)

1. **Traffic over a year.** `seo_get_domain_overview` with `history: true` on all four (71 each): `history[]` gives 12 months of `organic_traffic` and `organic_keywords` for the line chart; the same call gives the scoreboard (`domain_rank`, `organic_keywords`, `positions`). Check every brand's line for the same seasonal dip before calling one a decline.
2. **Share of page one.** `seo_search_keywords` with the category as `seed` (10 credits); keep the ten highest-volume keywords a buyer would search; `seo_get_serp` on each (1 credit each). Per brand, count the keywords where it is on page one and sum their volume. It is one location, one device, one day: fine for a pitch, not for a report.
3. **The gap.** `seo_get_keyword_gap` of the prospect against each competitor (10 each): the count (`meta.rows_available`), the summed `volume` of the rows returned, three examples a buyer would recognise.
4. **Authority.** `seo_get_backlink_summary` on each (10 each): `referring_domains` and `dofollow_share` beside `domain_rank`.
5. **AI share of voice.** `aeo_run_ai_answers` with the five questions and `brands` set to all four (135 credits), then `get_task`. Per brand, cells mentioned and cited out of 25. Five prompts is a small sample: write "in 25 answers", never "ChatGPT never recommends you", and give the margin of error from [ai-search](../../ai-search/SKILL.md).
6. **Paid presence.** `ads_get_advertiser_ads` on Meta and Google for each (8 credits): running ads and the oldest still running.

Calls repeated from a [prospect audit](prospect-audit.md) in the same week (the competitor list, a gap, the overviews) are cached and free.

## Deliver

One table per slide, each with a one-line headline, the tool and the date:

- Traffic trend: month, one column per brand (estimated organic visits).
- Scoreboard: brand, domain rank, referring domains, keywords in the top 3 and top 10, estimated organic traffic, running ads.
- Share of search: keyword, volume, each brand's position (blank when not on page one), and a total row with each brand's share.
- Gap: competitor, keywords they have that the prospect lacks, summed volume, three examples.
- AI share of voice: brand, mentions and citations out of 25, split by engine.

Label every figure as an estimate from a third-party index measured the same way for every brand, and never set it beside the prospect's own analytics as if the two were one number. If the prospect's `top_10` count is near zero, replace the traffic slide with share of page one: the estimate is modelled from deep rankings and can be far off ([field notes](field-notes.md)). After the win the same set becomes the baseline in [onboarding](onboarding.md).
