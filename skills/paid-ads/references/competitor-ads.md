# Researching competitor and category ads

When the user wants to see what named competitors run, or what a whole category pays for (a swipe file), on Meta, TikTok, LinkedIn or Google. Read [ad libraries](ad-libraries.md) first: what each library holds decides what this research can find.

## Two jobs

- **Named competitors.** One to five, each with its Meta page, TikTok advertiser name and handle, LinkedIn company and domain. Pull each with `ads_get_advertiser_ads`, up to three pages while new rows come back, and read the ten longest-running creatives in full. Ends in a table per competitor.
- **A category.** Three to five words the category's ads use: the product ("payroll software"), the problem ("month-end close"), the offer ("free template") and the leading brand names. `ads_search_ads` on Meta, TikTok and LinkedIn, two pages each; it catches advertisers the user never named. Google has no keyword search, so a Google category read starts from a list of three to five leaders. Ends in a swipe file of 20 to 30 ads grouped by angle, at most five creatives per advertiser.
- A request for both runs the competitors first and the category around them second.

## Which library for whom

- **B2B with a named buyer title:** LinkedIn and Google first. LinkedIn ads lead with a job-title hook and a proof point; Meta ads in the same category show the problem in the first seconds. Keep the two in separate files.
- **Consumer, visual, impulse:** Meta first, TikTok if the advertisers sell in Europe (the TikTok library is Europe only).
- **A competitor that plainly sells to consumers rarely runs LinkedIn ads.** Say so instead of spending credits.
- **Search demand exists:** Google, and pair it with [Google search ads](google-search-ads.md) for the keywords, since the library never shows them.

## Costs

| Job | Meta | TikTok | LinkedIn | Google |
|---|---|---|---|---|
| One competitor (3 pages, 10 read) | about 9 (with 5 transcripts) | about 18 (10 ads read, 5 transcripts) | about 3 (rows carry the text) | about 13 (10 ads read, +1 advertiser search) |
| Category | about 20 (4 keywords x 2 pages, 10 transcripts) | about 38 (adds 20 ads read) | about 8 | about 30 for five leaders |

Add 1 credit for each ad read with `ads_get_ad` where a row has no text. Never pass Google `details: true` without asking.

## Rank, read, tag

1. **Dedupe on `id`,** then group ads with the same headline, body or spoken hook into one creative and count its variants (adding Meta's `versions`).
2. **Rank by survival** as [winners](ad-libraries.md#winners-what-run-length-means) sets out, and set ads first shown in the past 14 days apart as current tests: they show where an advertiser is heading, not what works.
3. **Read the long-runners** as in [reading an ad](ad-libraries.md#reading-an-ad): text, transcript of the best five video ads per competitor (ten in a category), landing page.
4. **Tag each creative** from the ad itself:
   - Angle: pain, outcome, social proof, comparison, founder story, demo, objection, offer-led, brand.
   - Offer: free trial, discount, free shipping, bundle, guarantee, demo, lead magnet (report, template, webinar), none.
   - Proof: the number, review, logo, analyst mention or testimonial it leans on.
   - Format and placement; on TikTok, whether it looks like a creator talking to camera or a produced spot; on Google, search, display or YouTube.
   - Audience: the job title the hook calls out, and `targeting` where the library publishes it.
   - CTA: `cta`, or the call in the last line.

## Deliver

- Lead with the reminder that the libraries show what runs, not what converts.
- **Per competitor:** ad link, format, placements or network, first shown, days running, still running, variants, headline, hook, angle, offer, CTA, landing page title, published `impressions` and `spend` ranges where present. Under it, five lines: ads running now, the three proven creatives and why they likely work, the angles they repeat, the core offer, the current tests.
- **Category:** the ads grouped by angle, then a summary table (angle, ads, long-runners, advertisers using it, most common offer). Name the two or three angles with the most long-runners (table stakes) and any angle nobody uses (an opening, or a dead end; test it small).
- **Every platform at once:** run each library, then merge by angle and offer. The same angle surviving on two platforms is a stronger signal than either alone.

## Judgment

- The same offer across every ad is the advertiser's core offer; an offer in one ad is a test. A gated report or demo offer that keeps running says what the category's buyers trade their details for.
- An agency, reseller, affiliate or creator advertising for the competitor shows under its own name. Check `destination_url` before counting an ad as the competitor's.
- On TikTok an ad with no transcript and no text is a row with dates only: count it for survival, never guess its angle from the cover image. Ads that look like organic posts often win there; compare with the niche's top organic videos ([tiktok](../../tiktok/SKILL.md)) before briefing a produced spot.
- No ads found: try another spelling, the page or company id, the parent brand, a product's own page, or another region's Google account.
- Proven angles are table stakes. The user's ads need them plus something the category does not say yet, which comes from customer language ([ad briefs](ad-brief.md)).
- Everything about a competitor beyond its ads, and a weekly watch for new ones, belongs to [competitor-intel](../../competitor-intel/SKILL.md).
