---
name: check-google-brand-bidding
description: When the user wants to know who bids on their brand name in Google. Prices the brand terms, fetches the Google results for each on desktop and mobile to see the paid rows and the user's own organic rank, reads the bidders' text ads in the Google ads library, sorts them into competitors, resellers and affiliates, and gives a defend, watch or leave verdict per term. Also use when the user mentions is anyone bidding on our brand, competitors' ads show when people google us, should we run a brand campaign, who is conquesting our name, brand keywords, or a competitor ad above our own result. A keyword list for search ads goes to find-google-ads-keywords, a competitor's ads in each library to research-google-ads, research-meta-ads, research-tiktok-ads or research-linkedin-ads, and a weekly check for new conquesting to monitor-competitors.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Brand bidding

Who pays to show an ad when someone searches the user's brand name on Google, what those ads say, and whether the user should bid on their own name to defend it. It ends in a verdict per brand term and a table of the bidders; buying the brand campaign stays with the user.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_serp` and `ads_get_advertiser_ads` (hosts often add a prefix, for example `mcp__manifold__seo_get_serp`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `ads_*` tools are not, the ads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, run the term pricing and the result pages on the `seo_*` tools, and mark the bidders' ads "not checked".

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand and product names, the site, the competitors and their domains, and the market) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Brand terms**: the name and its common variants, plus the searches that sit closest to a purchase: "<brand> pricing", "<brand> reviews", "<brand> alternative", "<brand> vs". Default: the name and those four.
- **Site**: the user's domain, to find their own organic result on each page.
- **Market**: `location` and `language` if not the United States and English, and the `country` for the ad library.
- **Brand campaign**: whether the user already bids on the name, and what it costs a month. It changes the question from "should we defend" to "is it still needed".
- **Budget**: a default run costs about 6 + 5 x 2 + 3 + 3 x 3 = 28 credits: metrics on five terms, each page on desktop and mobile, and three bidders' Google ads with three read in full each. Add 1 per bidder whose domain finds nothing in the library. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Price the brand terms.** `seo_get_keyword_metrics` with the brand terms (about 6 credits): `volume`, `cpc` and `competition`. A `cpc` on the bare name means advertisers have bid on it; `competition` near zero means few do.
2. **See who shows up.** `seo_get_serp` on each brand term, once with `device: "desktop"` and once with `device: "mobile"` (1 credit each). Rows with `type: "paid"` are the ads on that fetch: note each advertiser's `domain`, `title` and `snippet`. From the same rows, note the user's own `organic_rank` and any other organic row about the brand (a competitor's "vs" page, a review site, a reseller). One fetch is one auction: a term with no paid rows is not clean until a fetch on another day (the 24-hour cache returns the same page within a day) also shows none.
3. **Read the bidders' ads.** For each advertiser from step 2, `ads_get_advertiser_ads` with `platform: "google"` and `country`, resolving the advertiser as [the libraries](../create-paid-ads-plan/references/ad-libraries.md#the-libraries) set out (1 credit a page). The library does not say which keywords an ad bids on, so read the three longest-running text ads with `ads_get_ad` on the row's `url` (1 credit each) and keep those whose `headline` or `body` names the user's brand or says "alternative". Days running follows the [winner rules](../create-paid-ads-plan/references/ad-libraries.md#winners): a conquesting ad running 90 days or more pays for the bidder. `seo_get_page` on its `destination_url` (free) shows whether it lands on a comparison page.
4. **Sort the bidders.** Competitors conquesting the name; resellers, affiliates and partners (check `destination_url`: an affiliate bidding on the brand often breaks the user's affiliate terms); and the user's own ads.
5. **Decide per term.** Defend (bid on it) when a competitor's ad shows on it, or when the user's organic result is not first. Leave it when the page shows only the user: the organic result already takes those clicks. The monthly cost is at most `volume` x `cpc`, and in practice a fraction of that, since most searchers still click the organic result.
6. **Deliver** two tables. Terms: term, `volume`, `cpc`, `competition`, advertisers seen (desktop, mobile), the user's organic rank, other organic rows about the brand, verdict (defend, watch, leave). Bidders: advertiser, kind (competitor, reseller, affiliate), the ad's headline and body, days running, still running, landing page title, whether the text names the brand. End with the user's actions: the brand campaign, an answer to each conquesting angle in the user's own ad, and a trademark complaint to Google where an ad's text uses the user's registered mark. Nothing is bid or filed from here.

## Judgment

- Ads rotate by auction, location, device and hour. Two fetches on different days on both devices is the floor for calling a term clean.
- Bidding on a competitor's name is allowed in Google Ads; using its trademark in the ad text can be restricted after a complaint. The user files that complaint, and only for a registered mark.
- A brand campaign usually costs little, because the user's ad is the most relevant one on the page. Its value is the clicks it keeps from the conquester, not the clicks it adds; where no competitor bids, a test pause in the user's account settles whether it is needed.
- Do not answer a conquester by bidding on their name by reflex. Competitor terms cost more and convert worse, as [find-google-ads-keywords](../find-google-ads-keywords/SKILL.md) says.
- A competitor's organic "<brand> alternative" or "vs" page is conquesting without ads. A comparison page of the user's own is [plan-comparison-pages](../plan-comparison-pages/SKILL.md).
- Conquesting starts and stops. To catch the day it starts, [monitor-competitors](../monitor-competitors/SKILL.md) reruns step 2 weekly.
- Credits and handoff follow the [ad libraries notes](../create-paid-ads-plan/references/ad-libraries.md#credits): say the estimate first, pass `max_credits` on a budget, and never buy or edit ads.

## Related skills

- The full keyword list for search ads: [find-google-ads-keywords](../find-google-ads-keywords/SKILL.md).
- A conquesting competitor's ads in every library: [research-google-ads](../research-google-ads/SKILL.md), [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md) or [research-linkedin-ads](../research-linkedin-ads/SKILL.md).
- The user's own "<brand> vs <competitor>" pages: [plan-comparison-pages](../plan-comparison-pages/SKILL.md).
- A weekly check for new bidders: [monitor-competitors](../monitor-competitors/SKILL.md).
