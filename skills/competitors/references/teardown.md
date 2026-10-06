# Competitor teardown

One competitor, every channel, one table: how big the company is, how strong it is in search, what it pays to advertise, where it posts, and whether AI engines recommend it. Each channel gets a snapshot and a link to the playbook that goes deeper; this playbook does not repeat them. It ends in the table and the three things the user should act on.

## Inputs to settle first

- **Competitor**: its domain and brand name. If the user gives only a name, confirm the domain before the first paid call.
- **The user's site**: optional. With it, the search row gets a "you" column for comparison.
- **Category**: the words a buyer uses for what the competitor sells, for the AI check. Default: from the `description` and `keywords[]` of the company record in step 1.
- **Market**: `location` and `language` if not the United States and English; `country` for the ad libraries.
- **Budget**: a default run costs about 2 + 25 + 4 + 11 + 36 = 78 credits, plus 25 for the user's own site in the search row. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Company.** `leads_get_company` on the domain (1 credit, 1 on `NoData`): `industry`, `employees`, `founded_year`, `description`, `keywords[]` and `linkedin_url`. Then `linkedin_get_company` with that URL (1 credit): LinkedIn's own `employees`, `followers` and `bio`. The description names the category for step 5. The record holds no funding; a round the competitor announced shows in its LinkedIn posts in step 4, and says what it can outspend.
2. **Search.** About 25 credits.
   - `seo_get_domain_overview` (5 credits): `domain_rank`, `organic_traffic`, `organic_keywords`, `positions` and `top_pages[]`.
   - `seo_get_ranked_keywords` with `limit: 100` (10 credits). Split the rows into branded (the keyword holds the brand name) and non-branded. From the non-branded ones, name the three topics and the pages that bring the most `traffic`, and count the keywords with commercial or transactional `intent`: those are the searches where the competitor meets buyers.
   - `seo_get_backlink_summary` (10 credits): `referring_domains` and `dofollow_share`.
   - Depth: keyword gaps and content in [seo](../../seo/SKILL.md); their links as targets in [link-building](../../link-building/references/backlink-targets.md).
3. **Paid.** `ads_get_advertiser_ads` with `active_only: true`, one page each on `facebook` (covers Instagram placements) with the brand name, `linkedin` with the company name, `tiktok` with the advertiser name and `google` with the domain (1 credit each). Only Facebook applies `active_only`; on the other libraries keep the rows whose `active` is true or whose `last_shown` is recent. On Google, if the domain returns nothing, `ads_search_advertisers` with the brand (1 credit) gives the advertiser id per region. Read: active ads per platform, the oldest `first_shown` among the active ones (an ad running for months is one that works), the formats, the `cta`, and the landing pages in `destination_url`. Leave `details` off; the creative text belongs to [paid-ads competitor ads](../../paid-ads/references/competitor-ads.md), which does the full ad teardown.
4. **Social.** About 11 credits for six platforms.
   - Profiles, 1 credit each: `tiktok_get_profile`, `instagram_get_profile`, `youtube_get_channel`, `twitter_get_profile` and `facebook_get_profile`; LinkedIn came in step 1. Try the brand name as the handle and keep a profile only when its `website` or `bio` points at the competitor's domain. Ask the user for a handle that does not match.
   - Recent posts, one page each (1 credit): `tiktok_get_videos`, `instagram_get_posts`, `youtube_get_videos`, `twitter_get_tweets`, `facebook_get_posts` and `linkedin_get_company_posts`. Read `followers`, posts in the last 30 days, and the median `views` or `likes` per post where the platform publishes them. `linkedin_get_company_posts` rows carry no engagement; cadence is all it shows here.
   - Depth: [TikTok](../../tiktok/references/competitor-accounts.md), [Instagram](../../instagram/references/competitor-accounts.md), [YouTube](../../youtube/references/competitor-channels.md), [LinkedIn](../../linkedin/references/company-page-audit.md) and [Facebook](../../facebook/references/page-audit.md).
5. **AI answers.** A light check: `aeo_run_ai_answers` with two category prompts ("best <category>", "best <category> for <the buyer>") and `brands` holding the competitor and the user, on the default engines (18 credits per prompt, 36). It returns a `task_id`; call `get_task` (free) after `poll_after_s`. Count the cells (one prompt on one engine) where each brand is mentioned, and note the domains cited most. The full check, with a prompt set and share of voice, is [ai-search visibility check](../../ai-search/references/visibility-check.md).
6. **Deliver** a table with one row per channel (company, search, paid per platform, social per platform, AI answers) and these columns: channel, what was measured (the numbers, with the tool), what it says (one line), the user's number where there is one, and the playbook for depth. Then three lines: where the competitor is strong, where it is absent or weak, and the one thing the user should act on first. Mark every channel the tools could not check "not checked".

## Judgment

- This is a snapshot. A trend needs two runs; the host keeps this table, and a scheduled rerun is [monitoring competitor watch](../../monitoring/references/competitor-watch.md).
- Separate branded traffic before comparing. A known brand gets most of its `organic_traffic` from its own name, which says nothing about the keywords the user can contest.
- No ads in any library does not prove no paid spend: search ads can run under a reseller or an agency's advertiser name, and affiliates promote without ads. Say "none found", not "none".
- Impressions and spend are the ranges a library publishes, and null where it publishes none. Count ads and read how long they run; do not turn ranges into a spend figure.
- A channel with an account but no post in 90 days is inactive. Report it as absent, since that is the gap the user can take.
- AI answers are live and non-deterministic: two prompts on five engines is ten samples, enough to see presence or absence, not a share of voice.
- The tools cannot see pricing (see [messaging and pricing](messaging-pricing.md)), product quality, the sales team, email marketing, or real revenue; `revenue` and the funding fields in the company record are always empty.
