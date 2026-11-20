# Field notes

What Manifold has seen in its own reads of Reddit through the `reddit_*` tools. Small samples, chosen by hand: read them as evidence of how communities and the tools behave, not as Reddit-wide rates. Newest first. For Reddit's share of AI citations in Manifold's own answer runs, see [ai-search](../../ai-search/SKILL.md).

## Twenty communities' rules (15 October 2026)

`reddit_get_subreddit` on 20 communities a B2B or consumer brand would want: SaaS, startups, Entrepreneur, smallbusiness, marketing, digital_marketing, SEO, PPC, sales, ecommerce, shopify, SideProject, EntrepreneurRideAlong, Emailmarketing, socialmedia, webdev, CRM, sysadmin, BuyItForLife, SkincareAddiction. 17 credits (three were cached). Rules read and classified by hand; a different reader could place a borderline rule differently.

**The user's own product:**

| Stance | Count | Communities |
|---|---|---|
| Not allowed anywhere | 10 | marketing (permanent ban), digital_marketing, SEO, PPC, sales (even when asked for a recommendation), ecommerce (ban without warning), shopify, socialmedia, BuyItForLife, SkincareAddiction (no brand-affiliated accounts at all) |
| Only in a set-aside thread | 3 | startups (monthly share thread), Entrepreneur (weekly threads), webdev (Showoff Saturday, projects not products) |
| In a relevant reply, with conditions | 4 | SaaS (1 mention or 3 links per 60 days, disclosed, alts counted; marketing and Reddit-automation tools banned), smallbusiness (relevant replies only; promotional-looking mentions removed since June 2026), Emailmarketing (high bar, disclosed), sysadmin (in an existing discussion, disclosed) |
| No rules published | 3 | SideProject and CRM (rules empty), EntrepreneurRideAlong (three generic rules) |

**Other rules:**

- **AI-written content removed:** 11 of the 18 with rules (SaaS, Entrepreneur, marketing, digital_marketing, sales, ecommerce, shopify, Emailmarketing, webdev, sysadmin, SkincareAddiction). Several ban the account; SaaS includes edited copy-paste.
- **"Pain points", validation or beta-tester posts banned:** 5 (smallbusiness, PPC, ecommerce, shopify, marketing); SaaS and startups send them to moderator approval or a feedback thread.
- **Posted account minimums:** 5 (marketing 30 days and 300 karma; shopify 30 days, 20 post and 20 comment karma; sales 10 karma earned in r/sales; sysadmin 24 hours; Entrepreneur prior comments there).
- **Links:** SEO removes all links; ecommerce and shopify ban external links; digital_marketing bans link posts; startups allows only links the poster has no affiliation with.
- **AMAs need approval:** 5. **Tell companies to buy Reddit ads:** 4 (marketing, sysadmin, BuyItForLife, SkincareAddiction). **Cite the 9:1 rule:** 1 (webdev).

**Data:** `subscribers` was null on all 20 (the tools have since dropped the field) and `submit_text` on all 20. `weekly_active_users` ranged from 14,489 (Emailmarketing) to 1,827,931 (BuyItForLife); `weekly_contributions` from 593 to 20,083 (SideProject).

## One week of r/CRM (8 to 15 October 2026)

`reddit_get_new_posts` on SaaS, smallbusiness and CRM, `since: "7d"`, `match` on crm, hubspot, pipedrive, salesforce, one page each: 3 credits, 21 rows.

- **One page does not reach far in a busy community.** Coverage reached back 7.7 hours on r/SaaS and 9.2 hours on r/smallbusiness, 6 days on r/CRM. All three came back `complete: false`.
- **Half of r/CRM's new posts were vendors'.** Of 20 posts, 10 were written by people building or selling a CRM or CRM service (pitches, "roast my CRM", "we're the team behind X, curious how you...", a pilot recruitment post, a disclosed implementer's advice). One more read as seeded: a buyer question naming two little-known vendors as options "that have come up on Reddit". About six were genuine user questions.
- **Pitches did badly; useful posts did not.** Pure pitches scored 0 to 3 with upvote ratios down to 0.33. A brand account's (nutshell_crm) non-promotional discussion post on what Outlook integration should mean scored 7 with a 0.89 ratio and 12 comments. The busiest post was a family business asking "which CRM do you recommend" (78 comments).

## One "which CRM" thread (15 October 2026)

`reddit_get_comments` on that thread, first page: 25 of 78 comments, 1 credit.

- 8 of 25 came from vendors, implementers or resellers: two disclosed (an Attio partner, a GoHighLevel builder), the rest naming their own product or linking their own site, blog or directory without saying so.
- Every comment scored 1 or 2 after about a day: score did not separate good from bad yet.
- The poster replied to two: the disclosed partner who asked where the leads came from, and a no-product answer on what to fix before buying. No product-plus-link reply got a response.
- Several replies read as AI-written (even structure, generic hedges). The community publishes no rule against it.

## How the search tools behaved (15 October 2026)

6 credits.

- `reddit_search_posts` "hubspot alternative", `time_range: "month"`: relevance sort, 7 rows, all on topic; `new` sort, 6 rows, all containing "hubspot" but 1 on topic.
- `reddit_search_comments` "hubspot alternative": `time_range` "week" and "all" returned the identical 9 rows, all from the past three weeks; 5 were vendors placing their own product, 1 was off topic (a Medicare thread). "switched from pipedrive" with `sort: "top"` and `time_range: "year"` returned comments from 2023 to 2026: the vendor ignores `time_range`, and the tool now drops the rows outside it.
- `reddit_search_subreddits` "crm for small business", year: 5 communities from 7 posts, `subscribers` null on each (since dropped).
- `upvote_ratio` was null on every search row and filled on check-in rows.

27 credits in all for these notes.
