---
name: build-personas
description: When the user wants buyer personas built from evidence rather than imagination. Sizes the job titles and seniority that buy in a company database, hears their public voice in LinkedIn posts and their frank voice on Reddit, and groups them into two to four personas where every field is evidence or marked as inference. Also use when the user mentions buyer personas, persona research, who is our ideal customer, who buys a tool like ours, which roles buy, personas by role and seniority, who decides and who uses it, or audience research on the buyers themselves. Writing the product context file goes to create-product-context, a list of people to contact to build-lead-list, how many companies could buy to size-market, the problems buyers name to find-pain-points, the doubts they raise to find-objections.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Buyer personas

Two to four buyer personas, each built from evidence rather than imagination: the job titles and seniority that buy, how many of them a company database holds, what they post about on LinkedIn, and what they say more frankly on Reddit. It ends in a persona table where every field is either evidence or marked as an inference.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_search_people`, `linkedin_search_posts` and `reddit_search_posts` (hosts often add a prefix, for example `mcp__manifold__leads_search_people`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- This skill reads three tool groups: `leads_*`, `linkedin_*` and `reddit_*`. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run on the sources that remain, and name the missing sources in the deliverable, since a signal that was not checked is not a signal that is absent.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product and the problem it solves, the target audience, the personas already written, current customers, customer language) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first. After delivering, offer to write the personas into `.agents/product-marketing.md` with [create-product-context](../create-product-context/SKILL.md).

## Inputs to settle first

- **Current customers**: the titles and companies of the people who bought, if the user has any. This is the best evidence there is; ask for it first.
- **Product and problem**: what it does, and the problem in the buyer's words.
- **Market**: industries and locations. Industries are LinkedIn's names, matched exactly: copy the `industry` string from `leads_get_company` on two of the user's customers (1 credit each). Default: the industry of the user's best customers, in the United States.
- **Candidate roles**: default eight titles guessed from the category and the customers (for a sales tool: founder, head of sales, sales manager, RevOps, account executive, and so on).
- **Budget**: a default run costs about 2 + 32 + 24 + 4 + 10 + 8 = 80 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Size the roles.** `leads_search_people` once per candidate title with `titles` (the full title, no `seniority`), `industries` and `locations` (4 credits a call, 32 for eight titles). Read `rows_available` in the response `meta`: how many people hold that title in the target market. Read the `title` of the rows too: they show the variants people actually use ("Revenue Operations Lead" as well as "RevOps Manager"). Then, for the two strongest functions, run once per `seniority` level (junior, senior, executive) with the bare function word as `titles` (`["Sales"]`, not `["Head of Sales"]`), 24 credits, to see where decision power sits. `seniority` combines with `titles` into phrases ("VP Sales", "Head of Sales"), so a full title with a `seniority` matches almost nothing.
2. **Hear the public voice.** `linkedin_search_posts` with the problem in the buyers' words and `since: "year"` (1 credit a page), two to four queries. `linkedin_get_profile` on up to 10 of the authors (1 credit each) confirms their role from the `bio`; keep the posts by people in a candidate role. Read what they are measured on, the goals and tools they name, and what they complain about in public.
3. **Hear the frank voice.** `reddit_search_subreddits` with the category (1 credit) finds the communities where these roles talk. `reddit_search_posts` in the top two with the problem words (1 credit each; relevance sort, filtered to the past year on `created_at`, per the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching)), then `reddit_get_comments` on the five busiest threads (1 credit each). Reddit is where people say what they would not post under their name: budgets, who signs, the tools they dropped and why. Take phrases verbatim. For pains ranked across all sources, use [find-pain-points](../find-pain-points/SKILL.md).
4. **Group into personas.** Cluster the evidence into two to four personas by role and by what they are trying to get done. Each needs: title variants, seniority, the count from step 1 with its filters, where they talk, what they are measured on, their top pains in their words, what triggers a purchase, their objections, and their role in the purchase (decides, uses, influences). Merge two personas that differ only in title.
5. **Deliver** a table with one row per persona: name (a role, not a made-up first name), title variants, seniority, count in the database (with the filters used), role in the purchase, goals and metrics, top pains (quotes with links), objections, triggers, where they talk (subreddits, LinkedIn topics), and the evidence count behind the row. Mark each field as evidence or inference.

## Judgment

- No filler. Age, hobbies and a stock photo add nothing a tool can back; a field with no evidence says "unknown".
- `rows_available` is the provider's total for the filters given: it compares roles with each other, it is not the number of buyers or a market size. Add up the title variants that describe one role before comparing.
- When managers far outnumber executives for a role, the person who uses the product and the person who signs are different people. Write both personas, and say which one the message must convince.
- LinkedIn shows the polished, public voice; Reddit the private, frank one. Take goals from LinkedIn and objections from Reddit.
- Current customers beat every search. With 20 real customer titles, start from them and use the tools to size and hear those roles, not to guess new ones.
- The leads tools are business databases. For a consumer product, skip step 1 and the industry lookup and build the personas from Reddit and from [find-pain-points](../find-pain-points/SKILL.md) across TikTok, Instagram and YouTube.
- Every quote carries its link; leave usernames out of anything the user will publish. Never contact anyone found here.
- If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.

## Related skills

- A list of people in a persona to contact: [build-lead-list](../build-lead-list/SKILL.md). How many companies could buy: [size-market](../size-market/SKILL.md).
- The problems these buyers name, ranked: [find-pain-points](../find-pain-points/SKILL.md). The doubts that stop them buying: [find-objections](../find-objections/SKILL.md).
- Positioning for the persona that signs: [find-positioning](../find-positioning/SKILL.md).
- The context file every skill reads: [create-product-context](../create-product-context/SKILL.md).
