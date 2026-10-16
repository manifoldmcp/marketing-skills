# Teardown

One competitor across every channel in one table: how big it is, how strong in search, what it pays to advertise, where it posts, and whether AI engines recommend it. A snapshot with the three things to act on, not a report on everything. Checked October 2026.

## What each source reveals, and costs

| Channel | Calls | Credits | What it says | Trap |
|---|---|---|---|---|
| Company | `leads_get_company`, then `linkedin_get_company` with its `linkedin_url` | 2 | Category, founding year, two headcounts, growth, revenue range | The two headcounts disagree; funding is never there ([reading the numbers](estimates.md#company-size)) |
| Search | `seo_get_domain_overview`, `seo_get_ranked_keywords` with `limit: 100`, `seo_get_backlink_summary` | 25 | Strength, the topics and pages that earn non-branded traffic, the commercial keywords where it meets buyers, link base | Estimates, not visits; the model changes on 1 November 2026 |
| Paid | `ads_get_advertiser_ads` on facebook (brand), linkedin (company), tiktok (advertiser), google (domain) | 4 | Active ads per library, the oldest still running, formats, offers and landing pages | Only Facebook honours `active_only`; LinkedIn rows are mostly undated |
| Social | Profiles on TikTok, Instagram, YouTube, X and Facebook, then one page of recent posts each, plus `linkedin_get_company_posts` | about 11 | Followers, posts in 30 days, median engagement | `linkedin_get_company_posts` carries no engagement; X returns one page |
| AI answers | `aeo_run_ai_answers`, two category prompts, `brands` holding the user and every rival likely to be torn down next | 36 | Cells where each brand is named; the domains cited | Ten cells show presence or absence, not share of voice |

A default teardown is about 78 credits, plus 25 for the user's own search row. Results are cached (7 days for SEO, 24 hours for ad lists and profiles, 30 days for company records), so a second teardown in the same week reuses them free, and one AI run can serve several competitors if they are all in `brands`.

## Reading it

- **Branded first.** Split ranked keywords into branded and non-branded before comparing with the user. A well-known rival's traffic is mostly its own name.
- **What ads reveal about strategy.** The offer in long-running ads (a trial, a migration service, a discount) is the offer that works for them. A burst of new ads on one theme is a launch or a push: in our October 2026 read, 20 of the first 24 Pipedrive LinkedIn ads pushed one product launch, most written in the first person like promoted posts from its own people (our reading of the copy, not a field the library returns). The landing pages in `destination_url` show which segment each push targets. Pass `country` as an EU market where the rival sells to see a year of stopped ads with reach and targeting, which the DSA requires there. The full creative read and the winner thresholds are in [paid-ads](../../paid-ads/SKILL.md).
- **Absence is the finding.** A channel with an account but no post in 90 days is inactive: report it as absent, since that is ground the user can take. "None found" in an ad library is not "none".
- **Headcount says what they can outspend.** A small team should not fight a paid war with a large rival; it should take the segments and channels the rival ignores.
- **AI answers say whether buyers hear about them.** A rival strong in search but missing from AI answers is exposed there, and the reverse. Depth is in [ai-search](../../ai-search/SKILL.md).

## What a teardown cannot see

Pricing, product quality, the sales motion, email marketing, funding and customer counts. No Manifold tool reads body text, so never fill a price, a feature or a number of customers from memory; where it matters, the host's browser, the user, or "not read". Quote the rival's own copy only as short evidence (a headline, a tagline) with the URL.

## Deliver

One row per channel: channel, what was measured (numbers with the tool), what it says in one line, the user's number where there is one, and the topic that goes deeper. Mark channels the tools could not check "not checked". Then three lines: where the competitor is strong, where it is absent or weak, and the one thing to act on first. This is a snapshot; a trend needs a second run ([monitoring](monitoring.md)).

## Without the connector

The public Meta Ad Library, LinkedIn Ad Library, TikTok Ad Library and Google Ads Transparency Center show a rival's ads for free; its homepage, pricing page, LinkedIn page and social profiles show the rest. Search the rival's category in a private window on two or three AI engines. Traffic cannot be checked by hand; say so rather than quoting a remembered figure.
