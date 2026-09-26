# Competitor ads

What one or more competitors are paying to show, in every library they use: the ads, since when, the offer, the hook and the page each one sends people to. It ends in a table per competitor with the long-runners at the top, because an ad that keeps running is the closest thing the libraries have to a result.

## Inputs to settle first

- **Competitors**: one to five, each with its domain. The advertiser differs per library: a page name or page id on Meta, the advertiser name on TikTok, a company name or id on LinkedIn, the domain or an advertiser id on Google.
- **Libraries**: default all four. They cost a credit a page, so skipping one saves little; skip LinkedIn only when the competitor plainly sells to consumers.
- **Market**: default every country. Pass `country` (a two-letter code) when one market matters, and `region` on Google's advertiser search.
- **Depth**: default up to three pages per library per competitor, and the ten longest-running ads read in full.
- **Budget**: a default run on three competitors costs about 3 x (12 + 10 + 5) = 81 credits: per competitor, up to 12 list pages, 10 ads read in full and 5 video transcripts, plus 1 for a Google advertiser search where the domain finds nothing. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Resolve each advertiser.** On Google, start with the competitor's domain as `advertiser`: it is one call. If it finds nothing, or the user cares about one region's account, `ads_search_advertisers` with `platform: "google"` and the brand name (1 credit), keeping the id whose `website` is the competitor's domain. On Meta, a name can match the wrong page: if the first rows are someone else's, take the page id from `advertiser_id` on a row of the right brand (`ads_search_ads` with the brand name, 1 credit, finds one) and pass that instead.
2. **Pull the ads.** `ads_get_advertiser_ads` on each library (1 credit a page), with `country` when one market matters, paging with `meta.cursor` up to three pages while new rows come back. On Meta, `active_only: true` when the user wants only what runs now.
3. **Rank by survival.** Work out days running and still running for each ad as the [router](../SKILL.md#winners) sets out, and sort each competitor's ads by days running. Group near-identical ads (same body or headline) into one creative and count its variants: many variants of one creative is a winner being scaled.
4. **Read the long-runners.** For each competitor's ten longest-running creatives, follow [reading an ad](../SKILL.md#reading-an-ad): `ads_get_ad` where the row has no text (1 credit each; on Google pass the row's `url`), a transcript for the five best video ads (1 credit each), and `seo_get_page` on each distinct `destination_url` (free). Note per ad the hook, the angle, the offer (trial, discount, demo, guarantee, lead magnet), the proof it uses and the CTA.
5. **Read the recent ones.** Ads first shown in the last 14 days are the competitor's current tests. List them apart: they show where the competitor is heading, not what works.
6. **Deliver** a table per competitor: library, ad link, format, placements, first shown, days running, still running, variants, headline, hook, offer, CTA, landing page title, and the published `impressions` and `spend` ranges where present. Under each table, five lines: which libraries they use, how many ads run now, their three proven creatives and why they likely work, the angles they repeat, and their current tests. End with what the user can take from it, and the reminder that the libraries show survival, not performance.

## Judgment

- No ads found is not proof of no ads. Try another spelling, the page id, the parent brand, or a product's own page; big companies run ads from several pages and advertiser accounts.
- On Meta, an ad missing from a second pull a week later has probably stopped: the library keeps few stopped ads outside the EU.
- Google's format tells the network: text ads are search ads, video ads mostly run on YouTube. The library does not say which keywords a search ad bids on; [PPC keywords](ppc-keywords.md) covers that.
- An agency or a reseller running ads for the competitor shows under its own name. Check `destination_url` before counting an ad as the competitor's.
- The same offer across every ad and library is the competitor's core offer. An offer that appears in one ad only is a test.
- Never copy a competitor's creative, footage or claims. Take the structure: the hook pattern, the angle, the offer type.
