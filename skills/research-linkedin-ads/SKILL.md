---
name: research-linkedin-ads
description: When the user wants to research ads on LinkedIn, for named competitors or across a whole B2B category. Reads the LinkedIn ad library by company or keyword, ranks the ads by how long they have run, reads the headline, body, offer and landing page of the long-runners, sets recent tests apart, and hands back a table per competitor or a swipe file grouped by angle and offer. Also use when the user mentions the LinkedIn ad library, LinkedIn ads or sponsored posts, is a competitor advertising on LinkedIn, a swipe file of LinkedIn ads, or the lead magnets and demo offers a B2B category pays for. Meta, TikTok and Google ads go to research-meta-ads, research-tiktok-ads and research-google-ads, organic LinkedIn posts to find-linkedin-post-formats, a brief for new ads to write-ad-brief, a paid ads plan to create-paid-ads-plan, and a weekly watch of new ads to monitor-competitors.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Research LinkedIn ads

What B2B advertisers pay to show on LinkedIn, read from LinkedIn's ad library: for one or more named competitors, or for a whole category by keyword so it catches advertisers the user never named. Each ad comes with its dates, headline, offer and the page it sends people to. It ends in a table per competitor or a swipe file grouped by angle, long-runners first, because an ad that keeps running is the closest thing the library has to a result.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `ads_get_advertiser_ads`, `ads_search_ads` and `ads_get_ad` (hosts often add a prefix, for example `mcp__manifold__ads_get_advertiser_ads`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `ads_*` tools are not, the ads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so; this skill needs it.
- Landing pages use `seo_get_page`. If the seo group is off, skip that step, say so, and work from the ad text.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the competitors, their LinkedIn company names and domains, the category and its words, the buyer's job titles, and the market) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **The job**: named competitors' ads, or the best ads across a category (a swipe file). Take it from the request; a request for both runs the competitors first and the category around them second.
- **Competitors**: one to five, each with its LinkedIn company name or id and its domain. A competitor that plainly sells to consumers rarely runs LinkedIn ads: say so and offer [research-meta-ads](../research-meta-ads/SKILL.md) instead.
- **Keywords** (category job): three to five words the category's ads use: the product ("payroll software"), the problem ("month-end close"), the offer ("free template", "book a demo") and the leading brand names. Ask for two; add the rest from the first pull.
- **Market**: default every country. Pass `country` (a two-letter code) when one market matters.
- **Depth**: competitors, up to three pages each and the ten longest-running creatives read in full. Category, 30 ads in the file.
- **Budget**: LinkedIn is the cheapest library to read, because the list rows already carry the text. Say it before starting; pass `max_credits` if the user gave a budget.
  - Competitors: about 3 credits each, one per list page. Three competitors cost about 9.
  - Category: about 8 credits: four keywords at two list pages each.
  - Add 1 credit for each ad read with `ads_get_ad` where a row has no text.

## Steps

1. **Pull the ads.** Both tools take `platform: "linkedin"`, 1 credit a page, with `country` when one market matters.
   - Competitors: `ads_get_advertiser_ads` with the company name or id as `advertiser`, paging with `meta.cursor` up to three pages while new rows come back.
   - Category: `ads_search_ads` for each keyword, two pages. Keyword search matches the ad's text, so a keyword with two meanings brings in other categories: drop those rows.
   - `active_only` has no effect here: LinkedIn does not mark an ad as running.
