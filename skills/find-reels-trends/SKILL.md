---
name: find-reels-trends
description: When the user wants to know what is trending on Instagram Reels in their niche right now. Finds the formats, topics and lengths that several accounts are using this week or month and that beat their own median reel views, from hashtag searches on Instagram, with example reels and a line on how the user could use each trend. Also use when the user mentions Reels trends, Instagram trends, what's trending on Reels, which Reels formats are taking off, trending Reels topics this week, or what to film next for Instagram. What is trending on TikTok goes to find-tiktok-trends; how the top reels open to find-instagram-hooks; why one reel went viral to analyze-viral-reel; ideas from search and Reddit demand to find-content-ideas; a 90-day Instagram plan to create-instagram-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find Reels trends

What is rising on Instagram Reels in a niche right now: the formats, topics and lengths that several accounts are using this week and that beat their own usual numbers. It hands back a short, dated table of trends, each with example reels and a line on how the user could use it.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `instagram_search_posts` and `instagram_get_reels` (hosts often add a prefix, for example `mcp__manifold__instagram_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `instagram_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: this skill needs them.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the niche, the category and the problems it solves, the market and its language) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Hashtags**: three or four tags the niche uses ("homedecor", "smallapartment", "rentalfriendly"). Default: the category tag, then the tags that recur in the captions of its first results.
- **Window**: `since: "week"` by default. Use `since: "month"` for a slow niche or when the user asks about the month.
- **Language**: hashtag search has no country filter, so name the language to keep. Default: English.
- **Budget**: a default run costs about 4 x 2 + 10 + 10 = 28 credits for four hashtags. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the [Instagram notes](../create-instagram-plan/references/platforms/instagram.md)** before the first call. They say how to read Instagram's numbers and what each call costs.
2. **Pull the recent reels.** `instagram_search_posts` for each hashtag with `since: "week"`, two pages each (1 credit a page). Keep the reels (`media: "video"`). Drop rows with `is_ad: true`, duplicates across hashtags, and captions in other languages. The rows come in Instagram's own order; there is no sort to ask for.
3. **Measure speed.** For each reel, views per day since `created_at`, and rank on it.
4. **Separate the trend from the account.** For the authors of the 10 fastest reels, `instagram_get_reels`, one page each (1 credit), and compare each reel with its author's median reel views. A reel at 3 times its author's median or more was lifted by what it did. A large account posting at its usual numbers is no evidence of a trend.
5. **Name the format.** `instagram_get_transcript` on the 10 strongest outliers (1 credit each). With the caption, name each format: talking head, voiceover over footage, tutorial, list, before and after, day in the life, POV or skit, or no speech (`NoData`: text on screen over music). A reel over two minutes comes back `InvalidTarget`: name its format from the caption.
6. **Group.** Group the outliers by format, by topic (the words and other hashtags that recur in `text`) and by length (`duration_s` under 15 seconds, 15 to 60, over 60). A group is a trend when it holds three or more reels from three or more accounts in the window. The rows carry no audio data: say so, and point the user to the trending audio shown in the app's Reels audio picker.
7. **Deliver** a table: trend (a format, a topic or a length), what it is in one line, reels in the window, accounts, median views, median views per day, median multiple of the account's own median, two example links with their first spoken line or caption line, how the user could use it, and the date the data was pulled.

## Judgment

- One viral reel is not a trend. Three accounts doing the same thing and beating their own medians is.
- Trends on Reels fade within one to three weeks. Date the table; a trend whose oldest example is two weeks old is probably at its peak.
- The trend is the format or the topic, never the reel. Advise the user to make their own version, not a copy.
- Hashtag search sees only posts that carry the tag, and many top reels carry none. The sample leans towards accounts that tag their posts; say so.
- A hashtag with fewer than about 20 reels in a week is too thin for a weekly read. Run it again with `since: "month"` and say the window changed.
- Business accounts on Instagram get a narrower music library than personal ones, so a trend that rides a popular song may be closed to a brand. Flag the ones that depend on the audio.
- When the user also wants TikTok, run [find-tiktok-trends](../find-tiktok-trends/SKILL.md) and keep the tables apart: views compare only within one platform.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Searches and listings this account already paid for are free for 6 hours, transcripts for 30 days.
- **Handoff.** The deliverable is a table of evidence. Never post or schedule. For the same read every week, the host schedules this skill and keeps the previous table; the server keeps no state.

## Related skills

- How the top reels open: [find-instagram-hooks](../find-instagram-hooks/SKILL.md). Why one reel beat its account's usual numbers: [analyze-viral-reel](../analyze-viral-reel/SKILL.md).
- What is rising on TikTok: [find-tiktok-trends](../find-tiktok-trends/SKILL.md).
- Ideas from what people search and ask: [find-content-ideas](../find-content-ideas/SKILL.md). Four weeks of dated posts: [create-content-calendar](../create-content-calendar/SKILL.md).
- An Instagram plan: [create-instagram-plan](../create-instagram-plan/SKILL.md). Instagram creators to make content with: [find-instagram-creators](../find-instagram-creators/SKILL.md).
