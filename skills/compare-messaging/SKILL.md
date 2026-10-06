---
name: compare-messaging
description: When the user wants to compare how competitors describe themselves and what they charge. Reads each competitor's homepage and pricing page headings, its active Meta, LinkedIn and Google ads, its bios and how ChatGPT and Gemini describe it, and lays out promise, buyer, proof, offer and pricing model side by side, with the claims everyone makes and the ones nobody makes. Also use when the user mentions competitor messaging, how does X pitch itself, competitor value props, taglines or headlines, competitor pricing tiers or packaging side by side, or what our competitors charge. Turning the differences into positioning options goes to find-positioning, a sales card against one rival to write-battlecard, and comparison pages to plan-comparison-pages.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Competitor messaging and pricing

How each competitor describes itself and what it charges, side by side: the promise it leads with, the buyer it names, the proof it leans on, the offer it pays to advertise, and its pricing model. The messaging comes from the tools; the prices come only from the pricing page itself, read by the host or pasted by the user. It ends in the table that [find-positioning](../find-positioning/SKILL.md) and [plan-comparison-pages](../plan-comparison-pages/SKILL.md) build on.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_page` and `ads_get_advertiser_ads` (hosts often add a prefix, for example `mcp__manifold__seo_get_page`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- This skill reads several tool groups: `seo_*`, `ads_*`, `aeo_*`, `leads_*` and the platform tools (`linkedin_*`, `instagram_*`, `tiktok_*`, `twitter_*`, `youtube_*`). If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run without them, and mark their columns "not checked" rather than leaving them empty.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the user's own site, the competitors and their domains, the pricing and the market) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Competitors**: two to five domains. Default: the direct competitors from [find-competitors](../find-competitors/SKILL.md).
- **The user's site**: include it as the last row, so the gaps show.
- **Pages**: the homepage and the pricing page (default `/pricing`; try `/plans` if that fails), plus one landing page per competitor if the user names one.
- **Pricing text**: whether the host can open web pages (a browser or fetch tool), or whether the user will paste each pricing page. Ask before step 2, since no manifold tool reads prices.
- **Market**: `country` for the ad libraries if not the United States.
- **Budget**: a default run costs about 4 x (3 + 3 + 1 + 2 + 1 + 4) = 56 credits for four competitors, and 14 more for the user's own row; the pages are free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the pages.** `seo_get_page` (free, rate limited) on each homepage and pricing page. Take `title`, `meta_description`, `h1[]` and `h2[]`. The `h1` is the promise, the meta description the one-sentence pitch, and the `h2[]` the proof and the feature pillars. On the pricing page, the headings often name the plans (Free, Starter, Pro, Enterprise): that is the packaging, not the price. A `status` of 404, or a `final_url` on a demo or contact page, means there is no public pricing.
2. **Read the prices.** No manifold tool reads page body text or prices. If the host has a browser or fetch tool, open each pricing page and take the plan names, the price of each, the billing unit (per seat, per usage, flat), the free plan or trial, the limit or feature that separates the tiers, and the annual discount. Otherwise ask the user to paste the pages. If neither is possible, fill the price columns with "not read"; never estimate a price from memory, from an ad or from a review site.
3. **Read the ads.** `ads_get_advertiser_ads` with `active_only: true` on `facebook` (the brand name), `linkedin` (the company name) and `google` (the domain), 1 credit a page each. Only Facebook applies `active_only`; LinkedIn and Google rows carry `active: null`, so keep those whose `last_shown` falls in the last 30 days. Read `headline`, `body`, `cta` and `destination_url`: ads carry the offer the competitor pays to test (a free trial, a demo, a discount, a template) and the angle (a pain, an outcome, a comparison). Google rows carry no creative text without `details: true` (25 credits), so call `ads_get_ad` with the ad URL (1 credit each) on the two or three that have run longest instead. The full ad teardown is [research-meta-ads](../research-meta-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md).
4. **Read the bios.** `linkedin_get_company` (1 credit) for its `bio`; `leads_get_company` (1 credit) for `description` and `keywords[]` (its LinkedIn specialties); and the profile of the one or two social accounts the competitor uses most (1 credit each: `instagram_get_profile`, `tiktok_get_profile`, `twitter_get_profile` or `youtube_get_channel`) for their `bio`. A bio is the shortest statement of positioning a company writes.
5. **Read how engines describe them.** `aeo_run_ai_answers` with one prompt per competitor, "What is <competitor> and who is it best for?" (up to 10 prompts), `engines: ["chatgpt", "gemini"]` and `brands` holding every competitor and the user (4 credits per prompt), then `get_task` (free) after `poll_after_s`. Read each `answer` for the category, the buyer and the price level the engines state. Where that differs from the competitor's own `h1`, buyers hear a different pitch from the one it writes.
6. **Code each competitor.** From steps 1 to 5, fill in: the category word it uses for itself, the buyer it names, the main promise, the proof (numbers, customer logos, awards in the headings), the main call to action (trial, demo, sign up, contact sales), and the offer in its ads. Mark the words a competitor repeats across its page, ads and bios: that is the message it has committed to. Then list the claims every competitor makes (table stakes) and the claims nobody makes.
7. **Deliver** a table with one row per competitor and the user last: category claimed, buyer named, how engines describe it, promise (the `h1`, quoted), proof, call to action, offer in ads, pricing model (free plan, trial, billing unit, entry price) with the source of each price (read by the host, pasted, or "not read") and the date, and the URL of each page read. Below it, two lists: the table-stakes claims, and the open claims no competitor makes.

## Judgment

- When an `h1` is generic ("Work better, together"), the `h2[]` and the meta description carry the real claim. Quote those instead.
- The homepage is the brand's promise; the ads are the offers it is testing. An ad still running after 90 days is most likely the message that converts for them, and it outranks the homepage as evidence.
- Hidden pricing is a finding, not a gap: it means a sales-led model with a demo call before any price. Say so; do not guess the number.
- Compare prices on one example, not across units: the price for the user's typical customer (say, a 10-seat team or 5,000 contacts a month) on each competitor's plan that fits.
- Prices change. Date every price, and never carry one over from an earlier conversation or a model's memory.
- `seo_get_page` fetches the raw page. A site behind a bot wall, or one that renders its headings with JavaScript, can come back with empty headings; the host's browser or the user's paste is the fallback.
- Quote short. A headline or a tagline is evidence, with its URL; a copied paragraph is not needed. Every other claim carries its tool and field, or the URL.
- Two headcounts or bios can disagree (`leads_get_company` against `linkedin_get_company`); report both.
- Credits: say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Ad lists and profiles are cached 24 hours, company records 30 days.
- Never contact anyone. After delivering, offer to write the competitors' claims and pricing into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Related skills

- Positioning options built from this table: [find-positioning](../find-positioning/SKILL.md).
- "X vs Y" and "X alternatives" pages: [plan-comparison-pages](../plan-comparison-pages/SKILL.md).
- A one-page card for sales against one competitor: [write-battlecard](../write-battlecard/SKILL.md).
- The competitors' ads in full: [research-meta-ads](../research-meta-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md).
- A competitor's pricing or homepage claims flagged when they change: [monitor-competitors](../monitor-competitors/SKILL.md).
