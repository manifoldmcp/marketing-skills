# Strategy

An outbound plan decides who to contact, why now and how many the team can work well, before anyone builds a list. It ends in a 90-day plan, not in a list of leads. The last section covers a company with few or no customers, where outbound is one of several ways to start conversations by hand.

## Inputs

Ask in one message; for anything unanswered, use the default and say so.

- **Goal:** qualified meetings a month. Default 8 by day 90.
- **Stage:** founder-led with under 20 customers, or a team with a motion that works. Default founder-led.
- **Best customers:** three to five domains, the ones that bought fastest and stayed. Without them the ICP is a guess; ask even when the user states an ICP, and when the customers disagree with the stated ICP, trust the customers and say so.
- **Capacity:** mailboxes, sending domains and hours a week. Default two warmed mailboxes on one secondary domain and the founder at three hours a week. This, not the market, sets the volume ([deliverability](deliverability.md#setup-that-still-holds)).
- **Markets:** the countries the buyers are in, which decides the legal basis ([legal](legal.md)).
- **Competitors:** two or three; [competitor-intel](../../competitor-intel/SKILL.md) finds them if the user does not know.

## Baseline

About 30 credits with Manifold.

- **ICP as filters:** `leads_get_company` on the best customers (1 each), written as the shared `industry` strings, employee band and regions.
- **The universe:** `leads_search_companies` with those filters; `rows_available` counts it (4). Read the first 20 rows; fewer than 15 fitting means the filters need work. Repeat for two or three other bands or regions to compare segments (4 each). A segment the customers already cluster in is where to start; a large one with no customers yet is a test.
- **Buyers per account:** `leads_search_people` with the buyer `titles` inside 100 of those domains (4). In our run about a third of small accounts had a findable marketing leader.
- **Signal volume:** `linkedin_search_posts` on two problem phrases and on "starting a new position as" plus the buyer title, `since: "month"` (1 a page each). Weekly posts on the problem mean signal-led outbound has enough to run weekly.
- **The crowded angle:** `ads_get_advertiser_ads` on each competitor on LinkedIn (1 a page): the pain they pay to put in front of the same buyers.

Counts come from one provider's index; read them as relative sizes between segments.

## Choosing tactics

| When the numbers say | Tactic |
|---|---|
| The user has past champions, buyers or power users | [Job changes](signals.md#job-changes): the warmest list, worked first |
| Three or more good customers | [Lookalikes](lists.md#lookalike-companies): the fastest precise account list |
| Buyers post about the problem weekly, or the offer follows an event | [Signals](signals.md): smaller lists, better timing |
| Under about 500 target accounts, or large deals | [Account research](accounts.md) on every reply, and first lines per person |
| Under about 200 new contacts a month | [First lines](messaging.md#first-lines) per lead; above that, by segment |
| The user already holds a list or CRM export | [Clean](deliverability.md#cleaning-a-list) and [enrich](lists.md#enriching-the-users-sheet) it before buying more |
| Every tactic | [A lead list](lists.md#a-lead-list) turns accounts into verified contacts |

Under about 500 accounts, research each one and skip the big sequence; over about 5,000, segment and let signals pick who goes first. At the founder-led stage two segments of 100 accounts beat one list of 1,000: the first 90 days are for learning which segment replies.

## The 30-60-90 day plan

| Period | Work | KPI the tools can re-measure |
|---|---|---|
| Days 1 to 30 | Sending domain set up and ramping; job changes checked; one segment, the best-fit 100 accounts, one or two buyers each, first lines on the top 50; a three- or four-step sequence live | Verified share of the list (`verification_status`); bounce rate under 2% from the sequencer |
| Days 31 to 60 | A weekly signal-led list; keep or drop the first segment on its replies; a second segment tested at the same size | Signal volume (`linkedin_search_posts` rows a week); replies per segment |
| Days 61 to 90 | Scale the segment that replied within mailbox capacity; account briefs for every account that replied; a second domain if volume needs it | Accounts left in the winning segment (`rows_available` of the same search); meetings a month |

Replies, meetings and pipeline are the outcome, but they live in the user's sequencer and CRM, not in these tools; name them and let the host read them if it can. Give each period an owner and three first actions for this week. Benchmark the user's replies against about 3% for sales outreach ([messaging](messaging.md#benchmarks)), and judge a segment only after a few hundred recipients: at 3%, 100 sends is three replies.

## First customers

With few or no customers, the first ones come from conversations the founder starts by hand with people who already have the problem. Outbound is one source among several.

- **Start with the network.** The first ten usually come from people the founder knows or can be introduced to. No tool sees it; ask.
- **Count where the conversations are, per week:** Reddit communities where the problem comes up ([reddit](../../reddit/SKILL.md) reads every new post in a window, not a ranked sample), LinkedIn posts by people in the ICP describing it (`linkedin_search_posts`, a floor, since search is ranked), people asking for an alternative to a competitor ([competitor-intel](../../competitor-intel/SKILL.md)), and, for B2B, whether a hand-picked list of a few hundred accounts exists (`rows_available`). A source with nothing in the last week is not a source yet.
- **Pick two** the founder's hours allow, and set a routine per source (answer five threads a day, send twenty first messages a week).
- **Help first.** On Reddit a pitch in a reply is removed and remembered; an answer that solves the problem earns the direct message.
- **The founder does this personally.** Every conversation also teaches what to build and what to say; automating it loses that.
- **Work back from the target.** If one conversation in ten becomes a customer, 100 customers take about 1,000 conversations. The one-in-ten is a planning assumption; replace it with the real rate after the first 50.
- **The KPI is conversations started a week**, kept in a tally (name, source, first contact date, status, customer yes or no) by the host. A plan across every channel is [growth-strategy](../../growth-strategy/SKILL.md).
