# Choosing channels

Which channels to be on is a question about where buyers already are, whether demand exists to capture or has to be made, and what the team can make every week. This page holds the signals that show a channel is worth it, the traps in reading them, and how the mix changed in 2025-2026. Checked October 2026.

## What changed in 2025-2026

- **Google search sends fewer clicks.** 68.0% of US Google searches ended without a click in January to April 2026, against 60.5% in 2024 and about 49% in 2019; clicks of any kind (organic, paid, Google's own properties) fell 22.9% from 2024 (SparkToro, June 2026, Similarweb desktop and mobile-browser panel; the Google app is not in it, so the real rate is likely higher). AI Mode was reached on only 0.34% of searches. Search is still the largest referrer for most sites; what fell is how much a ranking is worth.
- **AI assistants send almost no traffic but shape the list.** LLM referrals were 0.24% of sessions across 54 sites, and about 90% of those sites had under 0.6% (Amsive, September 2025). ChatGPT sent 76.9% of AI chatbot referrals in April 2026, Gemini 9.0%, Perplexity 7.7%, Copilot 3.8%, Claude 2.7% (Statcounter, May 2026), with ChatGPT's share falling for three months. The value is being named when buyers build a shortlist; [ai-search](../../ai-search/SKILL.md) owns how.
- **Ads now run inside AI answers.** ChatGPT shows labelled ads on its Free and Go tiers (US since February 2026, more markets since August), and Google runs ads in AI Overviews and AI Mode; Perplexity has been phasing ads out since late 2025. Neither company says ads change which brands the answer names. Treat them as a paid placement to test, not a route into the organic answer ([paid-ads](../../paid-ads/SKILL.md)).
- **Paid search got dearer for B2B.** In Dreamdata's 2025 customer data, non-branded Google search CPC rose 29% and CTR fell 26%, and its share of B2B ad budgets fell from 37% to 33% while LinkedIn's rose to 41% (vendor data, March 2026; companies and spend not disclosed).
- **Organic social reach per post moved by platform, not uniformly.** Instagram's median engagement rate fell about 26% from 2024 to 2025, LinkedIn's about 5%, Threads' about 18%; X's rose about 44% and Facebook's about 11% (Buffer, 52M+ posts by Buffer users, rates per reach). "Organic is dead" is wrong; "organic is uneven and small per post" is right. Details in [benchmarks](benchmarks.md).
- **Reposting is penalised.** Meta and YouTube both tightened rules on reused content in July 2025 ([repurposing](repurposing.md)). Cross-posting one file everywhere is no longer a cheap channel.

## Signals a channel is worth it

Each source counts in its own unit with its own floor. Never add searches, prompts, comments and views together: a channel clears its own floor or it does not, and the ranking comes from demand, competition and effort together.

| Channel | Worth it when | Read it with | Trap |
|---|---|---|---|
| Search | Non-brand category keywords add up to more than about 1,000 searches a month, with some at KD under 30; any one keyword needs about 50 a month to be worth a page | `seo_search_keywords` | Competitors' brand names dominate category sets (81% of the volume in our test): remove them before summing. Small keywords' `trend[12]` swings tenfold month to month; read the median month, not the latest |
| AI answers | Engines answer the buyer's questions and name competitors but not the user | `aeo_run_ai_answers`, `aeo_search_prompts` | Nobody named means the category is early, not that the channel is dead. `ai_search_volume` is a People Also Ask proxy, never searches |
| Reddit | The problem comes up every week in a few communities, and threads ask for a recommendation; a thread with 10 or more comments in the last year counts | `reddit_search_subreddits`, then `reddit_get_new_posts` | Count weekly posts only from `reddit_get_new_posts` with `pages: 3` and a check of `coverage[]`; one page covered hours, not the week, in our test. Search rows are a sample |
| Short video | Videos on the category in the last month with views in the tens of thousands, made by creators rather than vendors | `tiktok_search_videos`, `youtube_search_videos` with `sort: "popular"` | Compare only within one platform and one query; the median of the top results is the bar. In our test vendors' own videos had a median of 138 views |
| LinkedIn | A B2B buyer, and posts about the problem in the last month that draw comments | `linkedin_search_posts` | `views` is null and dates are approximate; judge on comments. A founder's profile is the channel more than a company page |
| Paid | Competitors keep ads running for 90 days or more: `first_shown` 90+ days ago, `active: true` on Meta, `last_shown` within 7 days elsewhere | `ads_get_advertiser_ads` | On Google `active` is null; a domain lookup can return other advertisers too: group rows by `advertiser` |
| Outbound | A B2B buyer whose ICP a company search can express (industry, headcount, location), and a price that pays for a sales conversation | `leads_search_companies` | `rows_available` is the provider's total before the keyword check, an upper bound |

Limits the tools cannot get round: X has no search here, so only a competitor's X account can be sized ([x-twitter](../../x-twitter/SKILL.md)); Facebook groups are read only from URLs the user supplies ([facebook](../../facebook/SKILL.md)); Product Hunt, Hacker News, Slack, Discord and newsletters are not read at all. Say "not measured", never "weak".

`seo_get_serp_competitors` returns sites that share search results, not business competitors. Take competitors from the user, from the brands AI answers name, or from [competitor-intel](../../competitor-intel/SKILL.md).

## Price sets the motion

A rule of thumb from deal economics, not a study:

- **Under about $1,000 a year per customer**, a sales conversation costs more than the customer pays. The product sells itself: search, communities, content, creators, a trial.
- **Above about $10,000 a year**, outbound and demos are normal and [outbound](../../outbound/SKILL.md) is a core channel.
- **Between the two**, product-led with a light sales touch for the larger accounts.

## Demand captured or demand made

- Search and search ads capture demand that exists. A new category often shows null or sub-floor volume; that means demand has to be made first (communities, creators, founder content, press), which takes longer.
- Demand for a new category shows up first in Reddit threads and AI prompts, before Google volume. Weigh those more when Google is empty.
- Before about 100 customers, the scalable channels are slow or costly tests of an offer that is still changing. Conversations first: founder outreach, communities, the user's network.

## How many channels

- One primary and at most one secondary for a team of one; two or three for a small team. The plan's value is what it leaves out, with the reason.
- The case is effort and sample size, not a study (see Unknown in the [skill](../SKILL.md)). To tell a channel that converts at 3% from one at 2%, at the usual 95% confidence and 80% power, takes about 3,800 visitors from each. Five channels at a few hundred visitors a month each never reach a verdict ([measurement](measurement.md#is-it-real)).
- A crowded channel is not closed. Take it when demand is large and the team has an angle the incumbents lack; take the open one first when the team is small.
- Choose channels where buyers already are, not where the founder is comfortable. When the two coincide, better still.

## Effort per format

Defaults to replace with the team's own times: a LinkedIn or X text post 45 minutes, a Reddit reply 20, a carousel 2 hours, a newsletter issue 2 hours, a short video 90 minutes, an article 5 hours, a YouTube video 8 hours. Divide the team's weekly hours by these to see what a channel costs before ranking it.

## The deliverable

A ranked table: rank, channel, demand (the number and its source), best evidence (a keyword, a prompt, a thread or a video URL), competitors present (who, still active or not), gap (open, contested, crowded), effort for this team (hours a week), and the first move (the topic skill that starts it). Every row names its evidence. The tools see what people search, ask and watch, not what converts; say so when the user asks which channel will bring customers.

## Without the connector

The same reading by hand, slower and noisier: Google's autocomplete and "People also ask" for the category, a free keyword planner for volume; the category asked in a logged-out ChatGPT and Gemini window, noting who is named; Reddit search sorted by new for the past month, counting posts per week in the three communities that recur; TikTok and YouTube search for the category, noting the views of the top ten and who made them; Google's and Meta's public ad libraries for each competitor, noting how long the oldest running ad has run. Mark every number as hand-collected and say a few samples cannot separate a gap from noise.
