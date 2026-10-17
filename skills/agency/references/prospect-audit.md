# Prospect audit

A prospect audit opens a sales conversation: a handful of cheap calls that find three findings the prospect can check for themselves, each with a competitor beside the number. The full audit and the plan are what the prospect pays for after signing. Checked October 2026.

## What lands and what backfires

- **Pick prospects the agency has a right to win.** Agencies credit industry or niche expertise (59%) and case studies (46%) far above price (31%) for the pitches they win; 47% win half or fewer of their pitches (AgencyAnalytics, 494 agency staff, February to April 2026, self-reported). An audit for a prospect outside the agency's niche wastes the effort.
- **Challenge, do not flatter or undercut.** In selection, 93% of clients want an agency that challenges them, 87% weigh chemistry, and only 20% weigh cost; agencies believe cost matters to 95% and chemistry to 5% (Setup with the CMO Institute, about 100 respondents, published January 2026). Three sharp, true findings with a competitor's number beside them are the challenge.
- **Lead with what the prospect can verify in a minute.** Give the exact prompt to ask ChatGPT, the keyword to search, the page to open. A finding they confirm themselves does more than any chart.
- **Never quote estimated traffic as theirs.** They know their real number. A third-party estimate can be off by orders of magnitude on a small site (see [field notes](field-notes.md)); quoting it ends the conversation. Compare domains with each other from the same source, labelled "estimated", or use rankings instead.
- **Be fair.** A prospect that already does well hears so, with the one gap that remains. Invented problems are found out in the first call.
- **Expect ghost pitches.** 45% of UK agencies had pitches recalled after the budget was pulled (JFDI and Opinium, 250 agencies, February 2025). Warning signs: no access to the decision maker, no budget in the brief, no structured process. Keep the free audit cheap and small for that reason.
- **Referrals still bring most clients.** 85% of agencies name referrals as a lead source, 33% outbound and 11% AI search (AgencyAnalytics, 2026); clients find agencies most happily through word of mouth (82% satisfied, Setup). An audit sent cold is a door opener with low odds; one sent to a warm referral converts better. Finding the person to send it to is [outbound](../../outbound/SKILL.md)'s job.

## Inputs

- **Prospect**: the domain, and what the business sells and where (a local dentist, a national store, a SaaS).
- **Service**: what the agency sells. It decides which finding leads, and a finding outside it (no ads, when the agency does not run ads) goes to the bottom or out.
- **Competitors**: one or two real businesses. Default: the top two from `seo_get_serp_competitors` with `limit: 10` (6 credits), skipping publishers, marketplaces and directories, which are SERP competitors, not business ones.
- **Buyer questions**: two questions a buyer would ask an AI engine ("best invoicing app for freelancers", "emergency plumber in Leeds").
- **Market**: `location` and `language` if not the United States and English.

## The calls (about 134 credits, under $1.50)

1. **Size.** `seo_get_domain_overview` on the prospect and each competitor (15 credits each): `domain_rank`, `organic_keywords`, `positions`, `top_pages`, and `organic_traffic` only as an estimate. If the prospect's `top_10` is 0, skip the traffic comparison: the figure is modelled from deep rankings.
2. **Quick wins.** `seo_get_ranked_keywords` on the prospect. Its rows come by traffic, highest first, so striking-distance keywords with little traffic sit deep: check `meta.rows_available` and pull every row when it is a few hundred (5 credits plus 5 per 100), then keep keywords ranking 4 to 20 with real volume. Then `seo_get_page` on the homepage and top page (free): a missing or overlong title, no meta description, no `h1`, no structured data are findings anyone can see.
3. **Gap.** `seo_get_keyword_gap` against the strongest competitor (10 credits). "competitor.com ranks for these 20 searches, worth N a month, and you rank for none."
4. **Site health.** `seo_run_technical_crawl` with `max_pages: 100` (5 credits), then `get_task` after `poll_after_s`: `broken_links`, `non_indexable`, `duplicate_titles`, `onpage_score`. Add `aeo_get_site_readiness` (free): a CDN or robots rule blocking AI crawlers is a finding few prospects know.
5. **AI answers.** `aeo_run_ai_answers` with the two questions and `brands` set to the prospect and the competitors (54 credits on five engines), then `get_task`. Count in how many of the ten answers each brand is named and cited. Say it is one run on one date and a second can differ.
6. **Ads.** `ads_get_advertiser_ads` with `platform: "facebook"` and `platform: "google"` for the prospect and the strongest competitor (1 credit each): whether each runs ads, how many still run (`active: true` on Meta; on Google `active` is null, so `last_shown` within 7 days), and the oldest `first_shown` still running. An ad kept for months is usually one that works.

A local business often has null or tiny estimates. Use `seo_get_position` (6 credits each) on two or three "service in city" keywords instead, and show who ranks above from `above[]`.

## Deliver

Two tables and three lines. A scoreboard: domain, domain rank, keywords in the top 10, estimated organic traffic (labelled), AI mentions out of ten, running ads. A findings table: area, finding, the number, the evidence (URL, keyword or prompt), and what fixing it involves. Then the three findings to open with, one sentence each, each with a number and a competitor. Every number names its tool and date. Nothing is sent from here.

## Without the connector

Search the prospect's main keywords in a private window and note who ranks above them; ask ChatGPT, Gemini and Perplexity the two buyer questions logged out; open the homepage source for title, description and `h1`; check the Meta Ad Library and Google Ads Transparency Center by hand. Say that hand checks are a snapshot, and give no traffic figure at all.
