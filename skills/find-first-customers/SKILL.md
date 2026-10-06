---
name: find-first-customers
description: When the user wants their first customers or first users and has few or none. Finds where people with the problem are talking now (Reddit communities in the last week, LinkedIn posts, competitors' unhappy customers, a B2B company count), counts the conversations a week each place can supply, picks two, and sets a weekly routine and a tally for reaching them by hand. Also use when the user mentions get our first 100 customers, we have zero users, find our first paying customers, early traction, pre-revenue, or do things that don't scale. A growth plan for a company past 100 customers goes to create-growth-plan, a cold outreach program to create-outbound-plan, a dated launch to create-launch-plan, Reddit replies alone to find-reddit-threads.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# First 100 customers

The first customers come from conversations the founder starts by hand, one at a time, with people who already have the problem. This skill finds where those conversations are happening now, counts how many a week each place can supply, picks two, and routes to the skills that work them.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_subreddits` and `linkedin_search_posts` (hosts often add a prefix, for example `mcp__manifold__reddit_search_subreddits`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The steps read `reddit_*`, `linkedin_*` and `leads_*`. If some of these tool groups are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, carry on, and mark those sources "not measured" rather than empty.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product, the problem in the buyer's words, the ICP and the competitors) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Product and problem**: what it does, and three to five phrases a buyer would use for the problem, not for the product ("chasing late invoices", not "AR automation").
- **ICP**: B2B or B2C, who exactly, and where they are.
- **Offer**: what the founder can give an early customer: a free pilot, a discount, setup done for them.
- **Hours**: a week for outreach and replies. Default: 5.
- **Network**: people the founder already knows in the ICP. No tool sees it, and it is usually where the first ten come from; ask.
- **Competitors**: two or three the buyers use now, including workarounds.
- **Budget**: about 3 + 5 + 3 + 2 + 4 = 17 credits here, plus the skills chosen in step 5, which state their own. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Count the people asking on Reddit.** `reddit_search_subreddits` with each problem phrase (1 credit each) for the communities where it comes up. Then `reddit_get_new_posts` on the top five with `since: "7d"` and `match` set to the phrases (1 credit per subreddit): every post about the problem there in the last week. Count the ones that ask for help or a tool. Search would give a sample, not a count; the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md) says why.
2. **Count the people posting about it on LinkedIn.** `linkedin_search_posts` with each phrase and `since: "month"` (1 credit each). Count the posts by people in the ICP who describe the problem. The search is ranked, so the count is a floor.
3. **Find the unhappy customers of competitors.** `reddit_search_comments` with "<competitor> alternative" at the default relevance sort (1 credit each). People asking what to switch to are the warmest conversations there are.
4. **Size the list (B2B).** `leads_search_companies` with the ICP's `industries` (LinkedIn's industry names, matched exactly), `employee_ranges` and `locations` (4 credits). `meta.rows_available` says whether a hand-picked list of a few hundred accounts is there to be built.
5. **Pick two sources, and open their skills.** Rank the sources by conversations a week times fit, and keep the two the founder's hours allow:
   - Reddit threads asking now: [find-reddit-threads](../find-reddit-threads/SKILL.md), in the communities from [find-subreddits](../find-subreddits/SKILL.md).
   - LinkedIn posts about the problem: [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md).
   - Competitors' unhappy customers: [find-competitor-complaints](../find-competitor-complaints/SKILL.md).
   - A hand-picked list of B2B accounts: [build-lead-list](../build-lead-list/SKILL.md), with [find-buying-signals](../find-buying-signals/SKILL.md) for the ones changing now and [write-first-lines](../write-first-lines/SKILL.md) for the opening message.
   - Facebook groups the founder belongs to: [mine-facebook-groups](../mine-facebook-groups/SKILL.md).
6. **Set the weekly routine.** Work back from the target. If one conversation in ten becomes a customer, 100 customers take about 1,000 conversations, and the hours a week decide how many weeks that is. Write the routine per source, for example: answer five threads a day, send twenty first messages a week.
7. **Deliver** a table: source, evidence (count a week or a month, and two example URLs), conversations a week it can supply, the linked skill, the routine, and the owner. Add a tally sheet (name, source, date of first contact, status, customer yes or no) for the host to keep: it is the KPI until customers exist. Nothing is sent or posted from here.

## Judgment

- Help first. On Reddit, a pitch in a reply is removed and remembered; an answer that solves the problem, naming the product only where the rules allow it, earns the direct message.
- The founder does this personally. At this stage every conversation also teaches what to build and what to say, and automating it loses that.
- Start with the network: the first ten usually come from people the founder knows or can be introduced to.
- A source with nothing in the last week is not a source yet. Do not plan around it.
- Measure conversations started a week, not impressions or followers.
- The one-in-ten rate is a planning assumption. Replace it with the real rate after the first 50 conversations.
- Say the estimate before the first paid call; `dry_run: true` prices any call for free. Sending, posting and messaging stay with the founder.

## Related skills

- A growth plan by goal once the first customers exist: [create-growth-plan](../create-growth-plan/SKILL.md).
- A go-to-market plan when the ICP is not settled: [create-gtm-plan](../create-gtm-plan/SKILL.md).
- Outbound as a program, past the founder's hand-picked list: [create-outbound-plan](../create-outbound-plan/SKILL.md).
- A launch on a date: [create-launch-plan](../create-launch-plan/SKILL.md).
- The problem in the buyer's words, as research: [find-pain-points](../find-pain-points/SKILL.md).
