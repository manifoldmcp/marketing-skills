---
name: research-google-ads
description: When the user wants to research ads on Google, for named competitors or the leaders of a category. Finds each advertiser in the Google ads library by domain or brand name, ranks their search, display and YouTube ads by how long they have run, reads the headline, body and landing page of the long-runners, sets recent tests apart, and hands back a table per competitor or a category file grouped by angle and offer. Also use when the user mentions the Google Ads Transparency Center, is a competitor advertising on Google, the search ads or YouTube ads a rival runs, or the offers a category pays for in search. Meta, TikTok and LinkedIn ads go to research-meta-ads, research-tiktok-ads and research-linkedin-ads, who bids on the brand name of the user to check-google-brand-bidding, keywords for search ads to find-google-ads-keywords, and a weekly watch of new ads to monitor-competitors.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Research Google ads

What advertisers pay to show on Google search, display and YouTube, read from Google's ad library: for one or more named competitors, or for the leaders of a category the user names. Google's library is keyed by advertiser, not by keyword, so a category read starts from a list of brands. It ends in a table per competitor or a file of the leaders' ads grouped by angle, long-runners first, because an ad that keeps running is the closest thing the library has to a result.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `ads_get_advertiser_ads`, `ads_search_advertisers` and `ads_get_ad` (hosts often add a prefix, for example `mcp__manifold__ads_get_advertiser_ads`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `ads_*` tools are not, the ads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so; this skill needs it.
- Landing pages use `seo_get_page`. If the seo group is off, skip that step, say so, and work from the ad text.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the competitors and their domains, the category leaders, and the market) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **The job**: named competitors' ads, or the ads of a category's leaders. Take it from the request.
- **Advertisers**: one to five competitors, or three to five category leaders, each with its domain. `ads_search_ads` does not take Google, so there is no keyword sweep here: when the user has no list of leaders, get one from them or from [find-competitors](../find-competitors/SKILL.md) first.
- **Market**: default every country. Pass `country` (a two-letter code) on the ad list when one market matters, and `region` on the advertiser search.
- **Depth**: up to three pages per advertiser, and the ten longest-running creatives read in full per competitor, or 20 in a category file.
- **Budget**: say it before starting; pass `max_credits` if the user gave a budget.
  - Competitors: about 13 credits each: up to three list pages and ten ads read in full, plus 1 for an advertiser search where the domain finds nothing. Three competitors cost about 40.
  - Category: about 30 credits for five leaders: one or two list pages each, an advertiser search where needed, and 20 ads read in full.
  - Never pass `details: true` without asking: it costs 25 credits a page.

## Steps

