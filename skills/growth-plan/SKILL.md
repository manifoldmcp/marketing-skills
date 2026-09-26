---
name: growth-plan
description: Growth planning with the manifold tools, for founders and marketers who know the goal but not the channel. Takes a cheap baseline across search, AI answers, communities, social, ads and company counts, finds the gaps against competitors, and routes to the right channel playbooks in a 30-60-90 day plan. Also plans the first 100 customers, a go-to-market for a new product, entry into a new country or segment, and brand awareness. Use when the user asks where to start, how to grow a startup, for a growth plan, growth strategy or marketing plan, for more traffic, signups, users, leads or customers with no channel named, for traction, first customers or first users, doing things that don't scale, a go-to-market or GTM plan, the ICP, positioning and channels for a new product, expanding into a new country or segment, international expansion, brand awareness, share of voice, or getting the brand known. It plans; running campaigns, posting and sending stay with the user.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Growth plan

The start for anyone who knows what they want (more signups, the first customers, a new market) but not which channel gets it. Every job here measures a little of everything, picks the few channels the evidence supports, and hands the work to the groups that own those channels.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `reddit_search_posts` (hosts often add a prefix, for example `mcp__manifold__seo_search_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The baselines read `seo_*`, `aeo_*`, `reddit_*`, the social platforms, `ads_*` and `leads_*`. If some of these tool groups are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, carry on, and mark those channels "not measured" in the plan rather than calling them weak.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it asks to grow and names no channel, open the growth plan.

| Job | The user says | Open |
|---|---|---|
| Growth plan by goal (traffic, signups, leads, awareness, first customers): channels chosen from a baseline, in a 30-60-90 plan | "where do I start", "how do I grow my startup", "we need more signups", "marketing plan for the next 90 days", "growth strategy" | [references/strategy.md](references/strategy.md) |
| First 100 customers: where the first buyers are talking now, and how to reach them by hand | "get our first 100 customers", "we have zero users", "find our first paying customers", "early traction", "do things that don't scale" | [references/first-100-customers.md](references/first-100-customers.md) |
| Go-to-market: ICP, positioning, channels and launch for a new product, in order | "go-to-market plan", "GTM strategy for our new product", "how do we take this to market", "ICP, positioning and channels", "we're building a second product" | [references/go-to-market.md](references/go-to-market.md) |
| Market entry: whether a new country or segment is worth it, and who wins there | "expand into Germany", "is there demand for us in the UK", "enter a new market", "international expansion", "move into the enterprise segment" | [references/market-entry.md](references/market-entry.md) |
| Brand awareness: the brand against competitors on branded search, AI answers, social and press, and the levers | "brand awareness plan", "nobody knows who we are", "share of voice against competitors", "grow branded search", "get our name out there" | [references/brand-awareness.md](references/brand-awareness.md) |

## Shared rules

### Goal first

One primary goal per plan. The goal sets the KPI, and the KPI decides which channels count:

| Goal | The KPI that decides | Leading signals the tools re-measure |
|---|---|---|
| Traffic | organic visits (the user's analytics) | estimated organic traffic (`seo_get_domain_overview`), ranks of target keywords (`seo_get_position`) |
| Signups | signups a week (the user's analytics) | the traffic signals, plus mentions and referring domains |
| Leads | qualified leads or meetings (the user's CRM) | size of the reachable market (`leads_search_companies`), posts about the problem (`linkedin_search_posts`) |
| Awareness | branded search (`seo_get_keyword_metrics` on the brand) | AI mention rate (`aeo_run_ai_answers`), mentions by others, referring domains |
| First customers | customers (the user's count) | conversations started a week, from the tally the plan sets up |

The tools see public signals, not conversion. Ask for the current number of the deciding KPI at intake, so day 90 has something to compare with.

### Channel signals

When the baseline supports a channel, with the reason for each threshold:

- **Search**: the category's main keywords add up to more than about 1,000 searches a month, with some at KD under 30. Below that, search captures little until demand exists.
- **AI answers**: the engines answer the buyer's questions and name competitors. Competitors named and the user not is a gap worth closing now; nobody named means the category is early.
- **Reddit**: communities where the problem comes up every week, and threads that ask for a recommendation. Count in the full window of `reddit_get_new_posts`, not from search, which is a sample (see the [reddit router](../reddit/SKILL.md)).
- **Short video**: TikTok or YouTube videos on the category in the last month with views in the tens of thousands.
- **LinkedIn**: a B2B buyer, and posts about the problem in the last month that draw comments.
- **Paid**: competitors with ads still `active` whose `first_shown` is 90 or more days ago. Advertisers rarely keep paying for a losing ad for three months.
- **Outbound**: a B2B buyer, a price above about $1,000 a year, and an ICP a company search can express (industry, headcount, location). Below that price, a sales conversation costs more than the customer pays.

### Credits

- A growth plan costs about 80 credits; first 100 customers about 20; go-to-market about 40; market entry about 105 per market; brand awareness about 130 for the brand and three competitors. The channel playbooks they route to state their own; add them up before running more than one.
- Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- The baseline calls are the same ones the channel playbooks start from, and a result this account already paid for is free while cached (7 days for most search data), so running the chosen playbook soon after the plan costs less.

### Handoff

- The deliverable is a plan that names the playbooks to run next. Offer to run the first one. Running campaigns, posting, sending and spending stay with the user.
- The server keeps no state. The host keeps the plan and its KPIs; re-measuring at day 30, 60 and 90 runs the same calls with the same competitors and queries. Scheduling that is the [monitoring](../monitoring/SKILL.md) group's job.

## Other groups

- A request that names the channel goes to that channel's group: [seo](../seo/SKILL.md), [ai-search](../ai-search/SKILL.md), [link-building](../link-building/SKILL.md), [paid-ads](../paid-ads/SKILL.md), [influencers](../influencers/SKILL.md), [leads](../leads/SKILL.md), [reddit](../reddit/SKILL.md), [tiktok](../tiktok/SKILL.md), [instagram](../instagram/SKILL.md), [youtube](../youtube/SKILL.md), [linkedin](../linkedin/SKILL.md), [facebook](../facebook/SKILL.md).
- A launch with a date: [launch](../launch/SKILL.md).
- Who the competitors are and how to beat them: [competitors](../competitors/SKILL.md). Who the customers are and what they need: [customers](../customers/SKILL.md).
- Which channels to publish content on, content ideas and a calendar: [content](../content/SKILL.md).
- Tracking the plan's KPIs every week: [monitoring](../monitoring/SKILL.md).
- A plan for an agency's client: [agency](../agency/SKILL.md).
