---
name: find-buying-signals
description: When the user wants accounts that might buy now, not a cold list. Checks accounts for the signals the tools can see (people posting about the problem on LinkedIn or Reddit, a funding round, a new leader in the buyer's seat, hiring for the role, ads running, traffic growth), scores them and returns a short list with the evidence and a why-now line for each. Also use when the user mentions intent data, buying intent, in-market accounts, companies that just raised, accounts with a new head of marketing, who is showing buying signals, or warm accounts to prospect this week. Past champions who changed jobs go to track-job-changes, LinkedIn posts to comment on to find-linkedin-posts-to-comment, a cold list of people by title to build-lead-list, one account researched before a call to research-account.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Buying signals

A cold list says who could buy; intent says who might buy now. This skill checks accounts for the signals the tools can see (people posting about the problem, a funding round, a new leader, hiring for the role, ads running, traffic growth), scores them, and ends in a short list of accounts with the evidence and a "why now" line for each.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_search_posts` and `leads_get_person` (hosts often add a prefix, for example `mcp__manifold__linkedin_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it this skill cannot map a person to a company or find the buyer; it can still run its LinkedIn, ads and search steps.
- This skill also reads `linkedin_*`, `ads_*` and `seo_*`. If one of those groups is switched off, skip the steps that need it and say which signal is missing from the result.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (what the user sells, the problem in the customers' own words, the buyer titles, the ICP) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Offer and trigger**: what the user sells and the event that makes a buyer need it now. It decides which signals count: a hiring tool cares about a round, an ad tool about ads running, a tool a new leader brings in about a new leader.
- **Problem phrases**: two or three ways buyers describe the problem in their own words, for the post searches.
- **Buyer titles**: the full titles of the person who buys, for the new-leader and hiring searches.
- **Accounts**: a list the user has, the ICP as filters, or the output of [find-lookalike-companies](../find-lookalike-companies/SKILL.md). Default: the ICP filters, checked 20 at a time.
- **Window**: how recent a signal must be. Default: 30 days for posts and new leaders, 180 days for everything else.
- **Budget**: a default run for 20 accounts costs about 10 + 10 x 3 + 4 + 20 x 3 + 3 + 5 x 3 + 4 = 126 credits: the problem-post searches, ten authors mapped to their company, the account search, three checks per account, three new-leader searches, five new leaders mapped, and one people search. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the signals** that fit the trigger, from what the tools can see:
   - People posting about the problem on LinkedIn: the strongest signal, because a person said it.
   - Reddit asks for a tool: strong on wording, but Reddit authors are anonymous and rarely name a company.
   - Funding: a news result from `seo_get_serp`, or the company's own post announcing the round.
   - A new leader in the buyer's seat: their own "starting a new position" post on LinkedIn.
   - Hiring for the role the product serves: a company's or a manager's post saying so.
   - Ads running: `ads_get_advertiser_ads`.
   - Traffic growth: `seo_get_domain_overview` with `history: true`.
   - Not visible to any tool here: the tech stack, job boards, website visits, review-site research, email engagement. If the user asks for them, say so and use the nearest visible signal.
2. **Start from people.** Run [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md) for the problem phrases and take its authors and posts from the window. For Reddit, [find-reddit-threads](../find-reddit-threads/SKILL.md) gives the threads; keep them as evidence of demand and wording, and as an account only when the post names the company. Map each LinkedIn author who could be a buyer to a company: `leads_get_person` with `linkedin_url` set to `https://www.linkedin.com/in/` plus the row's `author` (3 credits when matched, 1 on `NoData`) gives `company_domain` and `title`. On `NoData`, do not try variants of the URL: `linkedin_get_profile` (1 credit) and the employer named in its `bio` will do. Keep the authors whose company fits the ICP.
3. **Build the account pool.** The companies from step 2, plus the user's accounts, or `leads_search_companies` with the ICP filters and `keywords` for the category words (4 credits a page of up to 100 rows), or the lookalike list. Cut to 20 for the checks.
4. **Check account signals.** For each account:
   - `linkedin_get_company_posts` with the company's `linkedin_url` from its search row or record (1 credit a page): announcements inside the window, such as a round, a launch, a new market or a leadership hire. A company's own post saying it is hiring for the role the product serves is the hiring signal.
   - `seo_get_serp` with `"<company name>" raises` as `keyword` (1 credit), when the trigger is a round: a news result names the round and its date. A round with no date in the result does not count.
   - `ads_get_advertiser_ads` on the platform where the account would advertise (1 credit a page): the company name as `advertiser` with `platform: "facebook"` and `active_only: true`, or `platform: "linkedin"`, or the domain with `platform: "google"`. Only Facebook reports `active`; elsewhere read `last_shown`. Read the count of live ads and the newest `first_shown`.
   - Only when growth matters to the offer, and only for the top five: `seo_get_domain_overview` with `history: true` (61 credits) for 12 months of `organic_traffic`.
5. **Check for new leaders** when the trigger is a new buyer. `linkedin_search_posts` with `"starting a new position as <buyer title>"` and `since: "month"` for each of up to three titles (1 credit a page). Map each author to a company as in step 2 and keep those at accounts in the pool or at companies that fit the ICP: they are the new leaders, and the post dates the move. For the buyer at the other top accounts, `leads_search_people` with `company_domains` set to them and the buyer `titles` (4 credits for up to 100 accounts); the [search rules](../create-outbound-plan/references/lead-data.md#searches-and-counts) apply.
6. **Score.** Two points for a buyer at the account posting about the problem inside the window; one point for each other signal inside the window. Keep accounts with two points or more. Write one "why now" line per account from its strongest evidence.
7. **Deliver** a table: account, domain, score, each signal with its evidence and date (post URL, round and the article that dated it, active ad count and newest `first_shown`, leader and their post, hiring post, traffic change), the person to contact (the author of the post, the new leader, or the buyer from step 5), and the "why now" line. Hand the people to [build-lead-list](../build-lead-list/SKILL.md) from its reveal step.

## Judgment

- A person posting about the problem this month outranks any firmographic signal. Contact that person, with the post as the reason; do not go over their head.
- One signal is a reason to look; two are a reason to reach out now.
- A new leader is most open to new tools in the first few months in the seat (a rule of thumb). Reach them inside the window, and lead with their goal, not with congratulations.
- Ads running mean a budget and a growth goal. That is a signal only when the offer serves advertisers or growth teams.
- Signals decay. A post is warm for weeks; a round or a new leader for a few months. Date every row.
- `created_at` on LinkedIn posts is approximate ("3 weeks ago"), so a date near the edge of the window is uncertain.
- Post search is ranked and never complete: an account with no posts found may still have announced a round. Absence is not evidence.
- Checking the same accounts every week for new signals runs on the host's schedule: the host runs this skill again and keeps what it saw last time, since the server keeps no state.

## Related skills

- The accounts to check, when the user has none: [find-lookalike-companies](../find-lookalike-companies/SKILL.md).
- Emails for the people found: [build-lead-list](../build-lead-list/SKILL.md), from its reveal step. An opening line built on their post: [write-first-lines](../write-first-lines/SKILL.md).
- Past champions and buyers who moved to a new company: [track-job-changes](../track-job-changes/SKILL.md).
- Posts to comment on for social selling, rather than accounts to contact: [find-linkedin-posts-to-comment](../find-linkedin-posts-to-comment/SKILL.md). Reddit threads to reply to: [find-reddit-threads](../find-reddit-threads/SKILL.md).
- One account researched in depth before a call: [research-account](../research-account/SKILL.md).