1. **Resolve each advertiser.** As [the libraries](../create-paid-ads-plan/references/ad-libraries.md#the-libraries) set out: with the domain, pass it straight to `ads_get_advertiser_ads` as `advertiser` with `platform: "google"` (1 credit a page), the cheapest path. When the user has only a brand name, when the domain finds nothing, or when one market's account matters, call `ads_search_advertisers` first (1 credit) with `platform: "google"`, the brand name and `region`. One brand has one entry per region and similar names belong to other companies, so keep the id whose `website` is the advertiser's domain and pass that id.
2. **Pull the ads.** `ads_get_advertiser_ads` for each advertiser, with `country` when one market matters, paging with `meta.cursor` up to three pages while new rows come back (one or two per leader in a category file). List rows carry format (text, image, video), dates and an image, but no text. `active_only` has no effect here: Google does not mark an ad as running.
3. **Rank by survival.** Dedupe on `id` and work out days running for each as the [winner rules](../create-paid-ads-plan/references/ad-libraries.md#winners) set out: `active` is null on Google, so still running means `last_shown` within the last 7 days, and days running is `last_shown` minus `first_shown`, or today minus `first_shown` while it runs. When a row gives neither date, days running is unknown: say so rather than guess. Sort by days running, and split the rows by format: text ads are search ads, video ads mostly run on YouTube, image ads on the display network.
4. **Read the long-runners.** Follow [reading an ad](../create-paid-ads-plan/references/ad-libraries.md#reading-an-ad), on each competitor's ten longest-running ads or the 20 best in a category file:
   - The text: `ads_get_ad` with `platform: "google"` and the row's `url` as `id` (1 credit). It returns `headline`, `body`, `destination_url` and an impressions range. Read the few ads that matter this way rather than paying 25 credits a page for `details: true`.
   - Video ads: Google video has no transcript route here; work from the text.
   - The landing page: `seo_get_page` on each distinct `destination_url` (free): `title`, `h1` and `h2` show the offer the page leads with.
5. **Group and tag.** Group ads with the same headline or body into one creative and count its variants: many variants of one creative is a winner being scaled. Tag each:
   - Angle: pain, outcome, social proof, comparison, objection, offer-led, brand.
   - Offer: free trial, discount, free shipping, demo, guarantee, lead magnet, none.
   - Network: search, display or YouTube, from the format.
   - CTA: the call in the headline or the last line of the body.
6. **Set the recent ones apart.** Ads first shown in the last 14 days are current tests. List them apart: they show where an advertiser is heading, not what works.
7. **Deliver**, with the reminder at the top that the library shows what runs, not what converts.
   - Competitors: a table per competitor: ad link, network, format, first shown, days running, still running, variants, headline, body, angle, offer, landing page title, and the published `impressions` range where present. Under each table, five lines: which networks they buy, how many ads run now, their three proven creatives and why they likely work, their core offer, and their current tests.
   - Category: the leaders' ads grouped by angle, search ads apart from YouTube and display. Per ad: advertiser, library link, network, days running, still running, variants, headline, offer, landing page title. Then a summary table: angle, ads, long-runners, advertisers using it, most common offer.
   - End with what the user can take from it.

## Judgment

- No ads found is not proof of no ads. Try the brand name through `ads_search_advertisers`, another region's account, the parent company, or a product's own domain; big companies run ads from several advertiser accounts.
- The library does not say which keywords a search ad bids on. [find-google-ads-keywords](../find-google-ads-keywords/SKILL.md) covers that, and [check-google-brand-bidding](../check-google-brand-bidding/SKILL.md) covers who bids on the user's own name.
- An agency, a reseller or an affiliate running ads for the competitor shows under its own name. Check `destination_url` before counting an ad as the competitor's.
- The same offer across every ad is the advertiser's core offer. An offer that appears in one ad only is a test.
- A category read covers only the leaders the user named. Say so: advertisers outside the list are not in the file, and the keyword libraries ([research-meta-ads](../research-meta-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md)) catch more of them.
- Never copy a competitor's ads, claims or words. Take the structure: the angle, the offer type, the landing page the ad leads to.
- Credits and handoff follow the [ad libraries notes](../create-paid-ads-plan/references/ad-libraries.md#credits): say the estimate first, pass `max_credits` on a budget, and never buy, launch or pause ads. The server keeps no state; a weekly rerun of a competitor read is [monitor-competitors](../monitor-competitors/SKILL.md).
- **Every platform at once.** When the user wants all of one competitor's ads, or a category's, across platforms, run [research-meta-ads](../research-meta-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md) and [research-linkedin-ads](../research-linkedin-ads/SKILL.md) in turn, then merge the tables by angle and offer. The same angle running on two platforms is a stronger signal than either alone. For a one-page view of every library, [tear-down-competitor](../tear-down-competitor/SKILL.md) reads one page from each.

## Related skills

- The same read on the other libraries: [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md).
- The keywords to bid on in search ads: [find-google-ads-keywords](../find-google-ads-keywords/SKILL.md).
- Who bids on the user's brand name in Google: [check-google-brand-bidding](../check-google-brand-bidding/SKILL.md).
- A brief for the user's own ads from these long-runners: [write-ad-brief](../write-ad-brief/SKILL.md).
- Where paid ads fit in the plan and what to spend: [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
- Everything about a competitor beyond its ads: [tear-down-competitor](../tear-down-competitor/SKILL.md).
- New ads from the same competitors flagged every week: [monitor-competitors](../monitor-competitors/SKILL.md).
