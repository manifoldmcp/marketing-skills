# A Reddit plan

When the user wants a plan rather than one list: the few communities where buyers ask for help, where the brand and its competitors stand there, what the user can bring, and a 30-60-90 day plan. Communities reward people who help and ban people who advertise; the plan is built around that.

## Settle first

Ask in one message; for anything unanswered, use the default and say so.

- **Goal:** signups, customer research, or being the answer people recommend (which also feeds Perplexity and Google, not ChatGPT since August 2026; see [Google and AI answers](search-and-ai.md)). Default: be recommended in the three communities where buyers ask.
- **Stage:** before launch there is nothing to mention, so the plan is listening and participation only.
- **Who posts, from which account:** the founder or a named employee from a personal account with history is the default. A new account or a brand account changes the first 30 days ([rules](rules.md#the-account)).
- **Hours a week:** default 3, about ten careful replies. Set volume from hours, never from the thread count; twenty rushed replies read as spam and trip the filters.
- **Competitors:** two or three; default the ones Reddit names most in the baseline.
- **Budget:** about 33 credits of research, about 114 with the AI check.

## Baseline

About 23 credits.

- **Where the topic lives:** `reddit_search_subreddits` on three phrasings of the buyer's problem with `time_range: "year"`, then `reddit_get_subreddit` on the top 8 for weekly visitors, weekly contributions and rules ([communities](communities.md)).
- **How busy it is:** `reddit_get_new_posts` on those 8, `since: "7d"`, `match` on the problem terms, the brand and competitor names (8 credits). The brand and competitor matches are share of voice inside the chosen communities at no extra cost; complete only where `coverage[]` says so.
- **Share of voice across Reddit:** `reddit_search_comments` on the brand and each competitor (1 credit each), reading how often and in what tone each is named, and by whom. A sample: compare the brands with each other, never with a total.

## Gaps

About 10 credits more, or about 91 with the AI check.

- **Unanswered demand:** check-in posts asking for help or a tool with fewer than 5 `comments`.
- **Why rivals win:** `reddit_get_comments` on 5 threads where a competitor is recommended. Recommended by users, or by its own staff and resellers? A space where competitors are recommended mostly by their own people is open: a genuine user recommendation beats a vendor's.
- **Google and AI:** `seo_get_serp` on 5 category keywords for discussions rows; if Google shows Reddit, `aeo_run_ai_answers` on 3 prompts and count reddit.com citations per engine.
- **Rules:** the communities where the intended action (a mention, a link, a launch post) is not allowed, from [rules](rules.md).

## Choosing the work

- The rules check, always, before the first post ([rules](rules.md#the-rules-check)).
- Weekly threads to answer, when the check-in shows 5 or more relevant posts a week across the chosen communities ([threads and replies](threads.md)).
- Pain-point mining, when the user is pre-launch or needs messaging ([pain points](pain-points.md)).
- Threads Google and AI engines show, when the category's SERPs or Perplexity and AI Mode answers carry Reddit ([Google and AI answers](search-and-ai.md)).
- Complaints about competitors and "alternative to X" threads: [competitor-intel](../../competitor-intel/SKILL.md). A scheduled watch of the communities and the brand: [brand-monitoring](../../brand-monitoring/SKILL.md).

## The 30-60-90 days

| Period | Work | KPI the tools re-measure |
|---|---|---|
| Days 1 to 30 | Rules read for 3 to 5 communities; account history built with plain helpful comments, no product, no links; the weekly reply list started, at the volume the hours allow | Replies still visible logged out; `score` on the user's replies (`reddit_get_comments` on each thread) |
| Days 31 to 60 | Product mentions only where a thread asks and the rules allow, disclosed; one useful post per community where allowed (data, a lesson with numbers, never an ad); a competitor complaints pass | Brand mentions by other people in the chosen communities (`reddit_get_new_posts` with `match` on the brand, run weekly) |
| Days 61 to 90 | The threads Google and Perplexity show; an AMA or founder story where moderators approve it; more of whatever earned the best reply scores | Reddit threads naming the user among AI citations and in Google's discussions rows; brand mentions in the same search queries as the baseline |

Give each period an owner and three first actions for this week. The 30-day KPI is activity and reply score, never signups; the 90-day KPI is being named by other people. Signups from Reddit sit in the user's analytics, not in these tools: ask for UTM links on anything the user posts.

## Reddit ads

For an organic marketer, what matters about the paid side (campaign planning and creative go to [paid-ads](../../paid-ads/SKILL.md)):

- **Ads are the route many communities point to.** 4 of the 20 communities we read tell companies to buy ads rather than post. Paying is how to reach a community that bans the product mention.
- **Ads are posts.** They take comments by default, in public, and Reddit users comment bluntly. Leave comments on only if someone will answer them honestly within the day; an ignored thread of complaints under an ad is worse than none.
- **Targeting** can be by community, by keyword and conversation (ads placed in threads about a topic), by interest, and by the advertiser's own audiences. Community and keyword targeting follow the same list [communities](communities.md) produces.
- **Reddit Pro** is free for businesses: keyword trend tracking (Trends, launched at CES, January 2025), post analytics, and boosting an organic post into an ad. Reddit Community Intelligence (announced at Cannes Lions, June 2025, in alpha) added a listening product (Reddit Insights) and ad add-ons that show users' positive posts about the brand under the ad (Conversation Summary Add-ons).
- **Measurement** needs the Reddit Pixel or Conversions API on the user's site; Reddit's reports otherwise show clicks, not signups.
- **No public library of Reddit ads** exists outside political ads, and no Manifold tool reads Reddit ads, so competitor Reddit ads cannot be researched here.
- Ads exempt nobody: the brand's staff accounts still follow every community's rules when they comment.
