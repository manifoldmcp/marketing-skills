# Personas

Two to four buyer personas built from evidence, where every field is either evidence or marked as inference. A persona earns its place only if it changes a message, a channel or a product decision; age, hobbies and a stock photo add nothing a tool can back.

## Evidence, in order of strength

1. **Current customers.** The titles and companies of the people who bought, why they chose, how they found the product. With 20 real customer titles, start from them and use the tools to size and hear those roles, not to guess new ones. Ask for this first.
2. **What they say in public and in private.** LinkedIn for goals, metrics, tools and wins (the career-safe voice); Reddit for budgets, who signs, what they dropped and why (the frank voice).
3. **How many of them exist**, from a company database.
4. **Inference**: the model's own reasoning, and [synthetic respondents](synthetic-respondents.md). Label it as such.

## Sizing roles (business buyers)

- `leads_search_people` once per candidate title with `titles` (the full title, no `seniority`), `industries` and `locations` (4 credits a call). `rows_available` is how many people hold that title in the target market; the rows' `title` field shows the variants people actually use ("Revenue Operations Lead" as well as "RevOps Manager").
- Industries must be LinkedIn's names, matched exactly: copy the `industry` string from `leads_get_company` on two of the user's customers (1 credit each).
- To see where decision power sits, run the strongest functions once per `seniority` level (junior, senior, executive) with the bare function word as `titles` (`["Sales"]`, not `["Head of Sales"]`). `seniority` combines with `titles` into phrases ("VP Sales"), so a full title plus a seniority matches almost nothing.
- `rows_available` compares roles with each other. It is not the number of buyers or a market size ([market-research](../../market-research/SKILL.md) sizes markets). Add up the variants of one role before comparing.
- When managers far outnumber executives in a role, the user and the signer are different people. Write both, and say which one the message must convince.

## Hearing them

- `linkedin_search_posts` with the problem in buyer words and `since: "year"` (1 credit a page), two to four queries. `linkedin_get_profile` on up to ten authors (1 credit each) confirms the role from the bio; keep posts by people in a candidate role.
- `reddit_search_subreddits` with the category (1 credit) finds the communities where the roles talk; search the top two with the problem words and read the five busiest threads.
- Take phrases verbatim, with links. For pains ranked across sources, run [pain points](pain-points.md) and reuse its rows.

Default run about 80 credits: 8 title sizes (32), seniority splits (24), two company lookups, LinkedIn searches and profiles, Reddit.

## Consumer products

The leads tools are business databases. For a consumer product skip the sizing and build personas from the situation and the job ("a working parent deciding dinner at 5pm") rather than demographics, using Reddit, TikTok, Instagram and YouTube comments. Pew's platform demographics ([sources](sources.md#who-is-talking)) say who is likely behind each source.

## Each persona holds

Name (a role or a situation, never a made-up first name), title variants, seniority, count with the filters used, role in the purchase (decides, uses, influences, blocks), goals and what they are measured on, top pains in their words with links, objections, purchase triggers, where they talk (subreddits, LinkedIn topics, groups), and the evidence count behind the row. Mark each field evidence or inference; a field with neither says "unknown". Merge two personas that differ only in title. Add an anti-persona when the evidence shows who is loud but never buys; it goes to section 7 of `.agents/product-marketing.md`.

Every quote carries its link; leave usernames out of anything the user will publish. Never contact anyone found here: a list of people in a persona belongs to [outbound](../../outbound/SKILL.md), which holds the personal-data rules.
