# Reading ad libraries

The public ad libraries of Meta, TikTok, LinkedIn and Google are the only outside evidence of what a competitor pays to show. They show creative and dates, never results. Every paid ads job here reads them the same way; other topics point to this page. Checked October 2026.

## What a library can and cannot say

- **It can say** what an advertiser runs, since when, in which format, where it sends people, and (on Meta) on which placements. That is enough to see angles, offers, landing pages, and what an advertiser keeps paying for.
- **It cannot say** clicks, CTR, conversions, CPA or ROAS, which keywords a search ad bids on, or what a commercial ad cost. Spend is published only for political and social-issue ads (Meta, and election ads on Google). Say this at the top of every deliverable.
- **Ads shown in the EU carry more.** The EU Digital Services Act (Article 39, in force for the largest platforms since August 2023) makes them keep every ad shown in the EU for a year after it last ran, with the payer, the dates, the main targeting parameters and the reach by member state. That is why an EU-shown ad comes back with `reach`, `reach_by_country`, `targeting` and `payers`, and the same advertiser's US-only ad comes back with none. Never read a missing reach as low reach.
- **A missing ad means "not found", not "not running".** Libraries lag, keyword search is ranked by the library and never complete, and big companies advertise from several pages, regions and agencies.

## The four libraries

| Library | What it holds | Running flag | The trap |
|---|---|---|---|
| Meta (`platform: "facebook"`) | Every ad running now on Facebook, Instagram, Messenger, Threads, WhatsApp and Audience Network; stopped ads only if shown in the EU (a year after the last impression; the UK too, by third-party guides) or political (seven years) (Meta Transparency Center) | `active` | Keyword search returns stopped EU ads too, and loosely matching ones |
| TikTok (`platform: "tiktok"`) | The Commercial Content Library: ads shown in the EEA, Switzerland and the UK only | none | A US-only advertiser is invisible; data lags weeks |
| LinkedIn (`platform: "linkedin"`) | Ads run since June 2023, each kept a year after it last served (LinkedIn Help), with impression ranges and country shares where LinkedIn publishes them | none | Keyword search sorts newest first; many rows carry no dates |
| Google (`platform: "google"`) | The Ads Transparency Center: verified advertisers' search, display and YouTube ads, keyed by advertiser | none | A domain pull mixes in agencies and unrelated advertisers |

### Meta

- `ads_search_ads` (keyword) or `ads_get_advertiser_ads` (a page name, or the numeric id from a row's `advertiser_id` when a name matches the wrong page). `active_only: true` keeps ads running today and is the only library where it works.
- **Without `active_only`, a keyword search is mostly stopped ads.** In our October 2026 run, 17 of 29 rows for "crm software" had stopped, almost all of them ads shown in the EU. Five rows matched "crm" loosely (job and virtual-assistant ads). Pass `active_only: true` for a category sweep and drop off-topic rows by `destination_url`.
- `versions` counts the near-identical copies Meta groups under one ad: a high count is a creative being scaled. `placements` says where it ran. `countries`, `targeting` and reach are filled only for EU and UK ads.
- `active` and `last_shown` disagreed on 2 of 12 running ads in our run (active, last shown weeks earlier). Trust `active` for "still running", and say when the two disagree.
- For an EU or UK ad, `ads_get_ad` returns `reach`, `reach_by_country`, `reach_by_age_gender` and `payers`. The total and the country sum can differ (1.99M against 2.17M on one ad): quote one, never add them.

### TikTok

- **Europe only.** TikTok's library exists to meet the DSA, so it holds ads shown in the EEA, Switzerland and the UK. In our run every advertiser for "crm software" was European or British, and the newest ad had last run 20 days earlier. For a brand that advertises on TikTok only in the US, the library shows nothing; say so, and point the user to TikTok's Creative Center Top Ads by hand.
- `country` has no effect. List rows carry dates, format, a cover image and often the caption. `ads_get_ad` adds `destination_url` when TikTok has one, but in our run it returned no `body`, no countries and no `objective` on two of two ads, less than the list row. Keep the list row's text.
- The spoken hook is the ad: `tiktok_get_transcript` on the ad's `url` (1 credit). On NoData, find the same video on the advertiser's account with `tiktok_get_videos` and `sort: "popular"`, matched on the caption; `ai_fallback: true` transcribes audio for 11 credits.

### LinkedIn

- List rows already carry `headline`, `body`, `cta` and often `destination_url`, so it is the cheapest library to read. `targeting` shows languages and places (with exclusions), and `ads_get_ad` adds each country's share in `reach_by_country`.
- **Keyword search returns the newest ads first.** In our run every dated row had first run in the past three days, and 10 of 24 rows had no dates or impressions at all. A keyword sweep shows what the category is launching, not what lasts; pull advertisers by name to judge survival.
- **`countries` on LinkedIn rows lists where impressions were counted;** `targeting` says where the ad was aimed, which can be a city or region.
- About one row in five was an individual's post promoted as an ad (LinkedIn's Thought Leader ads), not a company ad. Tag them apart: they show which people a competitor puts its money behind.

