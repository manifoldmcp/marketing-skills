# Finding creators

How to build a shortlist of creators who already post on the niche, on one platform or across TikTok, Instagram and YouTube, and what each platform's search will and will not show. Vetting the finalists is in [vetting](vetting.md); UGC makers in [UGC](ugc.md); people already posting about the brand in [fans and affiliates](fans-and-affiliates.md).

## Settle first

- **Niche:** three to five search terms or hashtags: the category, the problem the product solves, a use case, and a competitor's name (creators who reviewed a rival already cover the category). Read the next terms off the captions of the first results.
- **Size band:** default 10K to 250K followers (on YouTube, a median of 2,000 to 100,000 views a video). Engagement on sponsored posts peaks between small and large accounts, and ROI favours the smaller end ([rates](rates.md#who-to-pay)). Nano is under 10K, micro 10K to 100K, mid 100K to 500K, macro 500K to 1M, mega above.
- **Market:** search has no country filter on any platform. Keep creators whose captions and speech are in the buyers' language; confirm country only for finalists.
- **Count:** 20 people by default, 15 on YouTube.

## What each platform's search returns

- **TikTok:** `tiktok_search_videos` is free text and ranked, 30 rows a page. Rows carry `author_followers`, so the first cut needs no profile calls. Use relevance for niche fit, `since: "month"` for active creators (`since: "year"` reaches back about six months). Add a term rather than a fourth page: later pages are weaker matches. Drop brand accounts, resellers and TikTok Shop clip accounts (many short product clips at a few hundred views, shop-sale copy in every caption).
- **Instagram:** `instagram_search_posts` takes one hashtag, no free text, no sort, 10 rows a page, and its rows carry no follower count, so every author needs `instagram_get_profile`. Many good creators do not tag, so Instagram is under-found; widen with more tags or start from competitors' partnership posts. Expect brand accounts in the results (3 of 10 in our run). The profile's `kind` does not settle person or brand: many creators run business accounts.
- **YouTube:** `youtube_search_videos` returns regular videos, not Shorts, with views and length but no follower count, likes or `is_ad`. Dates are approximate. Keep channels with two or more relevant videos in the results, not one that hit the topic once. Small channels can reach far beyond their subscribers through search (a 2,180-subscriber channel with a 52K-view review in our run), so judge by the video's views, then the channel's median.

All searches are ranked samples, never complete, and cached 6 hours (repeats are cheap). A creator the user expected and did not find is a reason for a second pass with narrower terms, not proof of absence. Say "in the top results for X", never "on TikTok".

## From results to a shortlist

1. **Group rows by `author`** and count matching posts per author. Niche fit beats one viral hit.
2. **Size** the top 30 to 40 with `tiktok_get_profile`, `instagram_get_profile` or `youtube_get_channel` (1 credit each). Read `bio` and `website` for a business email, "UGC", "brand collabs" or a management agency.
3. **One page of recent posts** for the best 20 to 25 (`tiktok_get_videos`, `instagram_get_reels` or `instagram_get_posts` for image accounts, `youtube_get_videos`; 1 credit). Work out the median, view rate, cadence, niche share and sponsored share as [vetting](vetting.md#reading-the-numbers) sets out, and apply its floors.
4. **Rank** by niche fit first (share of recent posts on the topic), then median views, then engagement. Never by followers.

## Across platforms

- **Match people, not handles.** For the top 20, look for the same person on the other platforms with the same handle (1 credit a profile), and count a match only with a second signal: the same `website`, a bio naming the other handle, or the same name in the same niche. Handles are claimed by strangers.
- **Put engagement on one scale.** Raw rates are not comparable across platforms (each has its own base and floor). Express each creator's view rate and engagement as a multiple of the median of the candidates on that platform: 2.0 on TikTok and 0.6 on YouTube means strong on TikTok. With fewer than ten candidates on a platform the median is noisy; show the raw rates too.
- **Pay where the views are.** Most creators are strong on one platform and carry an old following on the others.

## Contacts

- **The bio first.** Creators who take deals print a business email or a manager there. The profile `email` field is often null even when the bio shows an address (TikTok and Instagram in our run); YouTube's `email` is filled when the channel's bio gives one.
- **Then the `website`.** It is usually a link-in-bio page (Linktree, Beacons) that the host can open; when it is the creator's own domain, run the contact steps in [link-building](../../link-building/SKILL.md) with `department: ["executive", "marketing"]`, since the creator owns the site, and verify the one address the user will write to.
- Otherwise the contact is a platform DM, which the user sends. Never guess an address.

## Cost and delivery

A TikTok run of five terms is about 80 credits (searches, 40 profiles, 25 listings); Instagram about 75; YouTube about 102 (it needs `youtube_get_video` for likes); cross-platform matching about 40 more: about 300 for all three. Drop a platform the buyers do not use before cutting depth.

Deliver one row per person: handle and link per platform, followers, median views, view rate and engagement (or the multiple across platforms), posts a week, last post, niche share with one example link, sponsored posts seen, brands worked with, contact and its source, and one line on fit. Mark rows checked on one platform only. A list is not a vetting: before any fee, use [vetting](vetting.md).

## Competitors' creators

The creators a competitor pays are proven for the niche. Search each competitor's name on all three platforms and keep the posts with a sponsored signal; `ads_get_advertiser_ads` on Meta (`platform: "facebook"`, `active_only: true`) and TikTok shows creator videos the competitor paid to run as partnership ads or Spark Ads. On Meta read `active`; on TikTok read `last_shown`. A competitor that pays the same creator twice, or keeps a creator's video running as an ad a month after `first_shown`, has found a creator who sells. Flag these creators for possible exclusivity rather than excluding them.
