# Fans and affiliates

Two lists of people who already move buyers: creators posting about the brand without being paid, and publishers and creators earning from competitors. Both are cheaper and more believable than a cold creator. New brand mentions every week belong to [brand-monitoring](../../brand-monitoring/SKILL.md); this is the one-off list to work with.

## Brand fans

**Search the brand.** `tiktok_search_videos` with each brand term, `instagram_search_posts` with the brand and main product hashtags, `youtube_search_videos` with the brand and "<brand> review", all `since: "year"`, two pages each. Include product names and misspellings. A brand name that is also a common word ("Notion", "Ring") needs a product word beside it. Instagram finds only fans who tag the brand; a thin result for a young brand is normal, not a verdict.

**Keep the organic fans.** Drop the brand's own accounts, creators the user already pays or gifts, resellers and repost pages, and any row with a sponsored signal (both `is_ad` and the caption; see [vetting](vetting.md#sponsored-share)): those are already paid. Read each caption's stance: praise, a how-to, a comparison the brand wins. A complaint goes on a separate list for the user's support team, and one with reach needs an answer this week.

**Size and check** the top 20 to 30: profile, one page of recent posts, comments on their best brand post. No size floor: a 2,000-follower customer who posts unprompted is worth a gift. More than one brand post marks a real fan. Drop fans inactive for 30 days or with more than a third of posts sponsored.

**Pick the deal per fan:**

- **Gift or early access:** nano creators and anyone who posted more than once unpaid.
- **Affiliate code:** fans whose brand posts drew buying questions ("where do I get it", "link?").
- **Paid post:** fans whose brand posts reached or beat their own median.
- **Run their post as an ad:** a brand post that beat their median. It needs their permission, a Spark Ads code or partnership-ads access, and usually a fee ([rates](rates.md#running-posts-from-the-creators-handle)).

Their post is theirs: reposting needs consent and credit. Once the brand gives anything, a gift included, every later post about it must be disclosed ([disclosure](disclosure.md)). About 74 credits for a default run. Deliver: creator, platform and link, followers, median views, brand posts (count, best URL, views against their median), stance, buying questions, already takes deals, suggested deal, contact; the complaints list below it.

## Affiliate partners

The best affiliate recruits already earn from a competitor: they have the audience, the content and the habit.

- **Proof from links.** `seo_get_backlinks` with `limit: 1000` on each competitor domain (25 credits each). Keep rows whose `url_to` carries an affiliate parameter: `ref=`, `via=`, `aff=`, `affiliate`, `fpr=`, `partner=`, `irclickid`, `utm_medium=affiliate`. Each is proof of a deal. Links routed through an affiliate network's own domain do not show; the next step finds those sites.
- **Likely from rankings.** `seo_search_keywords` with `seed: "<competitor> review"` for which review searches have volume, then `seo_get_serp` for "<competitor> review", "<competitor> alternatives" and "best <category>". A publisher ranking for a competitor's review usually earns from it. Say which rows are proven (a parameter) and which likely (a ranking).
- **Video reviewers.** `youtube_search_videos` with "<competitor> review", `since: "year"`, then `youtube_get_video` on the strongest: the description's `links` show the creator's affiliate links (amzn.to, geni.us, tidd.ly, a brand's referral URL; all four reviews in our run had them). A creator already using affiliate links will compare the user's terms with what they earn now.
- **Size by reach.** `seo_get_traffic_estimates` (50 credits plus 50 per 100 domains) and `seo_get_domain_ratings`; rank by `organic_traffic`, and drop sites under about 1,000 visits a month unless they cover the niche exactly.
- **Contact** the partnerships person (marketing, the owner or business development) through the contact steps in [link-building](../../link-building/SKILL.md) with `department: ["marketing", "executive", "sales"]`.
- **Before outreach,** the user must have commission terms; never promise terms they have not set. A publisher promoting three competitors will compare. Skip coupon and cashback sites unless the user wants them. One row per publisher with its strongest evidence page. About 314 credits for 20 partners.
