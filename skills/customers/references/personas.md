# Personas

Two to four buyer personas, each built from evidence rather than imagination: the job titles and seniority that buy, how many of them a company database holds, what they post about on LinkedIn, and what they say more frankly on Reddit. It ends in a persona table where every field is either evidence or marked as an inference.

## Inputs to settle first

- **Current customers**: the titles and companies of the people who bought, if the user has any. This is the best evidence there is; ask for it first.
- **Product and problem**: what it does, and the problem in the buyer's words.
- **Market**: industries and locations. Default: the industry of the user's best customers, in the United States.
- **Candidate roles**: default eight titles guessed from the category and the customers (for a sales tool: founder, head of sales, sales manager, RevOps, account executive, and so on).
- **Budget**: a default run costs about 32 + 36 + 4 + 10 + 8 = 90 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Size the roles.** `leads_search_people` once per candidate title with `titles`, `industries` and `locations` (4 credits per page of 100, 32 for eight titles). Read `rows_available` in the response `meta`: how many people hold that title in the target market. Read the `title` of the rows too: they show the variants people actually use ("Revenue Operations Lead" as well as "RevOps Manager"). For the top three titles, run once per `seniority` level (junior, senior, executive; 36 credits) to see where decision power sits.
2. **Hear the public voice.** `linkedin_search_posts` with the problem in the buyers' words and `since: "year"` (1 credit a page), two to four queries. `linkedin_get_profile` on up to 10 of the authors (1 credit each) confirms their role from the `bio`; keep the posts by people in a candidate role. Read what they are measured on, the goals and tools they name, and what they complain about in public.
3. **Hear the frank voice.** `reddit_search_subreddits` with the category (1 credit) finds the communities where these roles talk. `reddit_search_posts` in the top two with the problem words (1 credit each; relevance sort, filtered to the past year on `created_at`, per the [router](../SKILL.md#reddit-search)), then `reddit_get_comments` on the five busiest threads (1 credit each). Reddit is where people say what they would not post under their name: budgets, who signs, the tools they dropped and why. Take phrases verbatim. For pains ranked across all sources, use [pain points](pain-points.md).
4. **Group into personas.** Cluster the evidence into two to four personas by role and by what they are trying to get done. Each needs: title variants, seniority, the count from step 1 with its filters, where they talk, what they are measured on, their top pains in their words, what triggers a purchase, their objections, and their role in the purchase (decides, uses, influences). Merge two personas that differ only in title.
5. **Deliver** a table with one row per persona: name (a role, not a made-up first name), title variants, seniority, count in the database (with the filters used), role in the purchase, goals and metrics, top pains (quotes with links), objections, triggers, where they talk (subreddits, LinkedIn topics), and the evidence count behind the row. Mark each field as evidence or inference.

## Judgment

- No filler. Age, hobbies and a stock photo add nothing a tool can back; a field with no evidence says "unknown".
- `rows_available` compares roles with each other; it is not the number of buyers ([router](../SKILL.md#signals-and-their-limits)). Add up the title variants that describe one role before comparing.
- When managers far outnumber executives for a role, the person who uses the product and the person who signs are different people. Write both personas, and say which one the message must convince.
- LinkedIn shows the polished, public voice; Reddit the private, frank one. Take goals from LinkedIn and objections from Reddit.
- Current customers beat every search. With 20 real customer titles, start from them and use the tools to size and hear those roles, not to guess new ones.
- The leads tools are business databases. For a consumer product, skip step 1 and build the personas from Reddit and from [pain points](pain-points.md) across TikTok, Instagram and YouTube.
- To turn a persona into a list of people to contact, use [leads lead list](../../leads/references/lead-list.md).
