# Reading Manifold's Facebook data

What the `facebook_*` tools can and cannot see, what their fields really hold, and what each call costs. Every other reference in this skill assumes these notes. Checked against the tool descriptions and live calls on 15 October 2026 (see [field notes](field-notes.md)).

## What each tool answers

| Question | Tool | Notes |
|---|---|---|
| Is this the brand's real page? Category, contact, is it advertising? | `facebook_get_profile` (`handle` or `url`) | 1 credit, cached 24 hours |
| What does the page post, how often, what gets a response? | `facebook_get_posts` | 1 credit a call, about 3 posts a call |
| One post's shares, views and length | `facebook_get_post` | 1 credit; the only source of shares, reel views and duration |
| What people replied | `facebook_get_comments` | 1 credit a call, 10 top-level comments a call |
| What a video or reel says | `facebook_get_transcript` | 1 credit, cached 30 days |
| What a public group talks about | `facebook_get_group_posts` (the group `url`) | 1 credit a call, 3 to 5 posts a call |
| Which groups and threads exist on a topic | `seo_get_serp` | Google indexes public group threads; there is no Facebook search tool |
| What the page pays to run | `ads_get_advertiser_ads` with `platform: "facebook"` | the ads themselves belong to [paid-ads](../../paid-ads/SKILL.md) |

Pages only: `kind` is always company. Personal profiles, including a founder posting from a personal profile, are not covered; the user brings those numbers.

## Traps in the fields

- **Listings are small.** In our October 2026 calls a page listing returned 3 posts a call and a group listing 3 to 5. Five calls cover about 15 page posts, which for a page posting daily is under two weeks, not 90 days. Size the window in posts, not days: about 30 posts (10 credits) for a page audit, and say what date range they reached.
- **Views in listings are unreliable.** `views` came back null on reels in a page listing even when `facebook_get_post` on the same reel gave 7,310 views. Read reel views from `facebook_get_post` on the shortlist; never compute a median view count from a listing where most videos are null.
- **Shares only from the single post.** `shares` is null in both listings; `facebook_get_post` fills it. Keep shares out of medians built from listings.
- **`media` is only video or text.** It is inferred from the view count and the URL shape, so an image, an album and a status post all read as text, and a video without a view count may too. A format split can only be video against the rest; Socialinsider's format benchmarks (in [feed](feed.md)) cannot be reproduced from this data.
- **`duration_s` is null in listings,** filled by `facebook_get_post` for videos. `is_ad` is always null: Facebook rows never say a post was boosted. `verified` on `facebook_get_profile` is always null and `posts_count` is null.
- **Comments are top-level only.** `depth` is always 0 and `replies` counts the replies without returning them. A post showing 29 comments needs three calls for all the top-level ones. `author_url` and `post_id` came back null.
- **`top_comments[]` on listings** carries a few comments Facebook shows under each post, with commenters' names. Useful as a free preview of which posts are worth a comments call; not a sample of opinion.
- **Group posts can have null `text`** (an image or a share with no caption) and an author of "Anonymous participant" where the group allows anonymous posts.
- **Profile extras worth reading:** `running_ads` says whether the page advertises now and `ad_library_id` is what `ads_get_advertiser_ads` takes as `advertiser`; `industry` is the page category; `phone`, `email`, `website` and `location` show whether the page is complete. Ratings and reviews are not returned; a Google result for the page often shows them in its snippet (for example "92% recommend (20 reviews)").
- **A null is unpublished, not zero.** Leave nulls out of every median and share.

## Transcripts

- Speech only. Text burned into the video, captions and visuals are not in it, and much of Facebook watches with the sound off. When a top video's transcript is thin or empty, say so rather than read meaning into it.
- Auto-generated: on a music-heavy reel our test transcript was a garbled run of lyrics and fragments, with `language` null. Treat a transcript with no clear sentences as no speech.
- `NoData` (no speech) is free; do not retry it.

## Groups

- Only public groups can be read. A private group, or a link that is not a group, returns nothing and still costs 1 credit.
- Group listings run newest first and are never a complete window: page with `meta.cursor` and dedupe on `id`. A busy local group added 8 posts in 7 hours in our test, so five calls are a few hours of a busy group and months of a quiet one.
- `facebook_get_post` and `facebook_get_comments` read a public group thread by its URL, including year-old threads found through Google. That is the way to read a group on a topic, because the group listing has no keyword filter (see [groups](groups.md)).

## Credits and caches

- Every `facebook_*` call is 1 credit, per call for the lists. `ads_get_advertiser_ads` is 1 credit per call. `seo_get_serp` is 1 credit for the top 10 and 1 more with `ai_overview: true`.
- Caches: page record 24 hours; posts, comments and single posts 6 hours; transcripts 30 days; ads 24 hours. A cached result this account already paid for is free.
- Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Typical costs: a page baseline (record, about 30 posts, three posts in full, one call of ads) about 15 credits a page, so about 60 for the user and three competitors. A comment mine (about 50 posts to pick from, then two calls on each of 15 posts), about 47. A group mine (three Google searches, five calls on each of three groups, 15 threads read with their comments), about 50.

## People in the data

Group members and commenters are private individuals and their names come back in the rows. Quote their words with the post link as evidence for the user; leave names out of tables and deliverables; paraphrase in anything public. Never build a list of people from a group or a comment thread for outreach; lead data rules live in [outbound](../../outbound/SKILL.md).

## Never act on the platform

Never post, comment, react, follow, join a group or send a message on the user's behalf. The deliverable is a table or a plan with a link to every page, post and group it cites. Draft a post or reply only when the user asks, in the page's own voice. The server keeps no state: a weekly watch of a page or group is the host's schedule, storing the last `id` seen (see [brand-monitoring](../../brand-monitoring/SKILL.md) and [competitor-intel](../../competitor-intel/SKILL.md)).