### Google

- `ads_search_ads` does not take Google. With the competitor's domain, pass it straight to `ads_get_advertiser_ads` as `advertiser` (1 credit a page). With only a name, or when one market's account matters, `ads_search_advertisers` with `region` first (1 credit): one brand has one entry per region, and similar names belong to other companies, so keep the id whose `website` is the domain.
- **A domain pull is loose.** For pipedrive.com, 34 of 39 rows were Pipedrive's own, four were an agency's display ads and one was an unrelated German signage company. Keep rows whose `advertiser` is the competitor, check agency rows by `destination_url`, and drop the rest.
- List rows carry format (text is search, video mostly YouTube, image display), dates and an image, no text. Read text with `ads_get_ad`, passing the row's `url` as `id` (1 credit): `headline`, `body`, `destination_url`, `variations` (every text variant), and impressions and per-country dates for ads shown in the EU. **`ads_get_ad` on Google often has no `first_shown`** (it falls back to the earliest region's date, which a US-only ad lacks): take days running from the list row. `details: true` costs 25 credits a page; ask first.
- Rows come sorted by when they last ran, not by age. Sort by `first_shown` yourself.

## Winners: what run length means

- An advertiser stops paying for an ad that loses, so an ad that has run long and still runs is the best proxy for a winner the libraries offer. Days running is `last_shown` minus `first_shown`, or today minus `first_shown` while it runs. Still running is `active: true` on Meta, or `last_shown` within the past 7 days elsewhere. When a row gives neither date, say days running is unknown.
- Running 30 days or more and still running: a likely winner. 90 days or more: proven. Under 14 days: a test, which says what the advertiser is trying, not what works. Many near-identical ads launched the same day are a test; the one still running a month later won it. The same body across formats and placements is a winner being scaled.
- **Compare within an advertiser, not across.** A large, steady advertiser keeps most of its ads running: 22 of Pipedrive's 34 live Google ads had run 90 days or more. For them "long-running" is the default, and the signal is which ads were cut and which new ones survive.
- **Automation keeps weak ads alive.** Advantage+ and Performance Max shift budget between ads without stopping them, so an ad can stay "running" on almost no spend. Run length says the advertiser has not killed it; it cannot say it gets the budget.
- No published study has tested run length against real performance. It is a sound inference, not a measurement: say so.
- `impressions` and `spend` are published ranges, as text ("1k-5k", "150000 to 175000"), and often null. Quote them as given; never convert, sum or rank on them.

## Reading an ad

1. **The text.** `ads_get_ad` (1 credit) where a row lacks `body` or `destination_url`: usually on Google, sometimes on TikTok. The id is the row's `id` on Meta and TikTok, its `url` on LinkedIn and Google. The hook is the first line of the body.
2. **The words of a video.** `tiktok_get_transcript` or `facebook_get_transcript` on the ad's `url` (1 credit). On NoData (free, do not retry), find the post on the advertiser's own account (`tiktok_get_videos`, `instagram_get_reels`, `facebook_get_posts`) by its caption and transcribe that (`instagram_get_transcript` for a reel up to two minutes). LinkedIn and Google video have no transcript route; use the text. The first spoken sentence is the hook.
3. **The landing page.** `seo_get_page` on `destination_url` (free, rate limited): `title`, `h1` and `h2` show the offer the page leads with and whether it is gated. No Manifold tool reads page body text.
4. **Link it.** Keep each row's library `url` so the user can see the creative; the tools return text and a media link, not the video.

## Credits and caching

- Lists cost 1 credit a page, `ads_get_ad` and `ads_search_advertisers` 1 credit, Google `details: true` 25 a page, transcripts 1 (11 with `ai_fallback: true`). Three pages per library per advertiser covers what most advertisers run.
- Lists are cached 24 hours, one ad and an advertiser search 7 days. A repeat this account already paid for is free; a cached result another account paid for costs a quarter of the price, at least 1 credit.
- Say the estimate before the first paid call. On a budget, pass `max_credits` on every call and stop at BudgetExceeded; `dry_run: true` prices a call for free.

## Handoff

- Never buy, launch, edit, pause or boost ads, and never ask for ad account access. The deliverable is a table or a brief the user takes into Ads Manager, TikTok Ads Manager, Campaign Manager or Google Ads.
- The user's own spend, CPA and ROAS live in their ad accounts. Ask for them when the work needs them.
- Never copy a competitor's footage, claims or lines. Take the structure: the angle, the hook pattern, the offer type, the landing page.
- The server keeps no history. To watch a competitor's ads over time, the host stores each table and reruns it; the watch itself belongs to [competitor-intel](../../competitor-intel/SKILL.md).
