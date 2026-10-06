# Facebook notes

What every skill that reads Facebook shares: which tools it needs, how to read the numbers, the floors, the credits and the handoff. Every skill that calls a `facebook_*` tool follows these notes.

## Tools

Facebook skills run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `facebook_get_posts` and `facebook_get_group_posts` (hosts often add a prefix, for example `mcp__manifold__facebook_get_posts`).

- If no manifold tool is there, stop and follow the connector check in the skill's SKILL.md.
- If the manifold tools are there but the `facebook_*` tools are not, the Facebook tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- The page audit and the Facebook strategy also read ads with `ads_get_advertiser_ads`, and group mining can look for groups with `seo_get_serp`. If the `ads_*` tools are off, deliver without the ads; if the `seo_*` tools are off, group mining needs the group links from the user.

## What the tools can read

- A page by `handle` or `url`: `facebook_get_profile` for the record, `facebook_get_posts` for its posts, newest first. A group by its `url`: `facebook_get_group_posts`, newest first. A post by its `url`: `facebook_get_post`, `facebook_get_comments` and, for a video or reel, `facebook_get_transcript`. Pages are businesses (`kind` is company); personal profiles are not covered.
- No keyword post search, no group search, and no comments across posts. Each list is one page per call with `meta.cursor` for the next; there is no complete window since a date, so page back until `created_at` passes the window and dedupe on `id`.
- Only public groups can be read. A private group returns nothing; the user can read it as a member, but the tools cannot.
- A number the platform does not publish is null, not zero. The listings (`facebook_get_posts`, `facebook_get_group_posts`) carry no share count; `facebook_get_post` does, so read shares on the shortlist only. `views` is null on posts that are not videos.
- `media` is only `video` or `text`: an image post reads as text, so a format mix can only split video from the rest. `duration_s` and `is_ad` are always null, and `verified` on `facebook_get_profile` is always null: paid posts show only in the ad library (`ads_get_advertiser_ads`, where `active` is set on Meta).

## Engagement

- Engagement on a post is `likes` (reactions) plus `comments`. Shares come only from `facebook_get_post`, so keep them out of the medians. Compare pages per 1,000 followers, not in raw counts, and use medians: one viral post drags a mean.
- Page posts reach few followers organically, and engagement well under 1% of followers per post is normal (a rule of thumb). Compare a page with its peers read the same way, not with a benchmark from elsewhere.
- Under about 8 posts in the window, a median says little. Say so rather than score it.

## Evidence

- Quote verbatim, with the post or comment link.
- Group members and commenters are private individuals. Leave their names out of the table, and never build a list of people from a group for outreach.

## Credits

- Every `facebook_*` call is 1 credit, per page of results for the lists, and `ads_get_advertiser_ads` is 1 credit per page. A `NoData` answer (a video with no speech, a private group) still costs 1; do not retry it.
- Caches: a page's record 24 hours, posts and comments 6 hours, transcripts 30 days, ads 24 hours. A result this account already paid for is free while cached.

## Handoff

- Never post, comment, react, join a group or send messages. The deliverable is a table.
- Draft a post, a reply or a comment only when the user asks, and then in the page's own voice, answering what the comment or post asked.
- The server keeps no state. To watch a page or a group every week, the host schedules the calls and keeps the ids already seen: a job for the [monitor-competitors](../../../monitor-competitors/SKILL.md) skill.
