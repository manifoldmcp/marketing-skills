# YouTube notes

What every skill that reads YouTube shares: which tools it needs, how to read the numbers, the floors, the credits and the handoff. Every skill that calls a `youtube_*` tool follows these notes.

## Tools

YouTube skills run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `youtube_search_videos` and `youtube_get_channel` (hosts often add a prefix, for example `mcp__manifold__youtube_search_videos`).

- If no manifold tool is there, stop and follow the connector check in the skill's SKILL.md.
- If other manifold tools are there but the `youtube_*` tools are not, the YouTube tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- Some skills also use other tool groups. Without `seo_*`, video ideas and the strategy lose Google demand: say so and rank on YouTube evidence alone. Without `aeo_*`, AI-cited videos can only read Google (`seo_get_serp`). Without `leads_*`, podcast guesting and creators deliver the contact the channel's own `bio` gives, unverified.

## Reading the data

- `youtube_search_videos` is YouTube's own ranked search: useful, never complete. `since` filters by upload date; `sort: "popular"` ranks by views. Leave `sort` at relevance otherwise, and use `since` for recency. It returns regular videos, not Shorts, so the tools cannot judge a Shorts strategy.
- A listing (search or `youtube_get_videos`) carries views and length (`duration_s`); likes and comments come only with `youtube_get_video`. Call that for the shortlist, not for every row.
- `created_at` on search rows and comments is approximate: YouTube shows an age ("3 weeks ago"), so a date is exact only to that unit. It is fine for "uploads in the last 90 days", not for a day-by-day timeline.
- `youtube_get_transcript` comes back `NoData` when a video has no captions: take its hook from the title and mark it.
- Views on a row are lifetime views. To compare an old video with a new one, use views per month since upload.
- `youtube_get_channel`: `followers` is subscribers, `posts_count` is the video count, `views` is total views, `created_at` is when it joined. `website` is always null for a channel here; a site or a business address, when the channel gives one, is in the `bio`.
- The `author` of a row is the channel handle that `youtube_get_channel` and `youtube_get_videos` take. When it is null, or the lookup comes back `NoData`, skip the channel.
- There is no YouTube search volume. For demand, `seo_search_keywords` gives Google's monthly volume for the phrase, and a `video` in the `features` of `seo_get_serp` means Google shows videos for that query. Both are proxies for YouTube demand; say so every time a number comes from them.

## Channel health

- **Median, not mean.** Judge a channel by the median views of its latest 10 videos (`youtube_get_videos`, default sort), leaving out videos under 14 days old: their views are still climbing. One viral video drags a mean far from what a new video will get.
- **Real audience.** A median under about 5% of subscribers (a rule of thumb) means most subscribers no longer watch: size the channel by the median, not the subscriber count.
- **Active.** No upload in the last 90 days means the channel is paused. Skip it for sponsorships and guesting; keep it as evidence for topics.
- **Outlier.** A video with at least 3 times its channel's median views is an outlier: the topic or the title pulled viewers beyond the channel's own audience. Outliers are the best evidence of demand YouTube has.
- **Engagement.** Likes and comments per 1,000 views, from `youtube_get_video`. A video with high views and under about 5 likes per 1,000 views was often promoted with ads, whose viewers rarely like or comment. Mark it as likely, not proven.

## Credits

- Every `youtube_*` call is 1 credit, a page or a record. The expensive calls in the YouTube skills are elsewhere: `aeo_run_ai_answers` (18 credits per prompt on the default five engines) and the contact steps (about 8 to 20 credits per domain).
- Transcripts are 1 credit but long. Read them for the shortlist only, to spare the context, not the credits.
- A result this account already paid for is free while cached: 6 hours for searches, listings, videos and comments, 24 hours for channels, 30 days for transcripts. A second pass the same day costs little.

## Handoff

- Never upload, post, comment, subscribe or send a pitch. The deliverable is a table or a plan. If the host has an email or sequencer tool, offer to pass the table to it; do not send.
- Write titles, scripts, comments or pitches only when the user asks, and then one per row, tied to the evidence in that row.
- The server keeps no state. If the user wants a channel or a topic watched every week, that is the [monitor-competitors](../../../monitor-competitors/SKILL.md) skill; the host schedules the run and keeps the previous results.
