---
name: customers
description: Customer and market research with the manifold tools. Ranks buyers' pain points across Reddit and TikTok, Instagram, YouTube and Facebook comments; builds buyer personas from job titles, seniority and how those people talk on LinkedIn and Reddit; checks demand for a product, feature or idea from search volume and trend, AI prompts, Reddit and video; and maps the players in a market by segment. Use when the user asks for customer or market research, voice of the customer (VOC), customer pain points or problems, jobs to be done, buyer personas, the ideal customer or who buys this, audience research, demand or idea validation, whether there is a market or search demand for something, whether a market is growing, a market map or landscape, the players in a category, or a segment breakdown. Pain points from Reddit alone belong to reddit, and a count of companies to sell to belongs to leads. The result is a table for the user; surveys, interviews, sending and posting are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Customers

Every job here reads what the market and its buyers say and do in public: what they search for, what they ask AI engines, what they write on Reddit and under videos, which roles they hold, and which companies serve them. Each ends in a table where every finding has its evidence.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_posts`, `seo_search_keywords` and `leads_search_people` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- These playbooks read several tool groups: `reddit_*`, `seo_*`, `aeo_*`, `leads_*`, `ads_*` and the platform tools (`tiktok_*`, `instagram_*`, `youtube_*`, `facebook_*`, `linkedin_*`). If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run the playbook on the sources that remain, and name the missing sources in the deliverable, since a signal that was not checked is not a signal that is absent.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only says "research our market", ask whether the user wants the buyers' problems (pain points), the buyers themselves (personas), whether demand exists (demand check) or who serves the market (market map).

| Job | The user says | Open |
|---|---|---|
| Pain points across sources: the problems buyers name on Reddit and under videos, clustered, counted and ranked, with quotes | "what are our customers' biggest pain points", "voice of the customer research", "what do people struggle with in X", "rank the problems people mention, with quotes", "jobs to be done research across platforms" | [references/pain-points.md](references/pain-points.md) |
| Personas: two to four buyer personas, each with evidence | "build buyer personas", "who is our ideal customer", "which roles buy a tool like ours", "persona research", "who decides and who uses it" | [references/personas.md](references/personas.md) |
| Demand check: whether people want X, with what each signal can and cannot prove | "is there demand for X", "validate this idea", "is anyone searching for X", "is this market growing or dying", "should we build X" | [references/demand-check.md](references/demand-check.md) |
| Market map: the players in a market, grouped by segment | "map the market", "market landscape for X", "who are all the players in X", "segment the X market", "category map with the leaders in each niche" | [references/market-map.md](references/market-map.md) |

## Shared rules

### Evidence

- Every finding carries its evidence: a quote with its link, or a number with the tool that returned it. A persona field or a pain with no evidence says "unknown" or goes.
- Count people, not posts. One person posting five times is one voice: dedupe on the author before counting.
- Every search here is ranked and never complete (Reddit, TikTok, YouTube, LinkedIn, Instagram). A count is a count in the sample; say how big the sample was, and use counts to rank findings against each other, never to size a market.
- Leave usernames out of anything the user will publish. Quote the words and link the post.

### Reddit search

- Run `reddit_search_posts` and `reddit_search_comments` with their defaults, `sort: "relevance"` and `time_range: "all"`, and filter the rows on `created_at` yourself. Sorted `new` or `top` across all of Reddit the vendor drops the query, and a narrow `time_range` can return nothing, comment search most of all.
- If most rows do not contain the query's words, the search failed; do not count them. The [reddit router](../reddit/SKILL.md) has the rest of the Reddit rules.

### Signals and their limits

- `rows_available` from `leads_search_people` and `leads_search_companies` counts records in one provider's database that match loose keywords: a relative size between segments or roles, not a market size.
- Views and likes measure attention, not intent to buy. A request for a recommendation ("is there a tool that...") is the strongest intent signal a public post carries.
- Nothing here proves willingness to pay. Only a sale, a preorder or a paid pilot does; say so whenever a verdict depends on it.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Cheap first: Reddit, TikTok, YouTube and LinkedIn searches, comment pages and `leads_get_company` cost 1 credit; `leads_search_people` and `leads_search_companies` cost 4 a page; `seo_search_keywords` costs 10. The expensive calls are `aeo_search_prompts` (45 at the default 50 rows), `aeo_run_ai_answers` (18 credits per prompt on the default engines) and `seo_get_traffic_estimates` (50 plus 50 per 100 domains).
- A result this account already paid for is free while cached, so rerunning a playbook on the same topic within the cache window costs little.

### Handoff

- Never post, reply, send a survey or contact anyone. The deliverable is a table for the user.
- The server keeps no state. The host keeps the table; listening to the same topics on a schedule belongs to the `monitoring` group.

## Other groups

- Pain points from Reddit only: [reddit pain points](../reddit/references/pain-points.md). Complaints about a named competitor: [reddit competitor complaints](../reddit/references/competitor-complaints.md).
- Comments on one platform only: [TikTok](../tiktok/references/comment-mining.md), [Instagram](../instagram/references/comment-mining.md), [YouTube](../youtube/references/comment-mining.md), [Facebook](../facebook/references/comment-mining.md) comment mining.
- People posting about the problem on LinkedIn, to engage with: [linkedin problem posts](../linkedin/references/problem-posts.md).
- How many companies could buy, and lists of them to contact: [leads market size](../leads/references/market-size.md) and [leads lead list](../leads/references/lead-list.md).
- Who the user competes with directly, their messaging and pricing, and positioning: [competitors](../competitors/SKILL.md).
- Keyword research for blog content: [seo](../seo/SKILL.md).
- Brand and topic mentions on a schedule: [monitoring brand mentions](../monitoring/references/brand-mentions.md).
- "Where do I start" or a growth plan with no channel named: [growth-plan](../growth-plan/SKILL.md).
