# X notes

What every skill that reads X (Twitter) shares: which tools it needs, what X shows, how to read the numbers, the credits and the handoff. Every skill that calls a `twitter_*` tool follows these notes.

## Tools

X skills run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `twitter_get_profile` and `twitter_get_tweets` (hosts often add a prefix, for example `mcp__manifold__twitter_get_tweets`).

- If no manifold tool is there, stop and follow the connector check in the skill's SKILL.md.
- If other manifold tools are there but the `twitter_*` tools are not, the X tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## What X shows

- Three tools: `twitter_get_profile` for an account, `twitter_get_tweets` for one page of its recent posts, newest first, and `twitter_get_tweet` for one post by URL. `twitter_get_tweets` returns a null cursor: there is no older history to page into, so a post older than the page needs its URL.
- No search and no replies. Accounts cannot be found by topic here, and what people replied cannot be read; mentions of a brand by other accounts are not visible.
- `comments` counts replies and `shares` counts reposts. Every row reads `media: "text"`, so whether a post carried an image or a video is unknown; judge the words, and open a post on X when it matters. `is_ad` is always null.
- `verified` on a profile is true for a paid checkmark too, so it says nothing about standing.

## Reading the numbers

- **Own posts only.** Drop rows whose text starts with "RT @" (reposts carry the original's numbers) and rows that start with "@" (replies), and leave out posts under 48 hours old: most of a post's views arrive in its first day or two.
- **Span.** The page covers a few days for an account posting ten times a day, months for one posting weekly. State the span, and compare cadence per week, never per page.
- **Engagement per view** is likes, replies and reposts over `views`. A null `views` is one X did not publish: leave the post out of the median.
- **Median, not mean.** One viral post makes an average meaningless. Median views over followers says whether the audience still looks.

## Credits

- Every `twitter_*` call is 1 credit. A result this account already paid for is free while cached: 24 hours for profiles, 6 hours for posts.

## Handoff

- Never post, reply, like, repost, follow or message. The deliverable is a table with a link to every post it cites.
- Draft posts only when the user asks, from the posts that worked.
- The server keeps no history. To see change over time, the host keeps each run's table and runs the same handles again: a job for the [monitor-competitors](../../../monitor-competitors/SKILL.md) skill.
