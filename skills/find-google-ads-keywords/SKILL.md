---
name: find-google-ads-keywords
description: When the user wants keywords for Google search ads. Builds a PPC keyword list from seed keywords with CPC, volume, competition, intent and trend, groups it into ad groups fitted to the monthly budget, confirms it with Keyword Planner figures, shows who bids on each term and what competitors' Google search ads say, and adds a negative keyword list. Also use when the user mentions PPC keywords, a Google Ads keyword list, what keywords to bid on, negative keywords, how much a click costs, CPC, SEM or paid search keywords. Who bids on the user's own brand name goes to check-google-brand-bidding, which platforms to spend on to create-paid-ads-plan, and keywords to rank for organically to create-seo-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# PPC keywords

A keyword list for search ads: the terms worth paying for, what a click costs, how they group into ad groups, the negatives that keep the wrong clicks out, and what competitors' Google ads say. It ends in tables the user loads into Google Ads; nothing here bids or builds the campaign.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `seo_get_keyword_metrics` (hosts often add a prefix, for example `mcp__manifold__seo_search_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the `ads_*` tools are missing, the ads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so; this skill still runs on the `seo_*` tools without the competitor ads step.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product, the site and its pages, the ICP, the competitors and their domains, the market and the ad budget) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Product and site**: what is sold and the pages ads can send people to.
- **Seeds**: three to five: the category ("payroll software"), the problem ("run payroll"), the audience ("payroll for restaurants"), and competitor names if the user wants to bid on them.
- **Market**: `location` and `language` if not the United States and English. For a local business, the city goes in the keyword ("plumber austin"), and the campaign's location targeting does the rest.
- **Ad budget**: monthly spend. It sets how many keywords the account can afford (step 3).
- **Competitors**: two or three whose Google ads to read. Default: the advertisers that show up in step 5.
- **Budget**: a default run costs about 3 x 10 + 24 + 10 + 3 x 11 = 97 credits: three seeds, Keyword Planner volume on the shortlist, ten result pages checked, and three competitors' Google ads (one list page and ten ads read each). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pull candidates.** `seo_search_keywords` with each seed (10 credits for 100 rows). The default `mode` keeps the seed in every keyword; `mode: "related"` adds the words buyers use without it. Read `volume`, `cpc` (USD), `competition` (0 to 1, how crowded the auction is), `intent` and `trend`.
2. **Sort by intent.** Keep `intent` transactional and commercial, and informational terms only with a buying modifier (price, pricing, software, tool, service, near me, best, vs, alternative, for a named audience). Group what is kept into ad groups of five to twenty close terms: brand (the user's name), competitor (other brands' names), category, audience ("for restaurants") and problem. One ad group is one message and one landing page.
3. **Fit the list to the budget.** Monthly spend divided by average `cpc` is the clicks the account can buy; times the expected conversion rate (ask; default 3%, a rule of thumb) is the conversions. Smart bidding steers on conversions per campaign, not clicks per ad group: under about 30 a month (a rule of thumb), plan one campaign of two to four ad groups on exact and phrase match with manual or capped bids, and cut to the ad groups the budget can feed, highest intent first. Broad match waits until conversion tracking clears that bar. Terms under about 20 searches a month rarely serve (Google marks them low search volume); keep them only inside an ad group that has bigger terms.
4. **Confirm with Keyword Planner.** `seo_get_keyword_metrics` with the shortlist and `source: "google_ads"` (24 credits flat, up to 1,000 keywords): Google's own volume, `cpc` and `competition`, the figures Google Ads will show the user. `kd` and `intent` are null in this source, and keywords over 80 characters come back empty. This is the one case where metrics follow `seo_search_keywords`: it is a different source.
5. **See who bids.** `seo_get_serp` on the ten biggest kept keywords (1 credit each). Rows with `type: "paid"`, when Google showed ads on that fetch, name the advertisers bidding on that term; one fetch is one moment, so a term with no paid rows may still have bidders. Competitors bidding on the user's brand name make brand terms worth defending.
6. **Read competitors' Google ads.** For each competitor, `ads_get_advertiser_ads` with `platform: "google"` and `country`, resolving the advertiser as [the libraries](../create-paid-ads-plan/references/ad-libraries.md#the-libraries) set out (1 credit a page). Text ads are search ads. Read the ten longest-running with `ads_get_ad` on the row's `url` (1 credit each): `headline`, `body` and `destination_url`. The library does not say which keywords an ad bids on; match headlines to ad groups by their words. `seo_get_page` on the landing pages (free) shows which page each competitor sends search clicks to.
7. **Build the negatives.** From the candidate rows the user would never want to pay for: job and salary terms, free, course, template, login, what is, how to (unless the user sells the answer), cheap (for a premium product), other meanings of the seed, and competitor names if the user will not bid on them. Take each from the actual list, not from memory, so every negative blocks something real.
8. **Deliver** three tables. Keywords: ad group, keyword, suggested match type (exact for the highest intent, phrase for the rest), Keyword Planner volume, `cpc`, `competition`, intent, trend (rising, flat, falling), who bids (from step 5), and the landing page on the user's site. Negatives: term, match type, reason. Competitor search ads: advertiser, headline, description, landing page, first shown, days running. Add the monthly clicks the budget buys at the list's average `cpc`.

## Judgment

- `cpc` is an average from past auctions. The user's real price depends on their ad's quality and who bids that day; treat it as a range, not a quote.
- Bid on the user's own brand when competitors bid on it or the organic result is not first; [check-google-brand-bidding](../check-google-brand-bidding/SKILL.md) checks both. Otherwise the organic listing already takes those clicks.
- Competitor terms cost more and convert worse than category terms. Google lets advertisers bid on a rival's name but can block the trademark in the ad text; the ad has to say why the user is different without it.
- High-volume informational terms rarely pay back in search ads at a small budget. They belong to content; hand them to [create-seo-plan](../create-seo-plan/SKILL.md).
- `competition` is Google's measure of how many advertisers bid, not keyword difficulty. `kd` is for organic ranking and says nothing about the auction.
- `trend` shows seasonality. Raise spend into the months where volume peaks rather than spreading it evenly.
- Days running for competitors' ads follows the [winner rules](../create-paid-ads-plan/references/ad-libraries.md#winners). Credits and handoff follow the [ad libraries notes](../create-paid-ads-plan/references/ad-libraries.md#credits): nothing here bids, builds or launches a campaign.

## Related skills

- Who bids on the user's brand name, and whether to defend it: [check-google-brand-bidding](../check-google-brand-bidding/SKILL.md).
- Competitors' ads across every library: [research-google-ads](../research-google-ads/SKILL.md), [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md) or [research-linkedin-ads](../research-linkedin-ads/SKILL.md).
- Whether to start with search at all, and the 90-day plan: [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
- Organic keywords and content that ranks: [create-seo-plan](../create-seo-plan/SKILL.md).
