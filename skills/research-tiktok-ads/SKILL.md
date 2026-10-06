---
name: research-tiktok-ads
description: When the user wants to research ads on TikTok, for named competitors or across a whole category. Reads the TikTok ad library by advertiser or keyword, ranks the ads by how long they have run, reads the text, transcripts and landing pages of the long-runners, sets recent tests apart, and hands back a table per competitor or a swipe file grouped by angle and offer. Also use when the user mentions the TikTok ad library, TikTok ads, what ads a brand runs on TikTok, a swipe file or ad inspiration from TikTok ads, winning TikTok ads in a niche, or the hooks and offers a market pays for on TikTok. Meta, LinkedIn and Google ads go to research-meta-ads, research-linkedin-ads and research-google-ads, organic TikTok hooks to find-tiktok-hooks, a brief for new ads to write-ad-brief, a paid ads plan to create-paid-ads-plan, and a weekly watch of new ads to monitor-competitors.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Research TikTok ads

What advertisers pay to show on TikTok, read from TikTok's ad library: for one or more named competitors, or for a whole category by keyword so it catches advertisers the user never named. Each ad comes with its dates, its spoken hook, the offer and the page it sends people to. It ends in a table per competitor or a swipe file grouped by angle, long-runners first, because an ad that keeps running is the closest thing the library has to a result.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `ads_get_advertiser_ads`, `ads_search_ads` and `ads_get_ad` (hosts often add a prefix, for example `mcp__manifold__ads_get_advertiser_ads`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `ads_*` tools are not, the ads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so; this skill needs it.
- Video ads use `tiktok_get_transcript` (and `tiktok_get_videos` when the library holds no video), and landing pages `seo_get_page`. If one of those groups is off, skip its step, say which, and work from the ad text.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the competitors, their TikTok advertiser names, handles and domains, the category and its words, and the market) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **The job**: named competitors' ads, or the best ads across a category (a swipe file). Take it from the request; a request for both runs the competitors first and the category around them second.
- **Competitors**: one to five, each with its advertiser name on TikTok, its TikTok handle (for the transcript fallback) and its domain.
- **Keywords** (category job): three to five words the category's ads use: the product ("protein bar"), the problem ("afternoon slump"), the offer ("free shipping") and the leading brand names. Ask for two; add the rest from the first pull.
- **Market**: `country` has no effect on TikTok's library, so the run covers every country; `ads_get_ad` lists the countries each ad ran in, and the user can filter on those.
- **Depth**: competitors, up to three pages each and the ten longest-running creatives read in full. Category, 20 ads in the file, each read in full.
- **Budget**: say it before starting; pass `max_credits` if the user gave a budget.
  - Competitors: about 18 credits each: up to three list pages, ten ads read in full and five video transcripts. Three competitors cost about 54.
  - Category: about 38 credits: four keywords at two list pages each, 20 ads read in full and ten video transcripts.
  - A transcript with `ai_fallback: true` costs 11 instead of 1; ask before using it on more than a few ads.

## Steps

1. **Pull the ads.** Both tools take `platform: "tiktok"`, 1 credit a page.
   - Competitors: `ads_get_advertiser_ads` with the advertiser name as `advertiser`, paging with `meta.cursor` up to three pages while new rows come back.
   - Category: `ads_search_ads` for each keyword, two pages.
   - `active_only` has no effect here: TikTok does not mark an ad as running.
