# LinkedIn notes

What every skill that reads LinkedIn shares: which tools it needs, how to read the numbers, the floors, the credits and the handoff. Every skill that calls a `linkedin_*` tool follows these notes.

## Tools

LinkedIn skills run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_search_posts` and `linkedin_get_post` (hosts often add a prefix, for example `mcp__manifold__linkedin_search_posts`).

- If no manifold tool is there, stop and follow the connector check in the skill's SKILL.md.
- If other manifold tools are there but the `linkedin_*` tools are not, the LinkedIn tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- These skills use only the `linkedin_*` tools. Email addresses for the people they find come from the [enrich-lead-list](../../../enrich-lead-list/SKILL.md) skill, which needs the `leads_*` tools; without them, deliver names and profile links.

## What LinkedIn shows

- `linkedin_search_posts` is LinkedIn's own ranked search: never complete, 1 credit a page, filtered by `since` (day, week, month, year or all) and with no other sort. Its ranking tends to favour posts that already have engagement.
- A search row carries the author's name (`author_name`) but no handle. A post URL of the form `linkedin.com/posts/<handle>_...` starts with the author's handle, the part before the first underscore: pass it as `handle` to `linkedin_get_profile` (1 credit) for `followers`, `location` and the `bio`, or to `linkedin_get_company` when the author is a company. A URL without that form cannot be traced to a profile; keep the name only.
- Engagement is `likes` and `comments`. LinkedIn publishes no view count (`views` is null). Search rows usually carry both counts; when they are null, `linkedin_get_post` (1 credit) has them. `linkedin_get_company_posts` carries no engagement at all, so every post judged from it needs `linkedin_get_post`.
- The post type is not visible: every row reads `media: "text"`, so a carousel, an image, a video or a poll looks like a text post. The data judges the words (length, hook, structure); to see the media, the host opens the post URL if it has a browser, or the user checks the top posts by eye.
- `text` is cut at 2,000 characters, and `created_at` is approximate: LinkedIn shows an age ("3d", "2w"), exact only to that unit.
- No comments. The tools cannot read what people replied, or who liked or commented. Judge a conversation by its counts, and let the user read the thread.
- No feed per person. A person's own posts cannot be listed. Find them through search on the topics they post about and keep the rows under their name. For the user's own posts, ask for the URLs and read each with `linkedin_get_post`.
- `linkedin_get_company` carries no follower count (`followers` is null); compare pages on posting and engagement, not audience size.

## Thresholds

- **Sample.** Judge a format, a hook or a leader on at least 30 posts in total and 10 per group compared. A pattern in five posts is an anecdote.
- **Size matters.** Compare engagement per 1,000 followers when the authors differ in size. 200 likes from a 5,000-follower author is a stronger post than 2,000 from 500,000.
- **Median, not mean.** One viral post drags a mean; report medians.
- **Giveaways.** A post that asks readers to "comment X" for a guide gets comments by design. Count it apart, or it inflates every comment figure.
- **Fresh.** Most of a post's reach comes in its first days. For commenting, only posts from the last few days are worth it; for judging formats, a month or a year is fine.

## Credits

- Every `linkedin_*` call is 1 credit, a page or a record, so a run costs tens of credits. Profiles and single posts are where the count grows: fetch them for the shortlist only.
- A result this account already paid for is free while cached: 6 hours for searches, posts and company feeds, 24 hours for profiles and company pages.

## Handoff

- Never post, comment, like, connect or message. The deliverable is a table or a plan; the user writes and posts in their own voice.
- Draft a post or a comment only when the user asks, one per row, tied to the evidence in that row, and marked as a draft to edit.
- The server keeps no state. A daily commenting list or a weekly watch of a topic is a job the host schedules, running [find-linkedin-posts-to-comment](../../../find-linkedin-posts-to-comment/SKILL.md) or [find-linkedin-buyer-posts](../../../find-linkedin-buyer-posts/SKILL.md) on that schedule, and keeps the posts already seen.
