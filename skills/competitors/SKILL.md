---
name: competitors
description: Competitor research with the manifold tools. Tears down one competitor across search, paid ads, social accounts, company data and AI answers; finds the real competitors (direct, indirect and search-only); compares their messaging and pricing; turns the differences into positioning options; and plans a competitive strategy. Use when the user asks for a competitor teardown, competitive analysis or intelligence, a competitor profile or battlecard, what a rival is doing in marketing, who our competitors are, alternatives to a product, competitor messaging, value propositions, taglines, pricing tiers or packaging, how we are different, positioning, differentiation, a positioning statement, which category to claim, or a plan to beat or take share from a rival. A recurring competitor watch belongs to monitoring, and a full ad teardown to paid-ads. The result is a table for the user; sending, posting and scheduling are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Competitors

Every job here reads what competitors show in public: their search footprint, their ads, their pages and bios, their social accounts and their company records. Each ends in a table the user can act on, with the source of every number.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_domain_overview`, `ads_get_advertiser_ads` and `leads_get_company` (hosts often add a prefix, for example `mcp__manifold__seo_get_domain_overview`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- These playbooks read several tool groups: `seo_*`, `aeo_*`, `ads_*`, `leads_*`, `reddit_*` and the platform tools (`linkedin_*`, `tiktok_*`, `instagram_*`, `youtube_*`, `facebook_*`, `twitter_*`). If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run the playbook without them, and mark their rows or columns "not checked" rather than leaving them empty, so nobody reads a gap as a zero.

## Menu

Pick one job from the request, open its playbook and follow it. If the request names one competitor and asks what they are doing, open the teardown; if it asks who the competitors are, open find competitors. If it fits two jobs, ask one question.

| Job | The user says | Open |
|---|---|---|
| Competitor teardown: one competitor across search, ads, social, company data and AI answers, in one table | "tear down close.com", "what is Pipedrive doing in marketing", "competitor profile of X", "competitive intel on our main rival", "battlecard snapshot for X" | [references/teardown.md](references/teardown.md) |
| Find competitors: who the real competitors are, direct, indirect and search-only | "who are our real competitors", "who else sells what we sell", "competitors we have never heard of", "who do buyers compare us with", "which of our search rivals are actual competitors" | [references/find-competitors.md](references/find-competitors.md) |
| Messaging and pricing: how each competitor describes itself, what it offers in ads, and its pricing model | "compare competitor messaging", "how does X pitch itself", "competitor value props and taglines", "competitor pricing tiers side by side", "what do our competitors charge" | [references/messaging-pricing.md](references/messaging-pricing.md) |
| Positioning: how the user differs, as positioning options to choose from | "how should we position against X", "what makes us different", "differentiation", "positioning statement", "which category should we claim" | [references/positioning.md](references/positioning.md) |
| Competitive strategy: a plan to win against named competitors | "competitive strategy", "how do we beat X", "plan to take share from X", "a funded rival just launched, what do we do", "90-day plan against the incumbents" | [references/strategy.md](references/strategy.md) |

## Shared rules

### Real competitors

- A business competitor sells to the same buyer for the same job. A SERP competitor only shares keywords. `seo_get_serp_competitors` returns SERP competitors, and many of its top rows are publishers, marketplaces, review sites and directories. Never report a domain from it as a business competitor until its homepage title or its company description (`seo_get_page`, `leads_get_company`) shows it sells the same thing.
- Three classes, used in every table here. **Direct**: the same job for the same buyer. **Indirect**: the same job done another way: a spreadsheet, an agency, a feature inside a bigger platform, hiring someone, doing nothing. **Search-only**: competes for the user's keywords but not for their buyers: publishers and vendors in adjacent categories. Search-only matters for SEO and never for sales.
- Compare the user with two or three competitors, not ten. A teardown of every name on a list is expensive and nobody reads it.

### Evidence

- Every claim about a competitor carries its source: the tool and field, or the URL. Mark an inference as an inference.
- The tools read public signals: titles and headings, ads, bios, firmographics and modelled traffic. No manifold tool reads a page's body text or its prices. When a job needs page text (a pricing table, a feature list), the host can open the page if it has a browser or fetch tool, or the user can paste it. Never fill a price, a feature or a customer count from memory.
- `organic_traffic` is an estimate modelled from rankings: good for comparing sites with each other, not a visit count.
- Reddit search runs with its defaults, `sort: "relevance"` and `time_range: "all"`, and the playbook filters on `created_at` afterwards. Sorted `new` or `top` across all of Reddit the vendor drops the query, and a narrow `time_range` can return nothing; if most rows are off topic, the search failed. The [reddit router](../reddit/SKILL.md) has the details.
- Two sources for headcount disagree often: `employees` from `leads_get_company` (the provider's estimate) and from `linkedin_get_company` (people who list the company on LinkedIn). Report both.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Cheap first. `seo_get_page` is free but rate limited; ad library pages, social profiles and listings are 1 credit each; the SEO domain calls are 5 to 10. The expensive calls are `aeo_run_ai_answers` (18 credits per prompt on the default five engines), `leads_get_company` (10 credits each) and `seo_get_traffic_estimates` (50 plus 50 per 100 domains). Keep AI answers to two or three prompts outside the `ai-search` group.
- A result this account already paid for is free while cached: 7 days for SEO data, 24 hours for ad libraries and profiles, 30 days for company records. A second competitor in the same week costs only its own calls.

### Handoff

- Never send, post, schedule or contact anyone. The deliverable is a table or a short document for the user.
- Quote competitors' copy only as short evidence (a headline, a tagline), with the URL.
- The server keeps no state. To compare against this run later, the host keeps the table; a teardown repeated on a schedule is the `monitoring` group's competitor watch.

## Other groups

- A competitor watched over time ("every week", "tell me when they launch", "track their ads"): [monitoring competitor watch](../monitoring/references/competitor-watch.md), which reruns the teardown on a schedule.
- A competitor's ads in full, with angles, offers and creative over time: [paid-ads competitor ads](../paid-ads/references/competitor-ads.md).
- A competitor's accounts on one platform in depth: [TikTok](../tiktok/references/competitor-accounts.md), [Instagram](../instagram/references/competitor-accounts.md), [YouTube](../youtube/references/competitor-channels.md), [LinkedIn company page audit](../linkedin/references/company-page-audit.md), [Facebook page audit](../facebook/references/page-audit.md).
- Keywords a competitor ranks for and the user does not, and "X vs Y" or "X alternatives" pages: [seo](../seo/SKILL.md) and its [comparison pages](../seo/references/comparison-pages.md).
- Whether AI engines recommend a competitor over the user, in depth: [ai-search visibility check](../ai-search/references/visibility-check.md).
- What people complain about in a competitor on Reddit: [reddit competitor complaints](../reddit/references/competitor-complaints.md).
- Competitors' backlinks as link targets: [link-building](../link-building/SKILL.md).
- Buyers, their pain points, demand for an idea and the whole market mapped by segment: [customers](../customers/SKILL.md).
- The same research for an agency's client or prospect: [agency](../agency/SKILL.md).
