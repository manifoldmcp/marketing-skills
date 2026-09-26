---
name: seo
description: Google organic search with the manifold tools. Plans an SEO strategy, finds quick wins in keywords ranking 4 to 20, diagnoses a traffic drop, picks decayed pages to refresh, runs a technical audit, plans blog topics and clusters, writes a content brief for one keyword, optimizes one existing page, plans X vs Y and alternatives pages, protects rankings through a site migration, and checks where a site ranks today. Use when the user asks for SEO, organic traffic, keyword research, a keyword gap, low-hanging fruit, striking distance keywords, a traffic or rankings drop, a Google update, content decay, updating old posts, a technical SEO audit, crawl errors, a content plan, topic clusters, what to blog about, a content brief, on-page SEO, comparison or alternatives pages, a domain change, redesign or replatform, a redirect map, Search Console, or where we rank on Google. Rank tracking over time, backlinks and AI answers are other groups; editing or publishing the site is out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# SEO

Google organic search, technical and content together, so that "why did traffic drop" has one home. Every job reads the same data: what the site ranks for, what Google shows on page one, and what the pages carry. The playbooks differ in which pages and keywords they look at, and what they hand back.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_serp` and `seo_get_ranked_keywords` (hosts often add a prefix, for example `mcp__manifold__seo_get_serp`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `seo_*` tools are not, the SEO group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: every playbook here needs it.
- The `console_*` tools (the user's own Google Search Console and Bing Webmaster data) are optional, and most accounts do not have them yet. Their absence never blocks a playbook; see [Search Console](#search-console).

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. A request for a plan, or "help with our SEO" with no job named, opens strategy. A new page for a keyword is a brief; an existing URL is optimize a page; many pages at once are quick wins or content refresh.

| Job | The user says | Open |
|---|---|---|
| SEO strategy: where organic traffic should come from, and a 90-day plan | "SEO strategy", "SEO plan for next quarter", "SEO roadmap", "how do we get more organic traffic", "where should our SEO effort go" | [references/strategy.md](references/strategy.md) |
| Quick wins: keywords that already rank 4 to 20, and the title, meta and internal link fixes that lift them | "SEO quick wins", "low-hanging fruit", "striking distance keywords", "almost on page one", "what can we fix this week" | [references/quick-wins.md](references/quick-wins.md) |
| Traffic drop: when and where organic traffic fell, and why | "our traffic dropped", "we lost rankings", "was it a Google update", "organic clicks are down", "why did this page fall" | [references/traffic-drop.md](references/traffic-drop.md) |
| Content refresh: existing pages that decayed or sit on page two, what outranks them and what to add | "which old posts should we update", "content decay", "refresh our blog", "pages that used to rank", "prune old content" | [references/content-refresh.md](references/content-refresh.md) |
| Audit: a technical crawl of the site, issues ranked by impact | "technical SEO audit", "crawl our site", "broken links and 404s", "duplicate titles", "indexing problems" | [references/audit.md](references/audit.md) |
| Content plan: blog topics and keyword clusters, in the order to write them | "content plan", "what should we blog about", "topic clusters", "keyword research for our blog", "pillar pages" | [references/content-plan.md](references/content-plan.md) |
| Brief: one new page for one keyword, built from what ranks | "content brief for X", "outline an article on X", "what should a page on X cover", "SEO brief for our writer" | [references/brief.md](references/brief.md) |
| Optimize a page: one existing URL against its keyword | "optimize this page", "on-page SEO for this URL", "why does this page rank 12th", "improve our pricing page for X" | [references/optimize-page.md](references/optimize-page.md) |
| Comparison pages: the user's own "X vs Y" and "X alternatives" pages | "vs pages", "alternatives page", "competitor comparison pages", "rank for competitor alternatives", "who ranks for us vs them" | [references/comparison-pages.md](references/comparison-pages.md) |
| Migration: keep rankings through a domain change, redesign or replatform, before and after launch | "site migration", "we're changing domains", "redirect map", "moving to Webflow", "check the redirects after launch" | [references/migration.md](references/migration.md) |
| Rank check: where a site ranks now, once | "where do we rank for X", "are we on page one", "check our Google position", "who ranks above us" | [references/rank-check.md](references/rank-check.md) |

## Shared rules

### Search Console

Search Console holds the user's real clicks, impressions, CTR and average position, plus index status and sitemaps. The playbooks that are much better with it (quick wins, traffic drop, content refresh, optimize a page, rank check on the user's own site, and the migration after-check) use it when it is there and run in full without it.

- **With it.** If the `console_*` tools are present, call `console_list_properties` first (free). Pick the property that covers the site: `sc-domain:example.com` covers every host and protocol, a URL property only that prefix. If it returns `NotConnected`, give the user its `connect_url` once, then carry on with the estimates. The console tools are free, limited to 60 calls a minute per workspace; Google's data lags about two days and goes back 16 months.
- **Without it.** If the tools are absent, go straight to the estimates and do not ask the user to connect anything. The estimate path runs on DataForSEO: `seo_get_ranked_keywords` on the user's domain or one URL (rank, volume and estimated `traffic` per keyword), `seo_get_domain_overview` with `history: true` (+56 credits, 12 months of estimated traffic and keyword counts), `seo_get_position` (one live rank, 6 credits) and `seo_get_serp` (the live page one, 1 credit per 10 results).
- **Label every number.** Search Console is measured; `traffic` and `organic_traffic` are models built from rank, volume and a click curve. Estimates are good for direction and for comparing sites, and can be far off for one site, most of all on long-tail and brand queries. Never mix the two in one column, and say which one each table uses.

### Keyword floors

- **Striking distance** is rank 4 to 20. At 1 to 3 little is left to gain; beyond 20 a page needs new content, not a fix.
- **Volume.** Skip keywords under about 50 monthly searches, except high-intent ones (a competitor's name with alternative, vs or pricing; "X software for Y"), where 20 buyers beat 2,000 browsers. A `volume` of null means no data, not zero: judge those by intent.
- **Difficulty.** Measure the site's reach from what it already wins: the median `kd` of the keywords where it ranks in the top 10 in `seo_get_ranked_keywords`. Aim at or below that, and up to about 10 above it for pages that will get links. A site with no top-10 rankings starts under KD 20.
- **Intent.** When the goal is signups or sales, `intent` commercial and transactional come first; informational keywords earn reach and support the money pages. Navigational keywords for another brand are off limits except on comparison pages.
- **Format.** The format of Google's top three (a list, a guide, a tool, a product page, a forum thread) is the format to match. A page in the wrong format does not rank by adding words.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Free first: `seo_get_page`, `get_task` and every `console_*` tool cost nothing (they are rate limited, so many URLs take time rather than credits). `seo_get_serp` is 1 credit per 10 results.
- Ranks in bulk, not one by one: `seo_get_position` is 6 credits per keyword and target, while `seo_get_ranked_keywords` is 10 credits per 100 rows. For more than two targets on one keyword, `seo_get_serp` with a larger `depth` is cheaper than a position per target.
- `history: true` on `seo_get_domain_overview` adds 56 credits: use it only when the question is about change over time.
- `seo_run_technical_crawl` charges on `max_pages` requested, not pages crawled, so size it to the site. `render: true` costs 10 times as much: use it only when the content needs JavaScript.
- A result this account already paid for is free while it is cached: 24 hours for SERPs and positions, 7 days for keyword, domain and backlink data.

### Handoff

- The server only reads. Never edit the site, publish, submit sitemaps, request indexing or set redirects. The deliverable is a table or a brief for the user, their writer or their developer. If the host has a CMS or document tool, offer to pass the result to it; do not publish.
- Titles, meta descriptions and outlines in a deliverable are drafts the host writes from the evidence in the table. Write a full article only when the user asks.
- The server keeps no history. Put the date, the market and the source (Search Console or estimate) on every deliverable so a later run can compare. Checks on a schedule belong to the `monitoring` group.

## Other groups

- Mentions and citations in AI answers (ChatGPT, Perplexity, Google AI Overviews), and whether AI crawlers can read the site: the [ai-search](../ai-search/SKILL.md) group; crawler access and llms.txt are its [site readiness](../ai-search/references/site-readiness.md) playbook.
- Backlinks, link prospects and lost or broken links: the [link-building](../link-building/SKILL.md) group. When a drop traces to lost links, its [lost links](../link-building/references/lost-links.md) playbook recovers them. Getting into other sites' "best X" lists is its [best-of lists](../link-building/references/best-of-lists.md) playbook.
- Rankings tracked every week, alerts on drops, a recurring SEO report: the [monitoring](../monitoring/SKILL.md) group, starting with [rank tracking](../monitoring/references/rank-tracking.md).
- Who the competitors are, their messaging, pricing and positioning: the [competitors](../competitors/SKILL.md) group. Comparison pages take their facts from its [messaging and pricing](../competitors/references/messaging-pricing.md) playbook.
- Content ideas across channels, social posts and repurposing a blog post: the [content](../content/SKILL.md) group.
- Keywords to bid on in Google Ads: the [paid-ads](../paid-ads/SKILL.md) group's [PPC keywords](../paid-ads/references/ppc-keywords.md).
- An audit, pitch or monthly report for a client or a prospect: the [agency](../agency/SKILL.md) group, which runs this group's audit and quick wins.
- "Where do I start" or "more signups" with no channel named: the [growth-plan](../growth-plan/SKILL.md) group.
