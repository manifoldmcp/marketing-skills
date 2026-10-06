---
name: create-gtm-plan
description: When the user wants a go-to-market plan for a new product. Runs a cheap market check (search demand, who wins Google and AI answers, the problem on Reddit, the B2B company count), then settles the ICP, the positioning, the sales motion, the channels and the launch in that order, in a one-page GTM document that links to the skills that go deep. Also use when the user mentions a go-to-market plan, GTM strategy for our new product, how do we take this to market, ICP, positioning and channels for a new product, or we are building a second product. A growth plan for an existing product goes to create-growth-plan, positioning alone to find-positioning, which channels alone to pick-channels, the launch day plan to create-launch-plan, a new country to create-market-entry-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Go-to-market

A go-to-market plan for a new product answers four questions in order: who it is for, why they would pick it over what they use now, how they will hear about it, and how it launches. Each question has a home in another skill. This skill runs a cheap market check, puts the answers in that order, and links to the skills that go deep.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `aeo_run_ai_answers` (hosts often add a prefix, for example `mcp__manifold__seo_search_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The market check reads `seo_*`, `aeo_*`, `reddit_*` and `leads_*`. If some of these tool groups are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, carry on, and mark those checks "not measured".

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product, its price, the ICP hypothesis, the competitors and alternatives, the market and the team) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Product**: what it does, its stage (idea, beta, ready) and its price or price range. The price sets the sales motion.
- **ICP**: who the founder thinks buys. Default: stated as a hypothesis and tested in step 2.
- **Competitors and alternatives**: products and workarounds (spreadsheets, an agency, doing nothing). Default: the ones the market check finds.
- **Market**: the country and language. Default: the United States, English.
- **Launch date**: if there is one.
- **Team and budget**: who sells, who writes, money for ads or creators, and credits (this skill costs about 35, plus the skills it opens, which state their own). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Check the market.** About 35 credits.
   - Demand: `seo_search_keywords` with the category as `seed` (10 credits). Read `volume` and `trend[12]`: demand that exists and can be captured, or a new category where demand has to be made.
   - Who wins now: `seo_get_serp` for "best <category>" and "<leading competitor> alternatives" (1 credit each), and `aeo_run_ai_answers` with "best <category> for <ICP>" and the competitors as `brands` (18 credits), then `get_task`.
   - The problem in the buyer's words: `reddit_search_posts` with the problem at the default relevance sort (1 credit).
   - B2B size: `leads_search_companies` with the ICP's `industries`, `employee_ranges` and `locations` (4 credits); read `meta.rows_available`.

   For depth: [check-demand](../check-demand/SKILL.md), [size-market](../size-market/SKILL.md), [find-competitors](../find-competitors/SKILL.md) and [map-market](../map-market/SKILL.md).
2. **Settle the ICP.** Run [build-personas](../build-personas/SKILL.md) with the hypothesis, and [find-pain-points](../find-pain-points/SKILL.md) for the problem in the buyer's own words. Choose one segment to win first.
3. **Settle the positioning.** Run [find-positioning](../find-positioning/SKILL.md) against the three closest competitors, and [compare-messaging](../compare-messaging/SKILL.md) to see what they claim and charge. Write one line: for whom, the problem, the alternative they use now, and why this is better.
4. **Choose the motion and the channels.** The price and the ICP set the motion (see Judgment). Then run [pick-channels](../pick-channels/SKILL.md) and check each candidate against the [channel signals](../create-growth-plan/references/channel-signals.md); keep two or three.
5. **Plan the launch.** With a date, run [create-launch-plan](../create-launch-plan/SKILL.md). Without one, set the date once the ICP and the positioning hold.
6. **Deliver** a one-page GTM document: the ICP (one segment, with the buyer's own words from step 2), the positioning line and the three alternatives, the price context, the motion, the channels with the evidence for each, the launch date and plan, KPIs per stage (conversations, trials or signups, customers), and three first actions. Each section links to the skill that produced it.

## Judgment

- The order matters. Channels chosen before the ICP reach the wrong people, and a launch before the positioning holds spends the one day of attention on an unclear message.
- The price sets the motion (rules of thumb, the same bands as the outbound signal in the [channel signals](../create-growth-plan/references/channel-signals.md)). Under about $1,000 a year, a sales call costs more than the customer pays, so the product sells itself through search, communities, content and a trial. Above about $10,000 a year, outbound and demos are normal and [create-outbound-plan](../create-outbound-plan/SKILL.md) is a core channel. Between the two: product-led with a light sales touch.
- Near-zero search volume means a new category. Demand is made (communities, creators, founder content, press), not captured (search, search ads), and it takes longer.
- One segment first. A GTM plan for "small businesses and enterprises" is two plans.
- The market check is a sample: one AI prompt, two results pages, one page of Reddit. It points the linked skills; it is not the verdict.
- Say the estimate before the first paid call; `dry_run: true` prices any call for free. A result this account already paid for is free while cached (7 days for most search data), so the linked skills pay again only for the calls that differ.

## Related skills

- A growth plan for a product already on the market: [create-growth-plan](../create-growth-plan/SKILL.md).
- The first customers by hand, once the ICP holds: [find-first-customers](../find-first-customers/SKILL.md).
- A new country or segment for an existing product: [create-market-entry-plan](../create-market-entry-plan/SKILL.md).
- The launch day plan, press and creators: [create-launch-plan](../create-launch-plan/SKILL.md).
- How to win against the competitors found: [create-competitor-plan](../create-competitor-plan/SKILL.md).
