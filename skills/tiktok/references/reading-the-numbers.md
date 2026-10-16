# Reading TikTok numbers

How to read what the TikTok tools return without being fooled by paid views, one lucky video, pinned posts or a benchmark built on a different formula. Every audit, trend read and plan in this skill rests on these rules.

## Engagement rate

- **Manifold's default:** likes plus comments over views, from the row a search or listing returns. Shares over views and saves over views are separate signals worth their own column: shares above the account's median mean people sent it on (useful or relatable), saves mean they meant to come back (recipes, tutorials, lists). Where `views` is null, use followers from `tiktok_get_profile` as the base and say so.
- **Benchmarks use other formulas, so say which one you compare with.**

| Source | Formula | Sample | Rate |
|---|---|---|---|
| Socialinsider, 2026 report (2025 data) | likes, comments, shares and saves over views | 2M videos, 214,507 profiles | 4.40% at 1K to 5K followers, 4.00% at 5K to 10K, 3.90% at 10K to 50K, 3.75% at 50K to 100K, 3.95% at 100K to 1M |
| SociaVault, April 2026 (vendor) | likes, comments and shares over followers, last 30 posts | 150,000 accounts | median 7.84% at 1K to 10K, 5.21% at 10K to 50K, 3.89% at 50K to 100K, 2.73% at 100K to 500K, 1.84% above 500K; 4.25% overall |

- **What the two tables mean together.** By views, engagement barely changes with size: a video that reaches people gets the same response whoever posts it. By followers it falls steeply, because big accounts' followers mostly do not see each video. A creator or rival with a high rate by followers may just have few followers and one hit.
- **Manifold's rate runs higher on top videos and lower on paid ones.** Organic top-30 videos had a median of 8.9% (meal prep) and 6.5% (budget apps) by likes plus comments over views; some paid rows were under 0.5% ([field notes](field-notes.md)). A niche's top results are survivors; compare an account with its own median, not with a niche's best.
- Quarterly figures move: Socialinsider reported the rate by views dipping to 3.85% in the first half of 2026, but its pages disagree on periods. Quote the annual table, dated.

## Baseline and outliers

- **Compare a video with its own account,** never with TikTok at large: the account's median views and median engagement rate over at least 20 recent videos (`tiktok_get_videos` with `sort: "latest"`, two pages of 10). Use the median: one viral video drags a mean up for months.
- **Leave out** videos under 7 days old (still climbing), `is_ad` rows (see below) and nulls. Pinned videos come first in a latest listing whatever their age (HubSpot's first three rows were from June, April and September), so order by `created_at` before cutting a window.
- **An outlier** has views at least 3 times the account's median. TikTok views swing widely, so less is ordinary spread. Where the median is under about 1,000 views (a rule of thumb), an outlier must also be in the top tenth of its window: 3 times a tiny median is noise.
- **Repeatable beats lucky.** Six outliers in 90 days mean a format that works; one means luck. Report the count of outliers, not just the biggest video.
- **Weak medians.** Call a median from fewer than 10 videos weak and say so. Inside a niche sample, one page (10 videos) of an author is enough to set their median for an outlier test.

## Paid posts

- `is_ad: true` marks ads, boosted posts and paid partnerships. Rows whose caption said #ad or carried a brand's "partner" hashtag were marked in our samples, but a German caption opening "Anzeige" (ad) was not: read the caption too, in the market's language. A brand that boosts most of its posts has few organic rows left: 6 of HubSpot's 10 latest videos were marked.
- **Keep them out of every organic median and report them in their own column.** Their views may be bought: Spark Ads run a creator's post as an ad, so the row shows paid views. A like rate far below the account's organic median (under about 1% of views where organic sits at 5% or more) is the tell.
- An organic video that reappears in the brand's ads is a winner the brand put money behind, the strongest sign it sells. The ad side lives in [paid-ads](../../paid-ads/SKILL.md).

## Floors

Rules of thumb, not platform facts.

- **Active:** no video in the last 30 days means inactive. Flag it in competitor lists.
- **Engagement:** a median under about 3% of views (likes plus comments) means the audience watches but does not respond.
- **Views against followers:** median views under about 5% of followers means most followers no longer watch: bought, lapsed, or grown on an old format. Creator vetting goes deeper in [creators](../../creators/SKILL.md).
- **Enough videos:** judge an account on 10 or more videos.

## Field traps

- **Search is a ranked sample.** Say "in the top results for X", never "on TikTok". `sort: "popular"` in search ranks strictly by likes (checked on 30 rows), which favours videos with more days to collect them; rank by views per day since `created_at`. In listings `sort: "popular"` ranks by views and reaches back years: weight the last 90 days.
- **`since: "year"` reaches back about six months** (the oldest of 30 popular rows was dated 20 April 2026 on 15 October). There is no country filter: read `language` (null on some rows) and the caption.
- **Null is not zero.** A number TikTok did not publish is null; leave it out. `author_verified` came back null on every row we read, so do not use it as a signal. On comments, `is_creator` was null too: spot the creator's replies by `author` matching the video's author.
- **Empty fields on rows.** `transcript` and `top_comments[]` were empty on every search and listing row we read; call `tiktok_get_transcript` and `tiktok_get_comments`.
- **Page sizes differ:** 30 rows a search page, 10 a listing page, about 20 comments a page. Plan credits on those.
- **Wrong handle:** a profile that comes back `NoData` means the handle is wrong; fix it before paying for listings. An empty listing on a live account means it has posted nothing.
- **Time zones:** `created_at` is UTC.
- **What is missing entirely:** watch time, completion, traffic sources (For You, search, following, profile), profile visits, link clicks, follower growth over time, Shop sales. The user reads their own in TikTok Studio. For growth over time the host must re-run the same read and keep the table; the server keeps no history.

## Without the connector

The method works by hand. Open the account in a browser, note views, likes, comments and shares for its last 20 videos (skipping pinned and those under 7 days old), take the medians, and mark videos at 3 times the median views. For a niche, search the term in the app and log the top 20 results with their dates. Say plainly that a hand sample is small and that ads and paid partnerships are hard to spot without the "Sponsored" or "Paid partnership" label. Never fill a gap with an estimate.
