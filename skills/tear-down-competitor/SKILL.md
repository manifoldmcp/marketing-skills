---
name: tear-down-competitor
description: When the user wants a full picture of one competitor's marketing. Tears down one competitor across company data, Google search, the Meta, LinkedIn, TikTok and Google ad libraries, its social accounts on six platforms and AI answers, in one table with the three things to act on. Also use when the user mentions a competitor teardown, a competitor profile, competitive intel on a rival, what is X doing in marketing, a snapshot of everything X does, or how big a competitor is and where its traffic comes from. Finding who the competitors are goes to find-competitors, a sales one-pager to write-battlecard, a competitor's ads in full to research-meta-ads, research-tiktok-ads, research-linkedin-ads or research-google-ads, one platform's account in depth to audit-tiktok-account, audit-instagram-account, audit-youtube-channel, audit-facebook-page, audit-linkedin-page or audit-x-account, and a weekly watch to monitor-competitors.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Competitor teardown

One competitor, every channel, one table: how big the company is, how strong it is in search, what it pays to advertise, where it posts, and whether AI engines recommend it. Each channel gets a snapshot and a link to the skill that goes deeper; this skill does not repeat them. It ends in the table and the three things the user should act on, with the source of every number.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_get_company`, `seo_get_domain_overview` and `ads_get_advertiser_ads` (hosts often add a prefix, for example `mcp__manifold__seo_get_domain_overview`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- This skill reads several tool groups: `seo_*`, `aeo_*`, `ads_*`, `leads_*` and the platform tools (`linkedin_*`, `tiktok_*`, `instagram_*`, `youtube_*`, `facebook_*`, `twitter_*`). If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run without them, and mark their rows "not checked" rather than leaving them empty, so nobody reads a gap as a zero.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the competitor's domain and handles from the competitors and accounts sections, the user's own site, the category and the market) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Competitor**: its domain and brand name. If the user gives only a name, confirm the domain before the first paid call.
- **The user's site**: optional. With it, the search row gets a "you" column for comparison.
- **Category**: the words a buyer uses for what the competitor sells, for the AI check. Default: from the `description` and `keywords[]` of the company record in step 1.
- **Market**: `location` and `language` if not the United States and English; `country` for the ad libraries.
- **Budget**: a default run costs about 2 + 25 + 4 + 11 + 36 = 78 credits, plus 25 for the user's own site in the search row. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Company.** `leads_get_company` on the domain (1 credit): `industry`, `employees`, `founded_year`, `description`, `keywords[]` (its LinkedIn specialties) and `linkedin_url`. Then `linkedin_get_company` with that URL (1 credit): LinkedIn's own `employees`, `followers` and `bio`. The description names the category for step 5; the two headcounts say what the competitor can outspend. They often disagree (the provider's estimate against people who list the company on LinkedIn): report both. On `NoData`, take the category from the homepage (`seo_get_page`, free) and call `linkedin_get_company` with the brand as `handle`.
2. **Search.** About 25 credits.
   - `seo_get_domain_overview` (5 credits): `domain_rank`, `organic_traffic`, `organic_keywords`, `positions` and `top_pages[]`.
   - `seo_get_ranked_keywords` with `limit: 100` (10 credits). Split the rows into branded (the keyword holds the brand name) and non-branded. From the non-branded ones, name the three topics and the pages that bring the most `traffic`, and count the keywords with commercial or transactional `intent`: those are the searches where the competitor meets buyers.
   - `seo_get_backlink_summary` (10 credits): `referring_domains` and `dofollow_share`.
   - Depth: keyword gaps and content in [create-seo-plan](../create-seo-plan/SKILL.md); their links as targets in [find-backlink-targets](../find-backlink-targets/SKILL.md).
3. **Paid.** `ads_get_advertiser_ads` with `active_only: true`, one page each on `facebook` (covers Instagram placements) with the brand name, `linkedin` with the company name, `tiktok` with the advertiser name and `google` with the domain (1 credit each). Only Facebook applies `active_only`; elsewhere `active` is null, so keep the rows whose `last_shown` falls in the last 30 days. On Google, if the domain returns nothing, `ads_search_advertisers` with the brand (1 credit) gives the advertiser id per region. Read: active ads per platform, the oldest `first_shown` among the active ones (an ad running for months is one that works), the formats, and on Facebook and LinkedIn the `cta` and the landing pages in `destination_url` (TikTok and Google list rows leave both null). Leave `details` off; the creative text belongs to [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md), which do the full ad teardown.
4. **Social.** About 11 credits for six platforms.
   - Profiles, 1 credit each: `tiktok_get_profile`, `instagram_get_profile`, `youtube_get_channel`, `twitter_get_profile` and `facebook_get_profile`; LinkedIn came in step 1. Take the handles from the context file's accounts section first; otherwise try the brand name as the handle and keep a profile only when its `website` or `bio` points at the competitor's domain. Ask the user for a handle that does not match.
   - Recent posts, one page each (1 credit): `tiktok_get_videos`, `instagram_get_posts`, `youtube_get_videos`, `twitter_get_tweets`, `facebook_get_posts` and `linkedin_get_company_posts`. Read `followers`, posts in the last 30 days, and the median `views` or `likes` per post where the platform publishes them. `linkedin_get_company_posts` rows carry no engagement; cadence is all it shows here.
   - Depth, per platform: [audit-tiktok-account](../audit-tiktok-account/SKILL.md), [audit-instagram-account](../audit-instagram-account/SKILL.md), [audit-youtube-channel](../audit-youtube-channel/SKILL.md), [audit-facebook-page](../audit-facebook-page/SKILL.md), [audit-linkedin-page](../audit-linkedin-page/SKILL.md), [audit-x-account](../audit-x-account/SKILL.md).
5. **AI answers.** A light check: `aeo_run_ai_answers` with two category prompts ("best <category>", "best <category> for <the buyer>") and `brands` holding the user and every competitor the user may tear down next (up to 10), on the default engines (18 credits per prompt, 36). It returns a `task_id`; call `get_task` (free) after `poll_after_s`. Count the cells (one prompt on one engine) where each brand is mentioned, and note the domains cited most. It is never cached, so reuse this result for the next competitor rather than paying again. The full check, with a prompt set and share of voice, is [check-ai-visibility](../check-ai-visibility/SKILL.md).
6. **Deliver** a table with one row per channel (company, search, paid per platform, social per platform, AI answers) and these columns: channel, what was measured (the numbers, with the tool), what it says (one line), the user's number where there is one, and the skill for depth. Then three lines: where the competitor is strong, where it is absent or weak, and the one thing the user should act on first. Mark every channel the tools could not check "not checked".

## Judgment

- This is a snapshot. A trend needs two runs; the host keeps this table, and a scheduled rerun is [monitor-competitors](../monitor-competitors/SKILL.md).
- Separate branded traffic before comparing. A known brand gets most of its `organic_traffic` from its own name, which says nothing about the keywords the user can contest. `organic_traffic` is a modelled estimate: good for comparing sites, not a visit count.
- No ads in any library does not prove no paid spend: search ads can run under a reseller or an agency's advertiser name, and affiliates promote without ads. Say "none found", not "none".
- Impressions and spend are the ranges a library publishes, and null where it publishes none. Count ads and read how long they run; do not turn ranges into a spend figure.
- A channel with an account but no post in 90 days is inactive. Report it as absent, since that is the gap the user can take.
- AI answers are live and non-deterministic: two prompts on five engines is ten samples, enough to see presence or absence, not a share of voice.
- The tools cannot see pricing (see [compare-messaging](../compare-messaging/SKILL.md)), product quality, the sales team, email marketing, or revenue and funding: the company record carries neither. No manifold tool reads a page's body text; never fill a price, a feature or a customer count from memory.
- Every claim carries its source: the tool and field, or the URL. Mark an inference as an inference. Quote the competitor's copy only as short evidence (a headline, a tagline), with the URL.
- Credits: say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. A result this account already paid for is free while cached: 7 days for most SEO data, 24 hours for ad lists and profiles, 30 days for company records.
- Never contact anyone. After delivering, offer to write the findings into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Related skills

- Who the competitors are in the first place: [find-competitors](../find-competitors/SKILL.md).
- The competitor's ads in full, with angles, offers and creative over time: [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md). For every platform in full, run all four in turn and merge the tables by angle.
- The same competitor watched every week: [monitor-competitors](../monitor-competitors/SKILL.md).
- A one-page card for sales against this competitor: [write-battlecard](../write-battlecard/SKILL.md).
- What people complain about in this competitor: [find-competitor-complaints](../find-competitor-complaints/SKILL.md).
- The same research for an agency's prospect: [prepare-client-pitch](../prepare-client-pitch/SKILL.md).
