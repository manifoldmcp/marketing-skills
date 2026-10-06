---
name: plan-comparison-pages
description: When the user wants their own X vs Y and X alternatives pages to rank on Google. Finds which competitor comparison searches have volume, which comparison pages the site already has and where they rank, and what competitors publish for the same searches (including pages about the user), with DataForSEO, then hands back a list of vs, alternatives and defence pages to create or improve. Also use when the user mentions vs pages, an alternatives page, competitor comparison pages, rank for competitor alternatives, who ranks for us vs them, or a competitor's page comparing itself to us. Getting into other sites' best-of lists goes to find-best-of-lists, a sales battlecard to write-battlecard, competitor pricing and messaging facts to compare-messaging, bidding on competitor names in Google Ads to check-google-brand-bidding.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Comparison pages

People who search "<competitor> alternatives" or "<competitor> vs <other>" are close to buying and already know the category. This skill finds which of those searches have volume, which comparison pages the user already has and where they rank, and what competitors publish for the same searches, including pages about the user. It ends in a list of the user's own "X vs Y" and "X alternatives" pages to create or improve.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `seo_get_serp` (hosts often add a prefix, for example `mcp__manifold__seo_get_serp`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Then follow the tool checks in the [Google search notes](../create-seo-plan/references/platforms/google.md#tools): the `seo_*` tools are required, the `console_*` tools optional.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the product's name, the competitors and their domains, the differentiation, the market) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Site and product**: the user's domain and the product's name as people search it.
- **Competitors**: the products buyers compare the user with. Ask; `seo_get_serp_competitors` returns sites that share search results, which are often publishers rather than rivals. [find-competitors](../find-competitors/SKILL.md) answers it if the user does not know.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run for three competitors costs about 3 x 2 x 10 + 10 + 30 + 10 = 110 credits, plus the cost of [find-competitors](../find-competitors/SKILL.md) if the competitors need finding; `seo_get_page` is free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the searches.** For each competitor, `seo_search_keywords` twice (10 credits each): `seed: "<competitor> alternative"` and `seed: "<competitor> vs"`. The default mode keeps phrases that contain the seed, which catches "alternatives", "free alternative", "alternative for <use case>" and every "vs" pair people search. Then `seo_get_keyword_metrics` (10 credits for up to 100 phrases) on the phrases with the user's own name: "<product> vs <competitor>", "<product> alternatives", "<product> reviews". Small brands often get null or tiny volume there; that is normal.
2. **See what the site has.** `seo_get_ranked_keywords` on the site with `limit: 500` (30 credits). Keep the rows whose keyword holds a competitor's name, "vs", "alternative" or "compare". They show which comparison pages exist and rank, at which `rank`, and which `url`. Ask the user for comparison pages that exist but rank for nothing, and read them with `seo_get_page` (free).
3. **See who ranks.** `seo_get_serp` for the ten phrases with the most volume (1 credit each). Sort each page one into: vendors' own comparison pages, third-party lists and review sites (G2, Capterra, blogs), forum threads (`type: "discussions_and_forums_element"`), and videos. A vendor page in the top five proves a vendor page can rank for that phrase. Page ones held only by lists and review sites are harder for the user's own page, and those lists are worth getting into instead: that is [find-best-of-lists](../find-best-of-lists/SKILL.md).
4. **Read what competitors publish.** `seo_get_page` on each vendor comparison page in the results (free, rate limited): `title`, `h1`, `h2` (the criteria they compare on: price, features, migration, support, who it is for), `word_count` and `schema_types`. Look hardest at the SERPs for the user's own name: a competitor's "<product> alternatives" page ranking there is taking the user's buyers and needs an answer first.
5. **Get the facts right.** A comparison page stands on current prices, plans and features for each competitor. No tool here reads a page's body text; take those facts from [compare-messaging](../compare-messaging/SKILL.md), or from the user. Never guess a competitor's price.
6. **Deliver** a table: phrase, volume, KD, page type (vs page, alternatives page, or a defence page for the user's own name), the user's URL and rank now (or none), who holds the top three (vendor, list, review site, forum), the competitor pages to match (URL and the criteria in their headings), action (create, improve, leave), priority.

## Judgment

- Follow the [Google search notes](../create-seo-plan/references/platforms/google.md): the [keyword floors](../create-seo-plan/references/platforms/google.md#keyword-floors), the [credits](../create-seo-plan/references/platforms/google.md#credits) and the [handoff](../create-seo-plan/references/platforms/google.md#handoff).
- Start with the phrases where a vendor page already ranks and the user has no page. They are proven winnable.
- "<competitor> alternatives" pages rank best as honest lists that include other tools, not a single pitch. A "vs" page can be a straight two-way comparison.
- Be fair and dated. A wrong claim about a rival's price is the first thing a buyer checks, and comparative claims carry legal risk in some markets. Put "as of <month>" on prices and a source for each claim.
- One page per competitor pair or per "alternatives" phrase. Do not build separate pages for "alternative" and "alternatives".
- A competitor's comparison page cited in AI answers is a matter for [build-ai-citations](../build-ai-citations/SKILL.md); this skill is about Google's page one.

## Related skills

- Who the competitors are: [find-competitors](../find-competitors/SKILL.md). Their current prices, plans and messaging: [compare-messaging](../compare-messaging/SKILL.md).
- Other sites' "best X" and alternatives lists to get into: [find-best-of-lists](../find-best-of-lists/SKILL.md).
- A brief for each page to create: [write-seo-brief](../write-seo-brief/SKILL.md). An existing comparison page to improve in depth: [optimize-page](../optimize-page/SKILL.md).
- Competitor names in Google Ads: [check-google-brand-bidding](../check-google-brand-bidding/SKILL.md). A comparison for the sales team: [write-battlecard](../write-battlecard/SKILL.md).
