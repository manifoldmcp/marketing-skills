# TikTok notes

What every skill that reads TikTok shares: which tools it needs, how to read the numbers, the floors, the credits and the handoff. Every skill that calls a `tiktok_*` tool follows these notes.

## Tools

TikTok skills run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos` and `tiktok_get_profile` (hosts often add a prefix, for example `mcp__manifold__tiktok_get_profile`).

- If no manifold tool is there, stop and follow the connector check in the skill's SKILL.md.
- If other manifold tools are there but the `tiktok_*` tools are not, the TikTok tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: every TikTok skill needs it.
- Two steps reach outside TikTok: a creator's address at their own website (the `leads_*` tools, through the [contact steps](../../../create-link-building-plan/references/outreach.md#contact-steps)) and a competitor's ads (the `ads_*` tools, in the [research-tiktok-ads](../../../research-tiktok-ads/SKILL.md) skill). If those tool groups are off, skip those steps and say so.

## Reading the numbers

- **Engagement rate** is likes plus comments over views, from the row a search or a listing returns. Where `views` is null, use `followers` from `tiktok_get_profile` as the base and say so. Shares over views is a separate signal: above the account's median, the video spreads by people sending it.
- **Baseline.** Compare a video with its own account, never with TikTok at large: the account's median views and median engagement rate over its recent videos (`tiktok_get_videos` with `sort: "latest"`, paged until you have at least 20). Use the median, not the mean: one viral video drags a mean up for months. Leave out videos under 7 days old (their views are still climbing), pinned videos (they sit first whatever their age, so order by `created_at`) and `is_ad` rows. For an author's median inside a niche sample, one page will do; call a median from fewer than 10 videos weak.
- **Outlier.** A video with views at least 3 times the account's median is an outlier. TikTok views swing widely from one video to the next, so less than that is ordinary spread. On an account whose median is under about 1,000 views (a rule of thumb), an outlier must also be in the top tenth of its window: 3 times a tiny median is noise.
- **Paid posts.** `is_ad: true` marks ads and paid partnerships. Keep them out of the organic medians and report them in their own column: their views may have been bought.
- **Cadence.** Videos a week from `created_at`, plus the share of weeks in the window with at least one video and the longest gap: an account that posts in bursts has no cadence to copy. `created_at` is UTC; convert it to the audience's time zone before reading posting hours.
- **Null is not zero.** A null number is one TikTok did not publish. Leave it out of the median.
- **Wrong handle.** A profile that comes back `NoData` means the handle is wrong: fix it before paying for listings. An empty listing on a live account means it has posted nothing: report 0 and do not page.
- **Search is a sample.** `tiktok_search_videos` is ranked and never complete. Say "in the top results for X", never "on TikTok". In search, `sort: "popular"` ranks by likes (in `tiktok_get_videos` it ranks by views), and `since: "year"` reaches back about six months. Search has no country filter: read the language of the captions and transcripts.

## Floors

The numbers here are rules of thumb, not platform facts.

- **Active.** No video in the last 30 days means the account is inactive: drop it from creator lists and flag it in competitor lists.
- **Engagement.** A median engagement rate under about 3% of views means the audience watches but does not respond. Skip such creators unless they fit the niche exactly.
- **Views against followers.** Median views under about 5% of followers means most followers no longer watch: bought, inactive or grown on an old format. It is the first sign to check with the [audience check](../../../vet-creator/SKILL.md).
- **Enough videos.** Judge an account on 10 or more videos. With fewer, report the numbers and say the median is weak.

## Credits

- Most calls cost 1 credit: a page of search, a page of an account's videos, a page of comments or followers, a profile, a transcript. Three cost more: `tiktok_get_audience` is 26, `tiktok_get_video` is estimated at 10 (1 when the vendor does not fetch the media), and `ai_fallback: true` on a transcript adds 10.
- Skip `tiktok_get_video` when a row from a search or a listing already has the video: the row carries the same numbers and caption. Call it only for a URL you have nothing else on.
- Transcripts go on the shortlist, and the audience split on the finalists only. A transcript that comes back `NoData` still costs 1 credit and means the video has no speech TikTok kept; use `ai_fallback: true` only on videos under 2 minutes that matter.
- A result this account already paid for is free while cached: 6 hours for searches, listings and comments, 24 hours for profiles, 7 days for followers and audience splits, 30 days for transcripts.

## Handoff

- Never post, comment, like, follow, message a creator or schedule anything. The deliverable is a table with a link to every video and account it cites.
- Write new hooks, scripts or captions only when the user asks, and base each on a pattern the table shows. Never pass off a creator's video or words as the user's.
- The server keeps no state. If the user wants the same check every week, the host schedules it and keeps the table: that is the [monitor-competitors](../../../monitor-competitors/SKILL.md) skill.
