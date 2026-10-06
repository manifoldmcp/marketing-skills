# Buying intent

A cold list says who could buy; intent says who might buy now. This playbook checks accounts for the signals the tools can see (people posting about the problem, a funding round, the stack, ads running, a new leader, traffic growth), scores them, and ends in a short list of accounts with the evidence and a "why now" line for each.

## Inputs to settle first

- **Offer and trigger**: what the user sells and the event that makes a buyer need it now. It decides which signals count: a hiring tool cares about a round, an ad tool about ads running, a migration service about a competitor in the stack.
- **Problem phrases**: two or three ways buyers describe the problem in their own words, for the post searches.
- **Accounts**: a list the user has, the ICP as filters, or the output of [lookalike companies](lookalike-companies.md). Default: the ICP filters, checked 20 at a time.
- **Technologies**: products whose presence in a stack is a signal (a competitor to displace, a tool the product plugs into). No tool reads a company's stack, so this signal comes only from what the user knows or a public post that names the product.
- **Window**: how recent a signal must be. Default: 30 days for posts, 180 days for everything else.
- **Budget**: a default run for 20 accounts costs about 10 + 10 x 3 + 20 x 3 + 4 + 5 x 1 = 109 credits: the post searches, ten authors mapped to their company, three checks per account, one people search and five leaders checked. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the signals** that fit the trigger, from what the tools can see:
   - People posting about the problem on LinkedIn: the strongest signal, because a person said it.
   - Reddit asks for a tool: strong on wording, but Reddit authors are anonymous and rarely name a company.
   - Funding: a round announced in the company's own posts (`linkedin_get_company_posts`), which also dates it. `leads_get_company` holds no funding: its `funding_stage` and `total_funding` are always empty.
   - Stack: no tool reads it (`technologies[]` on `leads_get_company` is always empty). Use what the user knows, or a post or page of the company's that names the product.
   - Ads running: `ads_get_advertiser_ads`.
   - A new leader in the buyer's seat: a company post welcoming the hire, or the leader's own post announcing the role. No record holds a role's start date (`employment_history` on `leads_get_person` is empty).
   - Traffic growth: `seo_get_domain_overview` with `history: true`.
   - Not visible to any tool here: hiring and job posts, website visits, review-site research, email engagement. If the user asks for them, say so and use the nearest visible signal.
2. **Start from people.** Run the LinkedIn steps of [problem posts](../../linkedin/references/problem-posts.md) for the problem phrases and take its authors and posts from the window. For Reddit, [threads to reply to](../../reddit/references/threads-to-reply.md) gives the threads; keep them as evidence of demand and wording, and as an account only when the post names the company. Map each LinkedIn author who could be a buyer to a company: build the profile URL (`https://www.linkedin.com/in/` plus the row's `author`) and call `leads_get_person` with it (3 credits, 1 on `NoData`); it gives `company_domain` and the title. Keep the authors whose company fits the ICP.
3. **Build the account pool.** The companies from step 2, plus the user's accounts, or `leads_search_companies` with the ICP filters (4 credits a page), or the lookalike list. Cut to 20 for the checks.
4. **Check account signals.** For each account:
   - `leads_get_company` (1 credit): its LinkedIn page URL, `description` and `employees`.
   - `linkedin_get_company_posts` with that `url` (1 credit a page): announcements inside the window, such as a round, a launch, a new market or a leadership hire. A round in the posts is the funding signal, with its date. A company's own post saying it is hiring is visible here as text; that is the only hiring signal there is.
   - `ads_get_advertiser_ads` with `active_only: true` on the platform where the account would advertise (1 credit a page): the company name as `advertiser` with `platform: "linkedin"` or `platform: "facebook"`, or the domain with `platform: "google"`. Only Facebook applies `active_only`; on the other libraries keep the rows whose `active` is true or whose `last_shown` is recent. Read the count of active ads and the newest `first_shown`.
   - Only when growth matters to the offer, and only for the top five: `seo_get_domain_overview` with `history: true` (61 credits) for 12 months of `organic_traffic`.
5. **Check for new leaders** when the trigger is a new buyer. `leads_search_people` with `company_domains` set to the accounts and the buyer `titles` (4 credits for up to 100 accounts) names the buyer at each. For the top five, `linkedin_search_posts` with the buyer's full name as `query` and `since` covering the window (1 credit each), keeping only rows whose `author` is their profile handle: a post announcing the new role dates it. A leadership hire in the company posts from step 4 counts the same.
6. **Score.** Two points for a buyer at the account posting about the problem inside the window; one point for each other signal inside the window. Keep accounts with two points or more. Write one "why now" line per account from its strongest evidence.
7. **Deliver** a table: account, domain, score, each signal with its evidence and date (post URL, round and where it was dated, technology, active ad count and newest `first_shown`, leader and start date, traffic change), the person to contact (the author of the post, or the buyer from step 5), and the "why now" line. Hand the people to the [lead list](lead-list.md) from its reveal step.

## Judgment

- A person posting about the problem this month outranks any firmographic signal. Contact that person, with the post as the reason; do not go over their head.
- One signal is a reason to look; two are a reason to reach out now.
- A round without a date is weak: a Series B may be two years old. Count it only when the company's posts or a source the host can open date the round inside the window.
- A stack signal from a post or a page can be out of date: companies drop tools without saying so. Treat it as likely, and confirm it on the call.
- Ads running mean a budget and a growth goal. That is a signal only when the offer serves advertisers or growth teams.
- Signals decay. A post is warm for weeks; a round or a new leader for a few months. Date every row.
- `created_at` on LinkedIn posts is approximate ("3 weeks ago"), so a date near the edge of the window is uncertain.
- Checking the same accounts every week for new signals is the [monitoring](../../monitoring/SKILL.md) group's job: the host schedules it and keeps what it saw last time.
