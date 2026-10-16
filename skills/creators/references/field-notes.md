# Field notes

What Manifold has seen in its own creator data. Small samples: read them as evidence of how the tools and the numbers behave, not as market benchmarks. Newest first.

## One niche, three platforms (15 October 2026)

Home espresso, United States, English. One page of `tiktok_search_videos` for "home espresso setup" (30 rows, last month), one page of `tiktok_get_videos` on seven creators with 39K to 237K followers (70 rows), `tiktok_get_profile` on one, `tiktok_get_followers` on two of them, `tiktok_get_audience` on one, one page of `instagram_search_posts` for #homebarista, `instagram_get_reels` and `instagram_get_profile` on two creators, one page of `youtube_search_videos` for "espresso machine review" (past year), `youtube_get_video` on four reviews and `youtube_get_channel` on two. 48 credits.

**Followers did not predict views.**

| Creator | Followers | Organic median views (recent page) | View rate | Note |
|---|---|---|---|---|
| A (TikTok) | 39K | about 11,500 | about 29% | Two recipe videos at 395K and 2.2M lift the page; the rest sit at 3K to 17K |
| B (TikTok) | 74K | about 142,000 | above 100% | Views come from the feed, not the following |
| C (TikTok) | 126K | about 7,000 | about 6% | |
| D (TikTok) | 237K | about 14,000 | about 6% | |
| E (TikTok) | 57K | about 3,400 | about 6% | Lifestyle; one espresso post in ten, fails niche fit |
| F (TikTok) | 81K | about 1,300 | under 2% | A pinned 2024 video at 1.4M views |
| G (TikTok) | 144K | about 400 | 0.3% | A pinned 2021 video at 30M views; recent posts are TikTok Shop clips at 160 to 750 views |
| H (Instagram reels) | 140K | about 24,700 | about 18% | |

Two of seven TikTok accounts were under the 5% floor, and both had an old viral video pinned first in the listing. A buyer pricing G on followers would pay for 144K and reach a few hundred.

**Brand posts against the creator's own median.** Five of six fell well short: a partner-tagged subscription post at 0.10x, a campaign-hashtag machine post at 0.74x, a brand-written frother post at 0.03x, two flagged brand posts at about 0.25x, an Instagram partnership reel at 0.68x. The sixth, a TikTok carrying #ad for a new machine, drew 226K views against a median near 7K (about 30x), most likely boosted with paid media; the same machine appeared undisclosed from another creator the same week at 37K.

**`is_ad` is not a deal record.** In the 70 listing rows, 10 had `is_ad: true` and 10 had a caption signal (#ad, a partner tag such as #tradepartner, a brand campaign hashtag, "shop through the link in my bio", TikTok Shop sale copy). They agreed on 2 of the 18 commercial-looking posts. Flagged posts included three trend videos naming no brand (possibly the creator promoting their own post). Unflagged commercial posts included "gifted by Philips", #cosoripartner and Amazon storefront posts. One flagged search row was a creator hoping a brand would notice her. On Instagram the same partnership reel was `is_ad: true` in the account's reel listing and false in the hashtag search row.

**The audience split is a small, recent sample.** `tiktok_get_audience` on creator A returned counts summing to 117 followers (39K in the account): United States 38.5%, Philippines 9.4%, Malaysia and the UAE 5.1% each, and 27 more countries. The newest page of `tiktok_get_followers` (50 accounts) was 40% US and held the same small countries (the Maldives, Belize, Puerto Rico), so the split appears to tally the most recent followers. At n = 117 the US share carries about ±9 points.

**Follower samples look hollow on healthy accounts.** Creator A's newest 50 followers included five with zero followers of their own and many with no bio, about as hollow as G's (whose view rate is 0.3%). G's newest followers were 65% US, more than A's. The follower sample did not separate the dead account from the healthy one; the view rate did.

**Field behaviour worth knowing:**
- `collaborators` was empty on all 100 TikTok rows; on Instagram it was filled on collab posts (a co-authored reel, a speaker brand).
- `instagram_get_profile` returned `posts_count` 12 for two verified accounts with long histories, the posts a signed-out visitor is shown; the tool now returns null there.
- `email` was null on TikTok and Instagram profiles whose bio printed an address; YouTube's `email` was filled for a channel whose bio said "business enquiries". Read the bio. `website` was a link-in-bio page (Linktree, Beacons) on three of four creators.
- `instagram_search_posts` rows carried no `author_followers`, and 3 of 10 rows were brand accounts. `youtube_search_videos` rows carried no follower counts and no `is_ad`.
- `youtube_get_video` exposed affiliate links in 4 of 4 reviews (amzn.to, an Amazon storefront, geni.us, tidd.ly) and `is_ad: true` on 2 of 4. One channel with 2,180 subscribers had a 52K-view review: on YouTube, search-driven reviews reach far beyond the subscriber count.
- One YouTube video reported exactly 1,000 comments: the count may be rounded from YouTube's display.