2. **Rank by survival.** Dedupe on `id`, then group ads with the same body or headline into one creative and count its variants: many variants of one creative is a winner being scaled. Work out days running for each as the [winner rules](../create-paid-ads-plan/references/ad-libraries.md#winners) set out: `active` is null on LinkedIn, so still running means `last_shown` within the last 7 days, and days running is `last_shown` minus `first_shown`, or today minus `first_shown` while it runs. When a row gives neither date, days running is unknown: say so rather than guess. Sort by days running. In a category file, cap each advertiser at five creatives, then fill the file with the long-runners first and the recent ads marked as tests.
3. **Read the long-runners.** Follow [reading an ad](../create-paid-ads-plan/references/ad-libraries.md#reading-an-ad), on each competitor's ten longest-running creatives or every ad in the category file:
   - The text: list rows already carry `headline`, `body`, `cta` and `destination_url`. Where one is missing, `ads_get_ad` with the row's `url` as `id` (1 credit). The hook is the first line of the body.
   - Video ads: LinkedIn video has no transcript route here; work from the text.
   - The landing page: `seo_get_page` on each distinct `destination_url` (free): `title`, `h1` and `h2` show the offer the page leads with, and whether it is gated behind a form.
4. **Tag each ad** from the ad itself:
   - Angle: pain, outcome, social proof, comparison, founder story, demo, objection, offer-led.
   - Offer: free trial, demo, lead magnet (report, template, guide, webinar), discount, guarantee, none.
   - Proof: the number, customer logo, analyst mention or testimonial the ad leans on, if any.
   - Audience: the job title or team the hook calls out, if any.
   - Format: `format` from the row, in the library's own words (image, video, text and others).
   - CTA: the `cta` field, or the call in the last line when it is null.
5. **Set the recent ones apart.** Ads first shown in the last 14 days are current tests. List them apart: they show where an advertiser is heading, not what works.
6. **Deliver**, with the reminder at the top that the library shows what runs, not what converts.
   - Competitors: a table per competitor: ad link, format, first shown, days running, still running, variants, headline, hook, audience, angle, offer, CTA, landing page title, and the published `impressions` and `spend` ranges where present. Under each table, five lines: how many ads run now, their three proven creatives and why they likely work, the job titles they speak to, their core offer, and their current tests.
   - Category: the swipe file grouped by angle. Per ad: advertiser, library link, format, days running, still running, variants, hook, audience, offer, CTA, landing page title. Then a summary table: angle, ads, long-runners, advertisers using it, most common offer. Name the two or three angles with the most long-runners (proven in the category) and any angle no advertiser uses (an opening, or a dead end: test it small).
   - End with what the user can take from it.

## Judgment

- No ads found is not proof of no ads. Try another spelling, the company id, the parent company, or a product's own page.
- An agency or a reseller running ads for the competitor shows under its own name. Check `destination_url` before counting an ad as the competitor's.
- LinkedIn ads lead with a job-title hook and a proof point, where Meta ads in the same category show the problem in the first seconds. Keep them in separate files when the user wants both.
- The same offer across every ad is the advertiser's core offer. A gated report or a demo offer that keeps running says what the category's buyers trade their details for.
- Never copy a competitor's creative, claims or words. Take the structure: the hook pattern, the angle, the offer type.
- Proven angles are table stakes. The user's ads need them and something the category does not say yet; [write-ad-brief](../write-ad-brief/SKILL.md) adds that from customer language.
- Keyword search is ranked by the library and never complete. A second pass with other words costs little and is cached for 24 hours.
- Credits and handoff follow the [ad libraries notes](../create-paid-ads-plan/references/ad-libraries.md#credits): say the estimate first, pass `max_credits` on a budget, and never buy, launch or pause ads. The server keeps no state; a weekly rerun of a competitor read is [monitor-competitors](../monitor-competitors/SKILL.md).
- **Every platform at once.** When the user wants all of one competitor's ads, or a category's, across platforms, run [research-meta-ads](../research-meta-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md) and [research-linkedin-ads](../research-linkedin-ads/SKILL.md) in turn, then merge the tables by angle and offer. The same angle running on two platforms is a stronger signal than either alone. For a one-page view of every library, [tear-down-competitor](../tear-down-competitor/SKILL.md) reads one page from each.

## Related skills

- The same read on the other libraries: [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md).
- A brief for the user's own ads from these long-runners: [write-ad-brief](../write-ad-brief/SKILL.md).
- Where paid ads fit in the plan and what to spend: [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
- The organic post formats and hooks that work on LinkedIn: [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md).
- A competitor's LinkedIn page beyond its ads: [audit-linkedin-page](../audit-linkedin-page/SKILL.md).
- Everything about a competitor beyond its ads: [tear-down-competitor](../tear-down-competitor/SKILL.md).
- New ads from the same competitors flagged every week: [monitor-competitors](../monitor-competitors/SKILL.md).
