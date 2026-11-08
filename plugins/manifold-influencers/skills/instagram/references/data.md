# Reading Manifold's Instagram data

How the `instagram_*` tools behave, what their fields really hold, and how to turn rows into numbers that survive scrutiny. Every Instagram job in this skill reads its numbers this way. Checked against live pulls on 15 October 2026 (see [field notes](field-notes.md)).

## Which tool answers what

| Question | Tool | Cost | Cached |
|---|---|---|---|
| How big is the account, what does its bio say | `instagram_get_profile` | 1 | 24 hours |
| Its reels with views, newest first | `instagram_get_reels` | 1 a page of 12 | 6 hours |
| All its posts (images, carousels, reels) | `instagram_get_posts` | 1 a page of 12 | 6 hours |
| Who posts under a hashtag | `instagram_search_posts` | 1 a page of about 10 | 6 hours |
| What people said under one post | `instagram_get_comments` | 1 a page of 15; 15 with `include_replies: true` | 6 hours |
| What is said in one reel | `instagram_get_transcript` | 1 | 30 days |
| One post from a bare URL | `instagram_get_post` | estimated 10 (1 when the vendor need not fetch the media) | 6 hours |

- A result this account already paid for is free while cached. `dry_run: true` prices any call for free; pass `max_credits` on every call when the user gave a budget, and stop at `BudgetExceeded`.
- Skip `instagram_get_post` when a listing or search row already has the post: it carries the same numbers and caption.
- `include_replies: true` is charged 15 even when no reply comes back. Use it only on a post whose top comments show many `replies` and where the thread matters.
- No tool returns followers lists, audience demographics, stories, saves, sends, reach, watch time, skip rate or profile visits. For the user's own account those are in Instagram Insights: ask the user for them. For a creator also on TikTok, the TikTok audience data in the [creators](../../creators/SKILL.md) skill is the nearest proxy for who follows them.

## Field traps

- **No post count, and `kind` misses most brands.** `posts_count` is null: Instagram shows a signed-out visitor 12 posts whatever the account has, so count posts from the listings. `kind` is company only for an account Instagram flags as a business; on 15 October 2026 every profile tested read person, brands included. Tell a brand from `industry` (Instagram's category label) or the bio. `email` and `phone` were null even where the bio gave an address: read the bio.
- **`likes` is null where the owner hides like counts** in the listings and `instagram_get_post`. Hashtag search cannot tell: there a `likes` of exactly 3 is a hidden count. Leave both out of any engagement rate. `comments` of 0 on a reel with large views usually means comments are off.
- **Listings include other accounts' posts.** Collab posts where the account was invited come back with another `author`. Filter on `author` before any account median; list collabs apart (they show who the brand works with).
- **Pinned posts come first.** Up to three old posts can sit at the top. Sort by `created_at` and drop pinned rows from cadence and medians.
- **Reel views come only from `instagram_get_reels` and search.** In `instagram_get_posts`, video rows have `views`, `duration_s` and `is_ad` null, and image rows never have views. `media` says only video or image: a carousel reads as an image.
- **`is_ad: true`** marks ads and paid partnerships in reels and search rows. In the posts listing it is null, not false: read the caption for #ad or a partner tag instead.
- **`shares` and `saves` are always null** on Instagram here.
- **`music`** is filled in hashtag search rows ("Original audio" or a track and artist) and empty in account listings. For an account's sounds, search its hashtags or check the app.
- **Fresh numbers move.** A reel's views can triple within a day. Search rows can lag the listing, or show views as null; take views from the listing.
- **`created_at` is UTC.** Convert to the audience's time zone before reading posting days or hours.
- **Comments come newest first,** 15 to a page, not by likes. Ranking by likes ranks only what was pulled; for the most-liked comments on a busy post, pull several pages.
- **Comment-for-DM and giveaway posts inflate comments.** "Comment RECIPE and I'll send it" reels can carry as many comments as likes. Count those keyword comments as demand for the offer, then leave the post out of comment-based medians.
- **Transcripts:** `NoData` means no speech (music and text on screen) and is charged. A reel over two minutes returns `InvalidTarget` at no charge; do not retry it, and read its caption instead. Pass `language` for multilingual reels.
- **Hashtag search** takes one tag (no free text, no sort), returns posts in Instagram's own order (neither newest nor most viewed), and sees only posts that carry the tag; many top reels carry none. Read the niche's real tags off the captions of the first results; broad tags (#reels, #explore, #love) are noise. Since the five-hashtag cap (December 2025) captions carry fewer tags, so every tag search covers less.
- **A wrong handle** returns `NoData` on the profile: fix it before paying for listings. An empty reels listing on a live account means it posts no reels.

## Turning rows into numbers

- **Baseline:** judge a post against its own account, never against Instagram at large. Take the median, not the mean (one viral reel drags a mean for months), over at least 10 posts of a kind, and say "weak" below that. Report reels and image posts apart. Leave out paid rows, pinned rows, other authors' rows and posts under 7 days old (views still climbing). One page of 12 reels can span six days or two years: page until the window is covered.
- **Engagement rate:** for a reel, likes plus comments over views; for an image or carousel, likes plus comments over followers. Never mix the two bases in one median, and never compare either with a published benchmark on a different base (see [benchmarks](benchmarks.md)).
- **Outlier:** a reel with at least 3 times the account's median views, or an image post with 3 times its median likes. On an account whose median is under about 1,000 views, also require the top tenth of its window: 3 times a tiny median is noise. The count of outliers in a window says more than the single biggest post: several mean a repeatable format, one means luck.
- **Cadence:** posts a week from `created_at`, the share of weeks with at least one post, and the longest gap.
- **Floors** (rules of thumb, not platform facts): no post in 30 days is inactive. For a creator being vetted, median reel views under about 5% of followers, or engagement under about 1% of views, means the following no longer watches; that floor is wrong for brands, whose reels typically reach 3% to 10% of followers (see [field notes](field-notes.md)). Compare a brand only with its direct competitors.

## Without the connector

The method works by hand, slowly: open each account in a logged-out browser, note the follower count, then the last 12 to 20 reels' view counts (shown on the profile's Reels tab) and dates, skipping pinned ones; compute the median and the outliers as above. Comments and captions can be read the same way. Instagram's own Competitive Insights in the Professional Dashboard compares the user's follower gains and posting frequency with up to 10 competitors for free. Say plainly that a hand count of a few accounts is a sample, and never fill a gap with a guessed number.
