# Monitoring

A weekly watch on two or three competitors that reports only what changed: ads started and stopped, new posts, moves in search footprint, bidding on the user's brand, and new headlines on their home and pricing pages. Checked October 2026.

## What to watch, and what it costs

| Signal | Call | Credits a run | Change to flag |
|---|---|---|---|
| Ads | `ads_get_advertiser_ads` per library the rival uses (leave `details` off) | 1 a page | New ids; ids gone; a new `destination_url` (a new offer or landing page) |
| Posts | One listing per channel: `linkedin_get_company_posts`, `twitter_get_tweets`, `youtube_get_videos`, `tiktok_get_videos`, and `instagram_get_posts` or `facebook_get_posts` where they matter | 1 a page | New ids; a post at twice the account's stored median engagement; launches, pricing, funding, hires, customer wins |
| Search footprint | `seo_get_domain_overview` | 5 | A URL new to `top_pages[]` (a comparison page, a free tool, a new product page); traffic only month over month |
| Brand bidding | `ads_get_advertiser_ads` with platform google for each rival (counted in Ads above) | 1 an ad | A new text-format ad: read it with `ads_get_ad` for the user's brand name in its headline. `seo_get_serp` seldom returns paid rows, so it cannot show this. The full read is [paid-ads](../../paid-ads/SKILL.md) |
| Pages | `seo_get_page` on home and pricing | free | New `title`, `h1` or `h2`: new positioning; new plan names: a pricing change |

About 13 credits a competitor, plus 1 for each new search ad read: about 39 a run for three rivals, about 168 a month weekly. The first run on Google may add `ads_search_advertisers` (1 each) to resolve the advertiser id per region.

## Cadence and noise

- **Weekly.** Ad lists and profiles are cached 24 hours, platform listings 6 hours, domain overviews and single ads 7 days: a daily run repeats the same numbers for nothing.
- **Traffic moves under about 10% a week are noise.** Report traffic month over month and flag weekly only a jump or a new top page. And the estimate model changes on 1 November 2026: a step across that date is the model ([reading the numbers](estimates.md#the-dataforseo-change-on-1-november-2026)). Re-baseline after it.
- **Stopped ads.** On Facebook an id stored as active that no longer appears, once every page is read (follow `meta.cursor` before calling it stopped). Elsewhere, a `last_shown` before the last run; LinkedIn list rows mostly have no dates, so on LinkedIn compare ids only.
- **One new ad is a test; five on one angle are a push.** An ad that stops within a week or two most likely lost its test. Keep each id's first sighting so every run can say how long it has run.
- **Gaps.** `twitter_get_tweets` returns one page, so a rival posting more than a page between runs leaves a hole; LinkedIn `created_at` is approximate, so dedupe on `id`.
- **Answer at most three changes.** Name the change worth a response and the topic that acts on it; responding is the user's call.

## State

The server keeps no history and has no scheduler. The host stores the settings (rivals, domains, handles, libraries, Google advertiser ids) and each run's state (ad ids with `active` and `last_shown`, post ids, overview numbers, page headings, the date and credits) where the next run can read it: a file, a doc, a sheet, its memory. Without it every run is a first run. The first run is the baseline, reported as the starting picture. A rival or channel added later starts its own baseline. If the host has a scheduler (a scheduled task, a routine, cron), set the watch up there; if not, say so. The server never sends alerts; if the host has an email or chat tool, offer to send the table there.

## Deliver

A change table: competitor, channel, what changed, the detail (headline, offer, topic, numbers before and after), evidence URL, first seen, why it matters. A quiet week is one line per competitor.

## Without the connector

Bookmark each rival's page in the Meta, LinkedIn and TikTok ad libraries and the Google Ads Transparency Center, follow their social accounts, and check their home and pricing pages weekly; log what is new by hand. Search traffic cannot be watched without a tool.
