# LinkedIn notes

What every skill that reads LinkedIn shares: which tools it needs, how to read the numbers, the floors, the credits and the handoff. Every skill that calls a `linkedin_*` tool follows these notes.

## Tools

LinkedIn skills run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_search_posts` and `linkedin_get_post` (hosts often add a prefix, for example `mcp__manifold__linkedin_search_posts`).

- If no manifold tool is there, stop and follow the connector check in the skill's SKILL.md.
- If other manifold tools are there but the `linkedin_*` tools are not, the LinkedIn tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- These skills use only the `linkedin_*` tools. Email addresses for the people they find come from the [enrich-lead-list](../../../enrich-lead-list/SKILL.md) skill, which needs the `leads_*` tools; without them, deliver names and profile links.

## What LinkedIn shows

- `linkedin_search_posts` is LinkedIn's own ranked search: never complete, 1 credit a page, filtered by `since` (day, week, month, year or all) and with no other sort. Its ranking tends to favour posts that already have engagement.
- A post row names its author three ways: `author` (the profile's handle), `author_name` and `author_url`, plus `author_followers`. Size authors from the row; call `linkedin_get_profile` (1 credit) only for the `bio`, `location` or `experience`, passing `author_url` as `url`. An `author_url` under `/company/` is a company page: `linkedin_get_company` reads it.
- Engagement is `likes` and `comments`. LinkedIn publishes no view count (`views` is null). Search rows usually carry both counts; when they are null, `linkedin_get_post` (1 credit) has them. `linkedin_get_company_posts` carries no engagement at all, so every post judged from it needs `linkedin_get_post`.
- `media` is `video` for a video post and `image` for a photo post. A document carousel or a poll may still read as `text`, so before judging a format on media, the host opens the top post URLs if it has a browser, or the user checks them by eye. A search row for a video post often carries what it says in `transcript`.
- `text` is cut at 2,000 characters, and `created_at` is approximate: LinkedIn shows an age ("3d", "2w"), exact only to that unit.
- Comments, partly. A post and a search row carry up to 10 comments in `top_comments`, each with the commenter's name and `author_url`; `linkedin_get_comments` (1 credit) returns them as rows. That is all LinkedIn shows a logged-out reader: on a post with 200 comments, 10 come back, and who reacted is never shown. Judge a conversation by its counts and read the 10 as a sample.
- A person's latest posts. `linkedin_get_profile` returns `recent_posts`, their five latest (URL, first lines, date). Further back, search the topics they post about and keep the rows whose `author` is theirs.
- `linkedin_get_company` carries `followers`, the employees the page shows in `people`, similar pages in `related` and its specialties in `topics`.

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
