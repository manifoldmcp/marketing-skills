# Signals

A cold list says who could buy; a signal says who might buy now. This page covers buying signals across accounts and job changes of people the user already knows. Checked October 2026.

## What the evidence says, and does not

- **Timing is the commonest claim in outbound and the least measured.** Instantly's 2026 benchmark names hiring, funding, launches and site visits as drivers of its best campaigns but publishes no lift. Figures of "15% to 25% reply rates" for signal-based email trace back to vendor round-ups, not to data. No independent study of intent data or job-change triggers was found for 2025-2026. Treat signals as a way to choose who goes first and what to say, and measure the lift on the user's own sends against a cold control.
- **The strongest signal is a person saying it.** A buyer posting about the problem this month outranks any firmographic or third-party intent score: the reason to write is in their own words, and it can be quoted.
- **Signals decay.** A post is warm for weeks; a round or a new leader for a few months. Date every row.
- **One signal is a reason to look; two are a reason to write now.** Score two points for a buyer at the account posting about the problem inside the window and one for each other signal; keep accounts on two or more.

## What the tools can see

| Signal | How | Cost | Trap |
|---|---|---|---|
| A buyer posting about the problem | `linkedin_search_posts` with the problem in the buyer's words, `since: "month"`; map each author to a company with `leads_get_person` by profile URL | 1 a page, 9 a person | Ranked, never complete; `created_at` is approximate ("3 weeks ago") |
| Reddit asking for a tool | The [reddit](../../reddit/SKILL.md) skill | 1 a call | Authors are anonymous; an account only when the post names the company |
| Funding | `seo_get_serp` with `"<company>" raises` as the keyword; the company's own posts | 1 | A round with no date in the result does not count |
| A new leader in the buyer's seat | `linkedin_search_posts` for `"starting a new position as <title>"`, or `title_start` on `leads_search_people` rows | 1 a page, 12 a page of 100 | Records lag a move by weeks |
| Hiring for the role the product serves | `linkedin_get_company_posts` on the account | 1 a page | Job boards are not visible here |
| Ads running | `ads_get_advertiser_ads`, Facebook with `active_only: true`, or LinkedIn or Google | 1 a page | Only Facebook reports `active`; elsewhere read `last_shown` |
| Traffic growth | `seo_get_domain_overview` with `history: true` | 71 | Worth it for the top five only, and only when growth matters to the offer |
| Headcount growth | `headcount_growth[]` on company rows | free with the row | Empty on most small companies (6 of 40 in our run) |

Not visible to any tool here: the tech stack, website visits, review-site research, email engagement, job boards. Say so when asked and use the nearest visible signal. Absence of a found post is not evidence: post search is ranked, so an account with nothing found may still have announced a round.

## Reading them

- **Contact the person who posted**, with the post as the reason; do not go over their head. A post asking for help with the problem the user solves is the warmest cold email there is.
- **A new leader** is, by rule of thumb, most open to new tools in the first few months in the seat. Lead with their goal, not with congratulations. In our run 3 of 15 dated marketing leaders at small software companies had started in the past 12 months, so a new-leader trigger yields a few accounts per hundred, not a list.
- **Ads running mean a budget and a growth goal.** A signal only when the offer serves advertisers or growth teams.
- **Re-checking weekly** runs on the host's schedule: the server keeps no state, so the host stores what it saw last time and compares.
- **Deliver** a table: account, domain, score, each signal with its evidence, link and date, the person to contact, and a one-line "why now". Then hand the people to the reveal step in [lists](lists.md#a-lead-list).

## Job changes

A champion, buyer or power user who moves to a new company knows the product and is choosing tools in a new seat. Vendors of champion-tracking tools claim large lifts; no independent figure was found, but it is the warmest list most companies have and the first one to work.

- **Check each known person** with `leads_get_person` by full LinkedIn profile URL (9 credits, 1 on `NoData`), against the company the host has on file. The same `company_domain` means still there (a new `title` is a promotion, also a reason to write); a different one means a mover; another domain of the same company is not a move. Without a profile URL, a name-and-domain match only says they are still there, and `NoData` means unknown.
- **Size the new companies** with `leads_get_company` (1 credit each) and mark whether they fit the ICP. A new company that is already a customer is an expansion lead for its account owner.
- **Date the move** from `title_start` on the record, or a "starting a new position" post. As a rule of thumb, tools come in during the first three months; after about six the move is background.
- **Reveal** only for movers at companies that fit. A default run of 200 people with 20 movers is about 1,980 credits.
- **Write as someone they know.** The shared history is the reason for the email; a mover dropped into a cold sequence wastes it.
- **The other side is churn risk.** A customer account whose champion left needs a new one; flag it to the user.
- **Records lag.** "Still there" can be wrong for weeks. Re-check quarterly; a person record is cached 30 days, so a re-check inside a month is free and no newer.

## Without the connector

Search LinkedIn for the problem phrases and for "starting a new position as" plus the buyer title, read the target accounts' LinkedIn pages and news by hand, and keep the date and link beside every signal. Say that a hand search is a sample, not coverage.
