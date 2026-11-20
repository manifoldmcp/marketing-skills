# Field notes

What Manifold has seen in its own pulls of the `instagram_*` tools. Small samples: read them as evidence of how the data behaves and of rough ranges, not as market benchmarks. Newest first.

## Fifteen accounts, one page of reels each (15 October 2026)

Inputs: `instagram_get_profile` and one page of `instagram_get_reels` on 7 brand accounts (Gymshark, Duolingo, Glossier, Buffer, Ahrefs, Vibrant Meals, Clean Eatz Kitchen) and 8 creators (4K to 1.7M followers, food and lifestyle, found through `instagram_search_posts` on #mealprep), plus `instagram_get_posts` on two brands, four transcripts and one page of comments. 179 reel rows, 37 credits.

**Reach against followers splits brands from creators, not tiers.** Median reel views over followers, leaving out pinned reels, paid rows, other accounts' rows and reels under 3 days old:

| Group | Accounts | Median views over followers | Range | Median engagement over views |
|---|---|---|---|---|
| Brands | 7 | 6% | 3% (Glossier, Buffer) to 10% (Gymshark) | 1.8% (1.3% to 4.5%) |
| Creators | 8 | 33% | 5% to 114% | 2.8% (1.8% to 6.5%) |

- Follower count predicted almost nothing: a 152K creator's median reel had 15K views, a 4K creator's had 2K, a 1.5M creator's had 1.7M. Glossier (3.1M) and Buffer (109K) both sat at about 3%.
- The old rule of thumb "median reel views under 5% of followers means a dead following" would have flagged three healthy brands with millions of followers. It is a creator-vetting floor, not a brand benchmark. For a brand, compare with direct competitors.

**One page is not a fixed window.** Twelve reels covered 6 days (Duolingo, Nicolette Brito) to 23 months (a creator posting a reel every few months). For a daily poster, one page is mostly reels under a week old whose views are still climbing; page until the window you need is covered.

**Data traps seen in this pull:**

- **`posts_count` returned 12 and `kind` returned person on all 15 profiles,** including Gymshark, Glossier and Duolingo. The 12 is the posts a signed-out visitor is shown, so the tool now returns `posts_count` null: count posts from the listings. `kind` is company only when Instagram flags a business account, and Gymshark's is a professional account without that flag: read a brand from `industry` (the category Instagram shows, such as Food & Drink) or the bio. `email` and `phone` came back null even where the bio carried an email address; read the bio.
- **`likes` of exactly 3 was a hidden like count.** 27 of 179 rows (15%), on 4 of 15 accounts, showed 3 likes against thousands to millions of views. Every such row on Clean Eatz Kitchen's reels carried Instagram's hidden-counts flag, so the listings and `instagram_get_post` now return `likes` null for them. Hashtag search carries no flag and still shows 3: drop those rows from any engagement rate.
- **Other accounts' reels sit in the listing.** 20 of 179 rows (11%) were authored by someone else: collab posts where the account was invited as a co-author. Filter on `author`.
- **Pinned reels come first.** 4 of 15 accounts had one to three old reels at the top (one from 2022). Sort by `created_at` and drop them from cadence and medians.
- **The posts listing has no reel numbers.** In `instagram_get_posts`, video rows had `views`, `duration_s` and `is_ad` all null; take reel views from `instagram_get_reels`. Image rows have no views at all.
- **`music` is filled in hashtag search, empty in listings.** Search rows carried the audio ("Original audio - handle" or a track and artist); every listing row had none.
- **Search and listings disagree on fresh reels.** The same reel showed views as null in a search row and 1,194 in the listing; another showed 15K in search and 39K in the listing hours later. Take views from the listing.
- **Comment-for-DM posts break the comment counts.** A dietitian's "comment RECIPES" reels had comments at 40% to 100% of likes (47K comments on one), against under 2% on her other reels. The first page of `instagram_get_comments` (15 rows, newest first) was all keyword replies. Giveaways do the same.
- **Transcripts:** a 60-second talking reel came back in full; a 6-second reel with no speech came back `NoData` (free). Reels of 161 and 179 seconds came back `ProviderUnavailable` with a 30-second retry hint and no charge: the vendor's refusal of reels over two minutes mentions its paid long-transcription option in credits, which the tool misread as its own balance running out. It now answers `InvalidTarget` with the reason. Do not retry a reel over two minutes.
- A reel that a 152K-follower creator posted under #mealprep had 19 views in search and was missing from her reels tab. Possibly a Trial Reel (shown to non-followers, kept off the profile); unconfirmed.
