# Swipe file

A swipe file is the category's ads in one place, sorted so a marketer can see which angles, offers and formats advertisers keep paying for. It is built by keyword, not by competitor, so it catches advertisers the user never named. It ends in a grouped table with a link to every ad.

## Inputs to settle first

- **Keywords**: three to five words the category's ads use: the product ("meal kit"), the problem ("dinner ideas"), the offer ("first box free") and the leading brand names. Ask for two; add the rest from the first pull.
- **Libraries**: keyword search runs on Meta, TikTok and LinkedIn. Default: Meta and TikTok for consumer products, Meta and LinkedIn for B2B. Google has no keyword search, so it adds only the two or three category leaders by name.
- **Market**: `country` if one market matters. Default every country.
- **Size**: default 40 ads in the file.
- **Budget**: a default run costs about 4 x 3 + 20 + 10 + 2 = 44 credits: four keywords at three list pages each on two libraries, 20 ads read in full, 10 transcripts, and one Google page for each of two leaders. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Sweep the libraries.** `ads_search_ads` for each keyword on each chosen library (1 credit a page): two pages on Meta with `active_only: true`, one on TikTok or LinkedIn. Pass `country` when one market matters. Keyword search matches the ad's text, so a keyword with two meanings brings in other categories: drop those rows.
2. **Add the leaders on Google.** For two or three category leaders, `ads_get_advertiser_ads` with each leader's domain (1 credit a page), or the advertiser search first when the domain finds nothing, as the [router](../SKILL.md#the-libraries) sets out. Keep their text ads for the search section of the file.
3. **Dedupe and rank.** Dedupe on `id`, then group ads with the same body or headline into one creative with a count of variants. Mark each creative by the [winner rules](../SKILL.md#winners). Fill the file with the long-runners first, then the recent ads marked as tests, up to the size the user asked for.
4. **Read them.** Follow [reading an ad](../SKILL.md#reading-an-ad): `ads_get_ad` on the rows with no text (1 credit each; TikTok rows always), a transcript for the ten best video ads (1 credit each), and `seo_get_page` on the landing pages (free).
5. **Tag each ad.** Four tags, from the ad itself:
   - Angle: pain, outcome, social proof, comparison, founder story, demo, objection, offer-led.
   - Offer: free trial, discount, free shipping, bundle, guarantee, lead magnet, demo, none.
   - Format: `format` from the row, plus video length or carousel where the text shows it.
   - CTA: the `cta` field, or the call in the last line when it is null.
6. **Deliver** the swipe file, grouped by angle. Per ad: advertiser, library link, format, days running, still running, variants, hook (first line or first spoken sentence), offer, CTA, landing page title. Then a summary table: angle, ads, long-runners, advertisers using it, most common offer. Name the two or three angles with the most long-runners (proven in the category) and any angle no advertiser uses (an opening, or a dead end: test it small).

## Judgment

- A swipe file is for structure, not for copying. The user takes the hook pattern and the angle, never the footage, the claims or the words.
- Proven angles are table stakes. The user's ads need them and something the category does not say yet; the [creative brief](creative-brief.md) adds that from customer language.
- One advertiser with twenty variants can swamp the file. Cap each advertiser at five creatives so the file shows the category, not its biggest spender.
- Keyword search is ranked by the library and never complete. A second pass with other words costs little and is cached for 24 hours.
- LinkedIn ads and Meta ads in the same category use different angles: a job-title hook and a proof point on LinkedIn, a problem shown in the first seconds on Meta. Keep them in separate sections when both are in the file.
- The libraries show what runs, not what converts. Say so at the top of the file.
