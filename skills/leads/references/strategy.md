# Outbound strategy

An outbound strategy decides who to contact, why now, and how many the team can work well, before anyone builds a list. It reads the ICP from the best customers, counts the market, checks where the signals are, and ends in a 90-day plan that this group's playbooks execute, not in a list of leads.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: qualified meetings or pipeline per month. Default: 8 qualified meetings a month by day 90.
- **Stage**: founder-led sales with under 20 customers, or a sales team with a motion that works. Default: founder-led.
- **ICP**: industry, size, region and the buyer's title. Default: read from the best customers in step 2.
- **Best customers**: three to five customer domains, the ones that bought fastest and stayed. Without them the ICP is a guess; ask for them even when the user states an ICP.
- **Budget**: credits for the research (this playbook costs about 80) and for lists (a 50-contact lead list is about 330), and the sending capacity. Default: 2,000 credits a month and one sequencer with two warmed mailboxes.
- **Team**: who writes and sends, which sequencer and CRM they use, and hours a week. Default: the founder, 3 hours a week.
- **Competitors**: two or three. Default: ask; if the user does not know, the [competitors](../../competitors/SKILL.md) group finds them.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 45 credits.
   - `leads_get_company` on three best customers (10 credits each). Read `industries[]`, `keywords[]`, `employees`, `location`, `funding_stage` and `technologies[]`, and write the ICP as search filters: the keywords and industries they share, the employee band that covers them, the regions.
   - `leads_search_companies` with those filters (10 credits). `rows_available` is the account universe. Read the first page of names: if fewer than about 15 of the first 20 fit, the keywords are too loose; tighten them before anything else.
   - `leads_search_people` with the buyer `titles` and `company_domains` set to the domains from that first page (1 credit for up to 100 accounts). Buyers per account and the share with `has_email: true` say how many people each account yields and how many are reachable by email.
   - The [router's stub rules](../SKILL.md#stubs-and-counts) apply: count, do not page.
3. **Gaps.** About 35 credits more.
   - Segments: `leads_search_companies` with the same keywords for two or three other employee bands or regions (10 credits each). Compare where the accounts are with where the customers are. A segment the customers already cluster in is where to start; a large segment with no customers yet is a test, not the plan.
   - Signals: `linkedin_search_posts` with two phrases of the problem in the buyer's words and `since: "month"` (1 credit a page each). If buyers post about the problem every week, signal-led outbound has enough volume to run weekly.
   - Competitors: `ads_get_advertiser_ads` with `platform: "linkedin"` on each competitor (1 credit a page). The offer and the pain they pay to put in front of the same buyers show which angle is crowded. The full teardown is the competitors group's job.
   - Displacement: whether a competitor's product appears in the best customers' `technologies[]`. If it is a detectable tool, "accounts using a competitor" is a list, but `leads_search_companies` cannot filter by technology, so each account costs a `leads_get_company` to check.
4. **Tactics.** Choose two or three from this group and say why each fits the numbers:
   - [Market size](market-size.md) when the ICP is not settled or the universe may be too small: counts per segment decide where to start.
   - [Lookalike companies](lookalike-companies.md) when there are three or more good customers: the fastest precise account list.
   - [Buying intent](buying-intent.md) when step 3 found weekly posts on the problem, or the offer is triggered by an event (a round, a new leader, a stack change): smaller lists, better timing.
   - [Lead list](lead-list.md) for every tactic: it turns accounts into verified contacts.
   - [Account brief](account-brief.md) for ABM: under about 500 target accounts, or deals large enough that each call deserves prep.
   - [First lines](first-lines.md) when the team sends under about 200 new contacts a month, where a researched opening line is worth the credits and the time.
   - [List cleaning](list-cleaning.md) and [enrichment](enrichment.md) when the user already holds a list or a CRM export: work what they have before buying more.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - By day 30: one segment, the best-fit 100 accounts, a lead list of one or two buyers per account, first lines on the top 50, the sequence live. By day 60: a weekly signal-led list from buying intent, and a keep or drop call on the first segment from its replies. By day 90: a second segment, and account briefs for the accounts that replied.
   - KPIs the tools can measure again later: accounts in each segment (`rows_available` of the same search), the verified share of each list (`verification_status` from the reveal, `leads_get_email_status` on older rows), and signal volume (`linkedin_search_posts` rows a week for the problem phrases). Replies, meetings and pipeline are the outcome, but they live in the user's sequencer and CRM, not in these tools; name them and let the host read them if it can.
   - **Deliver** one document: the inputs with defaults marked, the ICP as filters, a baseline table (segment, accounts, buyers per account, `has_email` share), the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and the three first actions.

## Judgment

- Volume follows capacity, not the size of the market. About 30 to 50 new cold emails a day per warmed mailbox keeps deliverability safe; the list size comes from mailboxes and hours, never from `rows_available`.
- Under about 500 accounts, research each one (account brief, first lines) and skip the big sequence. Over about 5,000, segment and let signals pick who goes first.
- At the founder-led stage the first 90 days are for learning which segment replies. Two small segments of 100 accounts each beat one list of 1,000.
- When the best customers disagree with the ICP the user states, trust the customers and say so.
- Lists covering the EU or the UK follow the router's [personal data](../SKILL.md#personal-data) rules; say so in the plan.
- Counts come from one provider's index, not the whole market. Read them as relative sizes between segments.
