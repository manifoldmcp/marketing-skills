# Go-to-market

A go-to-market plan for a new product answers four questions in order: who it is for, why they would pick it over what they use now, how they will hear about it, and how it launches. Each question has a home in another group. This playbook runs a cheap market check, puts the answers in that order, and links to the playbooks that go deep.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Product**: what it does, its stage (idea, beta, ready) and its price or price range. The price sets the sales motion.
- **ICP**: who the founder thinks buys. Default: stated as a hypothesis and tested in step 2.
- **Competitors and alternatives**: products and workarounds (spreadsheets, an agency, doing nothing). Default: the ones the market check finds.
- **Market**: the country and language. Default: the United States, English.
- **Launch date**: if there is one.
- **Team and budget**: who sells, who writes, money for ads or creators, and credits (this playbook costs about 35).

## Steps

1. **Check the market.** About 35 credits.
   - Demand: `seo_search_keywords` with the category as `seed` (10 credits). Read `volume` and `trend[12]`: demand that exists and can be captured, or a new category where demand has to be made.
   - Who wins now: `seo_get_serp` for "best <category>" and "<leading competitor> alternatives" (1 credit each), and `aeo_run_ai_answers` with "best <category> for <ICP>" and the competitors as `brands` (18 credits), then `get_task`.
   - The problem in the buyer's words: `reddit_search_posts` with the problem at the default relevance sort (1 credit).
   - B2B size: `leads_search_companies` with the ICP's industry, headcount and location (4 credits); read `meta.rows_available`.

   For depth: [demand check](../../customers/references/demand-check.md), [market size](../../leads/references/market-size.md), [find competitors](../../competitors/references/find-competitors.md) and [market map](../../customers/references/market-map.md).
2. **Settle the ICP.** Run [personas](../../customers/references/personas.md) with the hypothesis, and [pain points](../../customers/references/pain-points.md) for the problem in the buyer's own words. Choose one segment to win first.
3. **Settle the positioning.** Run [positioning](../../competitors/references/positioning.md) against the three closest competitors, and [messaging and pricing](../../competitors/references/messaging-pricing.md) to see what they claim and charge. Write one line: for whom, the problem, the alternative they use now, and why this is better.
4. **Choose the motion and the channels.** The price and the ICP set the motion (see Judgment). Then run [channels](../../content/references/channels.md) and check each candidate against the [channel signals](../SKILL.md#channel-signals); keep two or three.
5. **Plan the launch.** With a date, run the [launch plan](../../launch/references/launch-plan.md). Without one, set the date once the ICP and the positioning hold.
6. **Deliver** a one-page GTM document: the ICP (one segment, with the buyer's own words from step 2), the positioning line and the three alternatives, the price context, the motion, the channels with the evidence for each, the launch date and plan, KPIs per stage (conversations, trials or signups, customers), and three first actions. Each section links to the playbook that produced it.

## Judgment

- The order matters. Channels chosen before the ICP reach the wrong people, and a launch before the positioning holds spends the one day of attention on an unclear message.
- The price sets the motion. Under about $1,000 a year, a sales call costs more than the customer pays, so the product sells itself through search, communities, content and a trial. Above about $10,000 a year, outbound and demos are normal and [leads](../../leads/SKILL.md) is a core channel. Between the two: product-led with a light sales touch.
- Near-zero search volume means a new category. Demand is made (communities, creators, founder content, press), not captured (search, search ads), and it takes longer.
- One segment first. A GTM plan for "small businesses and enterprises" is two plans.
- The market check is a sample: one AI prompt, two results pages, one page of Reddit. It points the linked playbooks; it is not the verdict.
