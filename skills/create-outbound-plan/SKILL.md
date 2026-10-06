---
name: create-outbound-plan
description: When the user wants an outbound or ABM strategy, who to target, which signals to act on and how many to contact, before anyone builds a list. Reads the ICP from the best customers, counts the accounts and buyers, checks LinkedIn signal volume, new-leader triggers and competitor ads, and ends in a 30-60-90 day plan built from the outbound skills. Also use when the user mentions outbound strategy, plan our cold outreach, an ABM plan, how should we do outbound, start an outbound motion, cold outreach strategy this quarter, or moving from founder-led sales to outbound. A list of leads itself goes to build-lead-list, a market count alone to size-market, a plan across every channel to create-growth-plan, the first customers with no channel chosen to find-first-customers.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Outbound strategy

An outbound strategy decides who to contact, why now, and how many the team can work well, before anyone builds a list. It reads the ICP from the best customers, counts the market, checks where the signals are, and ends in a 90-day plan that the other outbound skills execute, not in a list of leads.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_search_companies` and `linkedin_search_posts` (hosts often add a prefix, for example `mcp__manifold__leads_search_companies`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it the baseline cannot count accounts or buyers; the LinkedIn and ads steps can still run.
- This skill also reads `linkedin_*` and `ads_*`. If one of those groups is switched off, skip the steps that need it and say which signal is missing from the result.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the goal, the ICP, the best customers, the personas and job titles, the countries, the competitors, the budget) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: qualified meetings or pipeline per month. Default: 8 qualified meetings a month by day 90.
- **Stage**: founder-led sales with under 20 customers, or a sales team with a motion that works. Default: founder-led.
- **ICP**: industry, size, region and the buyer's title. Default: read from the best customers in step 2.
- **Best customers**: three to five customer domains, the ones that bought fastest and stayed. Without them the ICP is a guess; ask for them even when the user states an ICP.
- **Budget**: credits for the research (this skill costs about 30) and for lists (a 50-contact lead list is about 326), and the sending capacity. Default: 2,000 credits a month and one sequencer with two warmed mailboxes on a secondary domain.
- **Team**: who writes and sends, which sequencer and CRM they use, and hours a week. Default: the founder, 3 hours a week.
- **Competitors**: two or three. Default: ask; if the user does not know, [find-competitors](../find-competitors/SKILL.md) finds them.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 11 credits.
   - `leads_get_company` on three best customers (1 credit each). Read `industry`, `keywords[]`, `employees`, `location` and `description`, and write the ICP as search filters: the `industry` strings they share, the employee band that covers them, the regions.
   - `leads_search_companies` with those filters (4 credits). `rows_available` is the account universe. Read the first 20 rows: if fewer than about 15 fit, the filters are too loose; change the industry or the band before anything else.
   - `leads_search_people` with the buyer `titles` and `company_domains` set to the domains from those rows (4 credits for up to 100 accounts). Buyers per account says how many people each account yields.
   - The [search rules](references/lead-data.md#searches-and-counts) apply: count, do not fetch.
3. **Gaps.** About 18 credits more.
   - Segments: `leads_search_companies` with the same industries for two or three other employee bands or regions (4 credits each). Compare where the accounts are with where the customers are. A segment the customers already cluster in is where to start; a large segment with no customers yet is a test, not the plan.
   - Signals: `linkedin_search_posts` with two phrases of the problem in the buyer's words and `since: "month"` (1 credit a page each). If buyers post about the problem every week, signal-led outbound has enough volume to run weekly.
   - Competitors: `ads_get_advertiser_ads` with `platform: "linkedin"` on each competitor (1 credit a page). The offer and the pain they pay to put in front of the same buyers show which angle is crowded. The full teardown is [tear-down-competitor](../tear-down-competitor/SKILL.md).
   - Triggers: `linkedin_search_posts` with `"starting a new position as <buyer title>"` and `since: "month"` (1 credit a page). The number of new buyers a month says whether a new-leader trigger has volume. The tools cannot see a tech stack, so "accounts using a competitor" is not a list they can build.
4. **Tactics.** Choose two or three of these skills and say why each fits the numbers:
   - [size-market](../size-market/SKILL.md) when the ICP is not settled or the universe may be too small: counts per segment decide where to start.
   - [find-lookalike-companies](../find-lookalike-companies/SKILL.md) when there are three or more good customers: the fastest precise account list.
   - [find-buying-signals](../find-buying-signals/SKILL.md) when step 3 found weekly posts on the problem, or the offer is triggered by an event (a round, a new leader): smaller lists, better timing.
   - [track-job-changes](../track-job-changes/SKILL.md) when the user has past champions, buyers or power users: the warmest list there is, and the first one to work.
   - [build-lead-list](../build-lead-list/SKILL.md) for every tactic: it turns accounts into verified contacts.
   - [research-account](../research-account/SKILL.md) for ABM: under about 500 target accounts, or deals large enough that each call deserves prep.
   - [write-first-lines](../write-first-lines/SKILL.md) when the team sends under about 200 new contacts a month, where a researched opening line is worth the credits and the time.
   - [clean-email-list](../clean-email-list/SKILL.md) and [enrich-lead-list](../enrich-lead-list/SKILL.md) when the user already holds a list or a CRM export: verify and fill in what they have before buying more.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - By day 30: one segment, the best-fit 100 accounts, a lead list of one or two buyers per account, first lines on the top 50, the sequence live. By day 60: a weekly signal-led list from buying signals, and a keep or drop call on the first segment from its replies. By day 90: a second segment, and account briefs for the accounts that replied.
   - KPIs the tools can measure again later: accounts in each segment (`rows_available` of the same search), the verified share of each list (`verification_status` from the reveal, `leads_get_email_status` on older rows), and signal volume (`linkedin_search_posts` rows a week for the problem phrases). Replies, meetings and pipeline are the outcome, but they live in the user's sequencer and CRM, not in these tools; name them and let the host read them if it can.
   - **Deliver** one document: the inputs with defaults marked, the ICP as filters, a baseline table (segment, accounts, buyers per account), the gaps in three lines each, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions.

## Judgment

- Volume follows capacity, not the size of the market. Since Google, Yahoo and Microsoft began enforcing their bulk-sender rules, cold mail goes from secondary domains with SPF, DKIM and DMARC set, two or three mailboxes per domain, and, as a rule of thumb for 2026, about 20 to 30 new cold emails a day per mailbox after two to three weeks of warm-up. The list size comes from mailboxes and hours, never from `rows_available`.
- Under about 500 accounts, research each one (account brief, first lines) and skip the big sequence. Over about 5,000, segment and let signals pick who goes first.
- At the founder-led stage the first 90 days are for learning which segment replies. Two small segments of 100 accounts each beat one list of 1,000.
- When the best customers disagree with the ICP the user states, trust the customers and say so.
- Lists covering the EU or the UK follow the [personal data](references/lead-data.md#personal-data) rules; say so in the plan.
- Counts come from one provider's index, not the whole market. Read them as relative sizes between segments.
- The plan sends nothing and writes to no CRM; the [handoff rules](references/lead-data.md#handoff) hold for every tactic. The [lead data](references/lead-data.md) rules (searches, emails, personal data, credits, handoff) are shared by every outbound skill.

## Related skills

- The outbound tactics this plan chooses from: [size-market](../size-market/SKILL.md), [find-lookalike-companies](../find-lookalike-companies/SKILL.md), [find-buying-signals](../find-buying-signals/SKILL.md), [track-job-changes](../track-job-changes/SKILL.md), [build-lead-list](../build-lead-list/SKILL.md), [research-account](../research-account/SKILL.md), [write-first-lines](../write-first-lines/SKILL.md), [clean-email-list](../clean-email-list/SKILL.md), [enrich-lead-list](../enrich-lead-list/SKILL.md).
- Who the buyer is and what they need, before any list: [build-personas](../build-personas/SKILL.md). The players and segments of a market: [map-market](../map-market/SKILL.md). Search demand for the problem: [check-demand](../check-demand/SKILL.md).
- Who the competitors are: [find-competitors](../find-competitors/SKILL.md). How they sell and price: [compare-messaging](../compare-messaging/SKILL.md). Their ads in depth: [research-linkedin-ads](../research-linkedin-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md), [research-meta-ads](../research-meta-ads/SKILL.md) or [research-tiktok-ads](../research-tiktok-ads/SKILL.md).
- A plan across every channel, not only outbound: [create-growth-plan](../create-growth-plan/SKILL.md). First customers with no channel chosen yet: [find-first-customers](../find-first-customers/SKILL.md).
- People posting about the problem, as posts to comment on rather than accounts: [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md).
