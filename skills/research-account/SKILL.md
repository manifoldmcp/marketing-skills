---
name: research-account
description: When the user wants one company researched before a sales call. Pulls the company record, the buying committee by title, recent LinkedIn posts and funding news, the ads it runs on Facebook, LinkedIn and Google, and its search footprint, into a one-page brief with the evidence linked and three questions for the call. Also use when the user mentions prep me for a call with, research this account, account research, account brief, call prep, who is on the buying committee at, or brief me before the demo. A prospect client researched for an agency pitch goes to prepare-client-pitch, a competitor researched as a rival to tear-down-competitor, accounts showing buying signals to find-buying-signals, a list of contacts to build-lead-list.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Account brief

One company, researched before a sales call: what it is, who sits on the buying committee, what the company and those people said lately, what it is paying to advertise, and how it shows up in search. It ends in a one-page brief with the evidence linked and three questions for the call.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_get_company` and `linkedin_get_company_posts` (hosts often add a prefix, for example `mcp__manifold__leads_get_company`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it this skill cannot find the buying committee; it can still run its LinkedIn, ads and search steps.
- This skill also reads `linkedin_*`, `ads_*` and `seo_*`. If one of those groups is switched off, skip the steps that need it and say which signal is missing from the result.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (what the user sells, the personas and job titles of the buying committee) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Account**: the company domain.
- **People on the call**: names and titles, or LinkedIn URLs. Default: the likely buyer found in step 2.
- **The call**: discovery, demo or negotiation, and what the user sells. It decides which facts matter.
- **Budget**: a default brief costs about 1 + 1 + 4 + 1 + 1 + 3 x 1 + 3 x 1 + 3 + 5 = 22 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Company.** `leads_get_company` on the domain (1 credit): `description`, `industry`, `employees`, `location`, `founded_year`, `keywords[]` (its LinkedIn specialties) and the LinkedIn page as `linkedin_url`. `linkedin_get_company` with that `url` (1 credit) adds `followers` and LinkedIn's `bio`. On `NoData` the provider does not know the domain: take what the company does from `seo_get_page` on the home page (0 credits, its `title` and `meta_description`) and ask the user for the LinkedIn page.
2. **Buying committee.** `leads_search_people` with `company_domains` set to the domain and the full titles around the deal: the economic buyer, the likely champion, the user of the product, and whoever signs off on security or procurement (4 credits). The rows give name, `title`, `location` and `linkedin_url`. For a person on the call the search missed, `leads_get_person` with their `linkedin_url`, or `first_name`, `last_name` and `domain` (3 credits when matched, 1 on `NoData`), which adds the `headline`. The [search rules](../create-outbound-plan/references/lead-data.md#searches-and-counts) explain how titles match.
3. **What they said lately.** `linkedin_get_company_posts` with the company `url` (1 credit a page): launches, funding, new markets, hires and events in the last 90 days. `seo_get_serp` with `"<company name>" raises` as `keyword` (1 credit): a news result in the top 10 names and dates a round. For each person on the call, `linkedin_get_profile` (1 credit) for their `bio`, and `linkedin_search_posts` with their full name as `query` and `since: "month"` (1 credit), keeping only rows whose `author` is their profile handle. No tool lists one person's feed, so this finds some of their posts, not all. If they post on X, `twitter_get_tweets` with their handle (1 credit).
4. **Ads.** `ads_get_advertiser_ads` three times (1 credit a page each): `platform: "facebook"` with `active_only: true` and `platform: "linkedin"` with the company name as `advertiser`, and `platform: "google"` with the domain. Only Facebook reports `active`; on LinkedIn and Google read `last_shown`. If the Google call finds nothing, `ads_search_advertisers` with the brand (1 credit) gives the advertiser id to try instead. What a company pays to promote is what it is trying to grow this quarter.
5. **Search footprint.** `seo_get_domain_overview` on the domain (5 credits): `organic_traffic`, `organic_keywords`, `domain_rank` and `top_pages[]`. Keep it to one line unless the user sells marketing or search, in which case `seo_get_ranked_keywords` with `limit: 100` (10 credits) shows what they rank for.
6. **Deliver** a one-page brief:
   - A company snapshot: size, location, what they do in one sentence, the latest round if one was found.
   - A committee table: name, title, role in the deal, background (headline or bio), LinkedIn URL.
   - Recent signals with dates and links: posts, the round, active ads.
   - Search footprint in one line.
   - Three questions for the call and one angle, each tied to a piece of evidence above.

## Judgment

- Check the current role before the call. The search row's `title` and `company_domain` and the LinkedIn profile agree most of the time; when they disagree, trust the profile and say so.
- Records lag. A company record can be a month old and a headcount band a quarter behind. Date anything that decides the pitch.
- The data has no tech stack, revenue or tenure. Ask about them on the call rather than guess.
- Stay on work. A brief that mentions someone's family, health or private accounts does harm on the call; leave that out even when it is public. The [personal data](../create-outbound-plan/references/lead-data.md#personal-data) rules apply.
- LinkedIn post dates are approximate ("3 weeks ago"). Say "recently", not a date, when the age is in weeks.
- A brief on a prospect client for an agency pitch, centred on their marketing, is [prepare-client-pitch](../prepare-client-pitch/SKILL.md). A competitor researched as a rival is [tear-down-competitor](../tear-down-competitor/SKILL.md).

## Related skills

- Many accounts checked for a reason to buy now, rather than one researched in depth: [find-buying-signals](../find-buying-signals/SKILL.md).
- Emails for the buying committee: [build-lead-list](../build-lead-list/SKILL.md), from its reveal step. An opening line for each of them: [write-first-lines](../write-first-lines/SKILL.md).
- The account's ads in depth: [research-linkedin-ads](../research-linkedin-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md), [research-meta-ads](../research-meta-ads/SKILL.md) or [research-tiktok-ads](../research-tiktok-ads/SKILL.md).
- Research on a prospect client for an agency pitch: [prepare-client-pitch](../prepare-client-pitch/SKILL.md). A rival, not a buyer: [tear-down-competitor](../tear-down-competitor/SKILL.md).
