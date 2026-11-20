# Measuring growth

What is working, how much of it the analytics can see, and when a change is real. Read it before setting KPIs, judging a channel at day 30, or answering "where do our customers come from". Checked October 2026.

## The attribution gap

Most of what moves a buyer happens where no tracker reaches.

- **B2B buyers choose before contact.** 94% of about 4,000 B2B buyers ranked a shortlist before engaging a seller; first contact came 61% of the way through the journey (69% in 2023-2024); the favourite at that point won 77% of the time; buyers evaluated 5.1 vendors and started with 3.6 (6sense Buyer Experience Report, 2025, self-reported survey of buyers with $25,000+ in purchases). 94% used LLMs during the purchase, mostly to compare vendors.
- **The journey is long and mostly before the pipeline.** In Dreamdata's 2025 data (66M+ sessions, 3.5M+ journeys from its customers; attribution model not stated), 81% of the journey happened outside the sales pipeline, a deal took 88 touches across 4 channels and 10 stakeholders, and the first LinkedIn ad impression came a mean 281 days before revenue. Organic LinkedIn company-page impressions appeared in 17.9% of closed deals, about 90 days before revenue.
- **AI answers influence without clicking.** AI assistants sent 0.24% of sessions across 54 sites (Amsive, 2025) while buyers use them throughout the purchase. Many AI visits also arrive with no referrer from apps and land in "direct"; Ahrefs suspected ChatGPT was not passing referral data before mid-2025.

So last-click reports undercount every channel that works early (communities, founder content, podcasts, AI answers, PR) and overcount the ones at the end (branded search, direct, retargeting). Say this whenever a user is about to cut a channel on last-click numbers alone.

## What to set up on day one

- **A required free-text "How did you hear about us?" field** at signup or demo request. It is the one cheap way to see podcasts, communities, AI answers and word of mouth. No study has validated it against ground truth; read it beside the software numbers, never instead, and code the answers into the plan's channels by hand.
- **One UTM link per venue, creator and post type**, so a launch or a creator can be counted.
- **AI referrals as their own segment:** `analytics_get_report` (free) with `dimensions` including `sessionSource` and a filter whose `operator: "regex"` matches chatgpt.com, perplexity, gemini.google.com, copilot and claude.ai. GA4 does not group them for you by default.
- **The starting number of the deciding KPI**, with its date, before any work begins.

## Proxies the tools re-measure

- **Branded search** is the cleanest awareness proxy: `console_get_search_analytics` (free) with a `query` filter containing the brand, daily; or `seo_get_keyword_metrics` on the brand, monthly through `trend[12]`.
- **AI mention rate** on a frozen prompt set: [ai-search](../../ai-search/SKILL.md) has the method and its error bars.
- **Organic clicks and ranks:** `console_get_search_analytics`, `seo_get_position`.
- **Community volume:** weekly posts about the problem in the same subreddits with `reddit_get_new_posts` (pages enough to cover the window).
- **Referring domains** after coverage: `seo_get_referring_domains`, reading `first_seen`.

They are leading signals. Signups, pipeline and revenue come from the user's analytics and CRM.

## Is it real

Small numbers move by chance. Before calling a channel won or lost:

- **Weekly counts.** A count of n events a week varies by about ±2√n by chance. At 25 signups a week, a move to 35 is within noise; two weeks compared differ by chance by up to about 14 at that level. Read three-week averages.
- **Conversion rates between channels** need samples per channel of about (95% confidence, 80% power):

| Rates to tell apart | Visitors per channel |
|---|---|
| 10% against 15% | about 700 |
| 2% against 4% | about 1,100 |
| 2% against 3% | about 3,800 |
| 5% against 6% | about 8,100 |

- **Spread thin, nothing is readable.** Five channels at a few hundred visitors a month each cannot be told apart in 90 days; two can. That arithmetic, not a study, is the strongest case for few channels.
- **Re-measure with the same calls, the same competitors and the same queries,** or the comparison measures the change of method.
- **Mixed results across sites are normal.** In Amsive's 54 sites, LLM traffic converted above the site's average on 56% and below on 41%: a portfolio mean says little about one site. Measure the user's own.