2. **Rank by survival.** List rows carry dates, format and a cover image but no ad text. Dedupe on `id` and work out days running for each as the [winner rules](../create-paid-ads-plan/references/ad-libraries.md#winners) set out: `active` is null on TikTok, so still running means `last_shown` within the last 7 days, and days running is `last_shown` minus `first_shown`, or today minus `first_shown` while it runs. Sort by days running. When a row gives neither date, days running is unknown: say so rather than guess.
3. **Read the long-runners.** Follow [reading an ad](../create-paid-ads-plan/references/ad-libraries.md#reading-an-ad), on each competitor's ten longest-running ads or every ad in the category file:
   - The text: `ads_get_ad` with the row's `id` (1 credit), always, since the rows carry none. It adds the title, the landing page, the countries and the published ranges. Read `headline`, `body`, `cta` and `destination_url`.
   - The words: `tiktok_get_transcript` on the ad's `url` (1 credit), for the five best ads per competitor or the ten best in a category file. On NoData (1 credit, do not retry), find the same video on the advertiser's own account with `tiktok_get_videos` and `sort: "popular"` (1 credit a page), matched on the caption, and transcribe that post. When TikTok holds no transcript for it either, `ai_fallback: true` transcribes the audio (11 credits, videos up to two minutes). The first spoken sentence is the hook.
   - The landing page: `seo_get_page` on each distinct `destination_url` (free): `title`, `h1` and `h2` show the offer the page leads with.
4. **Group the creatives.** Once read, group ads with the same title or the same spoken hook into one creative and count its variants: many variants of one creative is a winner being scaled. In a category file, cap each advertiser at five creatives, then fill the file with the long-runners first and the recent ads marked as tests.
5. **Tag each ad** from the ad itself:
   - Angle: pain, outcome, social proof, comparison, founder story, demo, objection, offer-led.
   - Offer: free trial, discount, free shipping, bundle, guarantee, lead magnet, demo, none.
   - Proof: the number, review or testimonial the ad leans on, if any.
   - Format: `format` from the row, plus video length where the transcript or text shows it, and whether it looks like a creator talking to camera (UGC style) or a produced spot.
   - CTA: the `cta` field, or the call in the last line when it is null.
6. **Set the recent ones apart.** Ads first shown in the last 14 days are current tests. List them apart: they show where an advertiser is heading, not what works.
7. **Deliver**, with the reminder at the top that the library shows what runs, not what converts.
   - Competitors: a table per competitor: ad link, format, first shown, days running, still running, variants, title, spoken hook, angle, offer, CTA, landing page title, countries, and the published `impressions` and `spend` ranges where present. Under each table, five lines: how many ads run now, their three proven creatives and why they likely work, the angles they repeat, their core offer, and their current tests.
   - Category: the swipe file grouped by angle. Per ad: advertiser, library link, format, days running, still running, variants, spoken hook, offer, CTA, landing page title. Then a summary table: angle, ads, long-runners, advertisers using it, most common offer. Name the two or three angles with the most long-runners (proven in the category) and any angle no advertiser uses (an opening, or a dead end: test it small).
   - End with what the user can take from it.

## Judgment

- No ads found is not proof of no ads. Try another spelling, the parent brand, or a product's own advertiser name.
- On TikTok the spoken hook is the ad. An ad with no transcript and no text is a row with dates only: count it for survival, but do not guess its angle from the cover image.
- An agency, a reseller or a creator running ads for the competitor shows under its own name. Check `destination_url` before counting an ad as the competitor's.
- The same offer across every ad is the advertiser's core offer. An offer that appears in one ad only is a test.
- Never copy a competitor's creative, footage, claims or words. Take the structure: the hook pattern, the angle, the offer type.
- Proven angles are table stakes. The user's ads need them and something the category does not say yet; [write-ad-brief](../write-ad-brief/SKILL.md) adds that from customer language.
- Keyword search is ranked by the library and never complete. A second pass with other words costs little and is cached for 24 hours.
- Ads that look like organic posts often win on TikTok. Compare the long-runners with the top organic videos in the niche ([find-tiktok-hooks](../find-tiktok-hooks/SKILL.md)) before briefing a produced spot.
- Credits and handoff follow the [ad libraries notes](../create-paid-ads-plan/references/ad-libraries.md#credits): say the estimate first, pass `max_credits` on a budget, and never buy, launch or pause ads. The server keeps no state; a weekly rerun of a competitor read is [monitor-competitors](../monitor-competitors/SKILL.md).
- **Every platform at once.** When the user wants all of one competitor's ads, or a category's, across platforms, run [research-meta-ads](../research-meta-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md) and [research-linkedin-ads](../research-linkedin-ads/SKILL.md) in turn, then merge the tables by angle and offer. The same angle running on two platforms is a stronger signal than either alone. For a one-page view of every library, [tear-down-competitor](../tear-down-competitor/SKILL.md) reads one page from each.

## Related skills

- The same read on the other libraries: [research-meta-ads](../research-meta-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md).
- A brief for the user's own ads from these long-runners: [write-ad-brief](../write-ad-brief/SKILL.md).
- Where paid ads fit in the plan and what to spend: [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
- Hooks from organic TikTok videos: [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md).
- Creators who make the UGC-style ads in the file: [find-ugc-creators](../find-ugc-creators/SKILL.md).
- Everything about a competitor beyond its ads: [tear-down-competitor](../tear-down-competitor/SKILL.md).
- New ads from the same competitors flagged every week: [monitor-competitors](../monitor-competitors/SKILL.md).
