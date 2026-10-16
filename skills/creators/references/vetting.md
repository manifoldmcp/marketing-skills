# Vetting a creator

Before money moves: is the audience real, still watching, responding in its own words, not tired of ads, and in the countries the user sells to. Every number is judged against the creator's own baseline, never a platform-wide benchmark. Checked October 2026.

## Depth follows the deal

- **Product-only gifting to a nano or micro creator:** one page of recent posts and a look at the comments. A full check costs more than the risk.
- **A flat fee:** everything below, about 41 credits for a creator on TikTok and Instagram, 11 more for YouTube.
- **One to five TikTok creators where only the audience country is in question:** the [audience check](#audience-country) alone, about 32 credits each.

## Reading the numbers

- **Median, never mean,** over at least 10 recent posts. One viral post drags a mean for months. Call a median from fewer than 10 posts weak.
- **Leave out** posts under 7 days old (14 on YouTube), sponsored and `is_ad` rows, and pinned posts. TikTok's latest listing puts pinned videos first whatever their age: order by `created_at`. Two of seven creators in our sample had an old viral hit pinned at the top ([field notes](field-notes.md)).
- **Engagement rate** is likes plus comments over views: TikTok videos, Instagram reels, YouTube (likes and comments only come with `youtube_get_video`, so read three to five recent videos). Instagram image and carousel posts have null views: use followers as the base and report them apart from reels. Instagram's `shares` is always null here.
- **View rate** is the median views over followers. It is the single best check on a following: bought, inactive or grown on an old format all show up as a low view rate.
- **Null is not zero.** A number the platform did not publish stays out of the median.
- **Wrong handle:** a profile that returns `NoData` means the handle is wrong. Fix it before paying 26 credits for an audience split.
- **YouTube views changed meaning on 24 August 2026,** when YouTube began counting a view the moment any video starts to play (Shorts had changed in March 2025; YouTube help centre, checked October 2026). Do not compare a channel's median across that date, and expect newer long-form counts to run higher. YouTube comment counts can be rounded (one video read exactly 1,000).

## Floors

Rules of thumb from Manifold's own reading, not platform facts. Below a floor, skip the creator unless they fit the niche exactly.

| | TikTok | Instagram | YouTube |
|---|---|---|---|
| Active | a post in the last 30 days | a post in the last 30 days | an upload in the last 30 days for a sponsorship |
| Engagement | about 3% of views | about 2% of views on reels, 1% of followers on images | about 5 likes per 1,000 views (lower often means ad-bought views) |
| View rate | about 5% of followers | about 5% of followers on reels | about 5% of subscribers |
| Niche fit | half of recent posts on the topic | the same | the same |
| Sponsored share | under about a third of recent posts | the same | the same |

A view rate far above 100% (feed-driven reach) is healthy for views but weakens every follower-based signal, including the audience split.

## Sponsored share

- **Count two signals and take the union.** `is_ad: true` marks what the platform labels; it is null where the platform does not say. Also count captions with "ad", "sponsored", "gifted", a partner tag (#brandpartner), a brand campaign hashtag, a discount code, an affiliate or shop link ("link in bio", "tap the cart"). In our sample the two agreed on 2 of 18 commercial-looking posts, and the same Instagram reel was flagged in one tool and not another.
- **On YouTube** the sponsor read is spoken. `youtube_get_transcript` on two recent videos finds "sponsored by" and "use code" and where the read sits. The description's `links` from `youtube_get_video` shows affiliate links (amzn.to, geni.us, tidd.ly and the like) and sponsor URLs.
- **Over about a third sponsored,** the feed is an ad feed and its audience skips ads.
- **Judge sponsored posts against the creator's own median.** Near it, the audience trusts the creator's recommendations; under half, it tunes them out (the usual case in our sample: 0.03x to 0.7x). Far above it, ask whether the post was boosted with paid media before crediting the creator.
- **List the brands** they worked with and flag the user's direct competitors (possible exclusivity).

## Comments

`tiktok_get_comments`, `instagram_get_comments` (leave `include_replies` off; on it costs 15) and `youtube_get_comments` on three recent posts near the median, plus one sponsored post. A real audience answers what was said, asks questions and writes in the creator's language. Warning signs: emoji-only or generic praise ("love this", "so cute") from the same few accounts, the same text from different accounts (pods, bots, AI-written comments), comments in languages the creator never uses. More than half generic is a failed check. Questions about the product under a sponsored post ("where is this from", "does it work for") are the best sign the creator sells. Reading twenty comments beats any rate.

## Followers

`tiktok_get_followers` (1 credit a page, newest followers) shows names, bios and each follower's own `followers` and `country`. Many real viewers have no bio and no followers, so a hollow page is weak evidence: in our run a healthy creator's newest followers looked as hollow as those of an account with a 0.3% view rate. Read it only together with the view rate: a mostly hollow sample and a view rate under 5% together point to a bought or dead following; the sample alone does not. A `following` close to `followers` on the profile is follow-for-follow growth. No tool shows follower history, so a following bought years ago shows only as a low view rate. Instagram and YouTube have no follower tool here.

## Audience country

- Only TikTok publishes a split: `tiktok_get_audience`, 26 credits, cached 7 days, finalists only. Read `share`, ignore `count` (followers in the sample, not in the account).
- **It is a small sample of recent followers.** Ours summed to 117 followers for a 39K account and matched the newest follower page's country mix. At that size a share carries a margin of about ±9 points: read the shape (one country dominant, or spread thin), not a five-point gap between two creators. A creator who went viral abroad last month will show it.
- **It describes followers, not viewers.** For a creator whose views routinely exceed their followers, most viewers come from the feed and may sit elsewhere.
- **Thresholds for a sales campaign:** 50% or more in the target countries passes, 30% to 50% needs a closer look, under 30% fails. For awareness with worldwide sales, say which the verdict assumes.
- **Instagram and YouTube:** no split. Report the signals (caption and comment language, the profile's `location`, places named in comments) and mark them unproven; for a creator also on TikTok, the TikTok split is a proxy. Ask the creator for a screenshot of their own analytics before a large fee.

## Verdict

A scorecard per platform: followers, median views, view rate, engagement, cadence, last post, sponsored share and sponsored views against the median, comment quality (share specific, two quoted examples, pod signs), follower sample (TikTok), audience in the market, brands worked with, effective CPM ([rates](rates.md#a-fair-price)). Then go, go at a lower fee (name the fee that meets the user's paid-social CPM), or skip, with the two reasons that decided it. A verdict reads public numbers: it cannot see reliability, contracts or past sales, so ask for a media kit with results from past deals.

## Without the connector

The same checks work by hand: open the creator's profile logged out, write down views on the last 15 posts (skip pinned and under a week old), take the median and divide by followers; mark which posts are sponsored by both the label and the caption; read twenty comments on three posts; ask the creator for their audience-country screenshot. Say plainly that a hand count on one platform is a spot check, not a measurement.
