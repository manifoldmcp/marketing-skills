# First 100 customers

The first customers come from conversations the founder starts by hand, one at a time, with people who already have the problem. This playbook finds where those conversations are happening now, counts how many a week each place can supply, picks two, and routes to the playbooks that work them.

## Inputs to settle first

- **Product and problem**: what it does, and three to five phrases a buyer would use for the problem, not for the product ("chasing late invoices", not "AR automation").
- **ICP**: B2B or B2C, who exactly, and where they are.
- **Offer**: what the founder can give an early customer: a free pilot, a discount, setup done for them.
- **Hours**: a week for outreach and replies. Default: 5.
- **Network**: people the founder already knows in the ICP. No tool sees it, and it is usually where the first ten come from; ask.
- **Competitors**: two or three the buyers use now, including workarounds.
- **Budget**: about 3 + 5 + 3 + 2 + 4 = 17 credits here, plus the playbooks chosen in step 5, which state their own. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Count the people asking on Reddit.** `reddit_search_subreddits` with each problem phrase (1 credit each) for the communities where it comes up. Then `reddit_get_new_posts` on the top five with `since: "7d"` and `match` set to the phrases (1 credit per subreddit): every post about the problem there in the last week. Count the ones that ask for help or a tool. Search would give a sample, not a count; the [reddit router](../../reddit/SKILL.md) says why.
2. **Count the people posting about it on LinkedIn.** `linkedin_search_posts` with each phrase and `since: "month"` (1 credit each). Count the posts by people in the ICP who describe the problem. The search is ranked, so the count is a floor.
3. **Find the unhappy customers of competitors.** `reddit_search_comments` with "<competitor> alternative" at the default relevance sort (1 credit each). People asking what to switch to are the warmest conversations there are.
4. **Size the list (B2B).** `leads_search_companies` with the ICP's industry, headcount and location (4 credits). `meta.rows_available` says whether a hand-picked list of a few hundred accounts is there to be built.
5. **Pick two sources, and open their playbooks.** Rank the sources by conversations a week times fit, and keep the two the founder's hours allow:
   - Reddit threads asking now: [threads to reply](../../reddit/references/threads-to-reply.md), in the communities from [find subreddits](../../reddit/references/find-subreddits.md).
   - LinkedIn posts about the problem: [people posting about your problem](../../linkedin/references/problem-posts.md).
   - Competitors' unhappy customers: [competitor complaints](../../reddit/references/competitor-complaints.md).
   - A hand-picked list of B2B accounts: [lead list](../../leads/references/lead-list.md), with [buying intent](../../leads/references/buying-intent.md) for the ones changing now and [first lines](../../leads/references/first-lines.md) for the opening message.
   - Facebook groups the founder belongs to: [group mining](../../facebook/references/group-mining.md).
6. **Set the weekly routine.** Work back from the target. If one conversation in ten becomes a customer, 100 customers take about 1,000 conversations, and the hours a week decide how many weeks that is. Write the routine per source, for example: answer five threads a day, send twenty first messages a week.
7. **Deliver** a table: source, evidence (count a week or a month, and two example URLs), conversations a week it can supply, the linked playbook, the routine, and the owner. Add a tally sheet (name, source, date of first contact, status, customer yes or no) for the host to keep: it is the KPI until customers exist. Nothing is sent or posted from here.

## Judgment

- Help first. On Reddit, a pitch in a reply is removed and remembered; an answer that solves the problem, naming the product only where the rules allow it, earns the direct message.
- The founder does this personally. At this stage every conversation also teaches what to build and what to say, and automating it loses that.
- Start with the network: the first ten usually come from people the founder knows or can be introduced to.
- A source with nothing in the last week is not a source yet. Do not plan around it.
- Measure conversations started a week, not impressions or followers.
- The one-in-ten rate is a planning assumption. Replace it with the real rate after the first 50 conversations.
