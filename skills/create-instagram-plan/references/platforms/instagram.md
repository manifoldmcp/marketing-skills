# Instagram notes

What every skill that reads Instagram shares: which tools it needs, how to read the numbers, the floors, the credits and the handoff. Every skill that calls a `instagram_*` tool follows these notes.

## Tools

Instagram skills run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `instagram_search_posts` and `instagram_get_profile` (hosts often add a prefix, for example `mcp__manifold__instagram_get_profile`).

- If no manifold tool is there, stop and follow the connector check in the skill's SKILL.md.
- If other manifold tools are there but the `instagram_*` tools are not, the Instagram tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: every Instagram skill needs it.
- Two steps reach outside Instagram: a creator's address at their own website (the `leads_*` tools, through the [contact steps](../../../create-link-building-plan/references/outreach.md#contact-steps)) and a competitor's ads in Meta's library (the `ads_*` tools, in the [research-meta-ads](../../../research-meta-ads/SKILL.md) skill). If those tool groups are off, skip those steps and say so.

## Reading the numbers

- **Engagement rate.** For a reel, likes plus comments over views. For an image or carousel post, `views` is null, so use likes plus comments over `followers` from `instagram_get_profile`. Never mix the two bases in one median: report reels and image posts apart.
- **What is missing.** `shares` is always null on Instagram here, and saves, reach and stories are not in the tools at all. A null number is one Instagram did not publish, such as views on an image post: leave it out of the median.
- **Baseline.** Compare a post with its own account, never with Instagram at large: the account's median over its recent reels (`instagram_get_reels`) and, apart, its recent image posts (`instagram_get_posts`), paged until you have at least 20 of each kind it posts. Use the median, not the mean: one viral reel drags a mean up for months. Leave out posts under 7 days old (their views are still climbing), pinned posts and `is_ad` rows. For an author's median inside a niche sample, one page will do; call a median from fewer than 10 posts weak.
- **Outlier.** A reel with views at least 3 times the account's median, or an image post with likes at least 3 times its median, is an outlier. Less than that is ordinary spread. On an account whose median is under about 1,000 views (a rule of thumb), an outlier must also be in the top tenth of its window: 3 times a tiny median is noise.
- **Cadence.** Posts a week from `created_at`, plus the share of weeks in the window with at least one post and the longest gap. `created_at` is UTC; convert it to the audience's time zone before reading posting hours.
- **Paid posts.** `is_ad: true` marks ads and paid partnerships. Keep them out of the organic medians and report them in their own column: their reach may have been bought.
- **Listings run newest first.** `instagram_get_posts` and `instagram_get_reels` have no popular sort. To find an account's top posts, page back through the window and sort the rows yourself. Order by `created_at`: a pinned post can come first whatever its age. `media` says only video or image; a carousel reads as an image post.
- **Search is by hashtag only.** `instagram_search_posts` takes one hashtag and `since`, with no sort and no free text, and its results are ranked by Instagram and never complete. Use the hashtags the niche actually uses (read them off the captions of the first results); a broad tag like #love is noise. Keep reels with `media: "video"` when the job is about Reels.
- **Transcripts** cover reels up to two minutes, though reels can run to three. A longer reel comes back `InvalidTarget`: skip it. `NoData` means no speech: the reel runs on music and text on screen.
- **Wrong handle.** A profile that comes back `NoData` means the handle is wrong: fix it before paying for listings. An empty `instagram_get_reels` on a live account means it posts no reels: report 0 and do not page.
- **No audience data.** Instagram publishes no audience split and the tools have no follower list. For a creator also on TikTok, the TikTok [audience check](../../../vet-creator/SKILL.md) on that handle is the nearest proxy.

## Floors

The numbers here are rules of thumb, not platform facts.

- **Active.** No post in the last 30 days means the account is inactive: drop it from creator lists and flag it in competitor lists.
- **Engagement.** A median reel engagement rate under about 2% of views, or a median image-post engagement rate under about 1% of followers, means the following no longer responds. Skip such creators unless they fit the niche exactly.
- **Views against followers.** Median reel views under about 5% of followers means most followers no longer watch: bought, inactive or grown on an old format.
- **Enough posts.** Judge an account on 10 or more posts of a kind. With fewer, report the numbers and say the median is weak.

## Credits

- Most calls cost 1 credit: a page of hashtag search, a page of posts or reels, a page of comments, a profile, a transcript. Two cost more: `instagram_get_post` is estimated at 10 (1 when the vendor does not fetch the media), and `include_replies: true` on comments costs 15 a page, charged even when no reply comes back.
- Skip `instagram_get_post` when a row from a search or a listing already has the post: the row carries the same numbers and caption. Call it only for a URL you have nothing else on.
- Leave `include_replies` off unless a comment the job depends on shows many `replies`. Transcripts go on the shortlist only.
- A result this account already paid for is free while cached: 6 hours for searches, listings and comments, 24 hours for profiles, 30 days for transcripts.

## Handoff

- Never post, comment, like, follow, send a DM or schedule anything. The deliverable is a table with a link to every post and account it cites.
- Write new hooks, captions or scripts only when the user asks, and base each on a pattern the table shows. Never pass off a creator's post or words as the user's.
- The server keeps no state. If the user wants the same check every week, the host schedules it and keeps the table: that is the [monitor-competitors](../../../monitor-competitors/SKILL.md) skill.
