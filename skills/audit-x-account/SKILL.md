---
name: audit-x-account
description: When the user wants to know what an X (Twitter) account does and what works for it, their own or a competitor's. Reads one account's profile and latest posts, measures cadence, median views per follower and engagement per view, tags each post's topic and shape, finds its outliers, and sets it against one or two competitor accounts read the same way, ending in a scorecard and changes. Also use when the user mentions an X or Twitter audit, audit our Twitter, how does our Twitter compare to a competitor's, what a founder or brand posts on X, or which tweets get the views. TikTok goes to audit-tiktok-account, Instagram to audit-instagram-account, YouTube to audit-youtube-channel, Facebook to audit-facebook-page, LinkedIn to audit-linkedin-page; turning a video or post into a thread to repurpose-content; a rival beyond social to tear-down-competitor.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Audit an X account

X has three tools here and no search or replies, as the [X notes](references/platforms/x.md) say. So the audit reads one account's own feed (how often it posts, what it posts, what earns views and engagement against its own median) and sets it against a competitor's feed read the same way. It ends in a scorecard, with a link to every post it cites, and a short list of changes.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `twitter_get_profile` (hosts often add a prefix, for example `mcp__manifold__twitter_get_profile`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `twitter_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the user's own handle, the competitors and their accounts) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Account**: the user's X handle.
- **Competitors**: one or two handles in the same market: a peer, and one account the user would like to be. X has no search, so they cannot be found here; ask, or take them from the competitor's site if the host can open it.
- **Goal**: what the account is for: followers, clicks to the site, conversation, or the founder's reputation. It picks the metric that leads the scorecard.
- **Posts older than the feed**: the URLs of a pinned post, a launch post or a post the user remembers doing well, if they want those in the audit.
- **Budget**: a default run costs about 3 x (1 + 1) + 5 = 11 credits for the account, two competitors and five older posts. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the platform notes** before the first call: [X notes](references/platforms/x.md).
2. **Profiles.** `twitter_get_profile` on each handle (1 credit each). Read `followers`, `following`, `posts_count`, `created_at`, `bio` and `website`. Does the bio say who the account is for, and does it link the site?
3. **Recent posts.** `twitter_get_tweets` on each handle (1 credit each). One page, newest first, with no older history. Note the span of the page, from the oldest to the newest `created_at`. Per post: views, likes, `comments` (replies) and `shares` (reposts).
4. **Measure.** For each account, leave out reposts, replies and posts under 48 hours old, as the [X notes](references/platforms/x.md#reading-the-numbers) say, then compute:
   - posts per week: posts on the page divided by the span in days, times 7;
   - median views per post, and median views divided by followers (whether the audience is alive);
   - median engagement per view: likes, replies and reposts divided by views;
   - the share of posts with a link, of thread openers (a "1/" marker or a numbered list), and of questions.
5. **Tag the posts.** Read each post's text and tag its topic (product news, how-to, opinion, story, industry news, promotion, engagement bait) and its shape (one line, long post, thread opener, link post, question). Shape comes from the text, since the rows do not say whether a post carried media.
6. **Find the outliers.** For each account, the three posts with the most views relative to its median and the three with the least. Read them side by side: what do the top ones share (a topic, a shape, a number in the first line, a link or none)?
7. **Read the older posts.** `twitter_get_tweet` on each URL the user gave (1 credit each): the feed page does not reach them. Set their numbers against today's median to see whether the account has grown or faded since.
8. **Deliver** two tables and a list.
   - Scorecard: metric (followers, posts per week, median views, views per follower, median engagement per view, link share, thread share, top topic), the user's account, each competitor, and what the gap means. The metric that matches the goal comes first.
   - Posts to learn from: account, post URL, views, engagement, topic, shape, and why it worked in one line.
   - Three to five changes, each tied to a row (for example: post four times a week instead of one; lead with the numbered how-to threads that earn three times the median; add the site link to the bio, which has none).
   Drafts of new posts only when the user asks; the host writes them from the posts that worked.

## Judgment

- **Own baseline.** Judge a post against its own account's median, never against X at large.
- **Followers are not reach.** Median views per follower is the better health number: a large account with low views per follower has an audience that stopped looking.
- **Size apart.** A competitor ten times the size is a benchmark for topics and shapes, not for raw numbers. Compare views per follower and engagement per view instead.
- **Repeatable, not lucky.** Several outliers of one topic or shape mean a repeatable format; one means luck. Shapes an account keeps posting usually work for it; one it tried once and dropped probably did not.
- **A snapshot.** An audit is a snapshot. To see whether the changes worked, run it again in four to six weeks on the same handles; the server keeps no history, so the host keeps this run's scorecard.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- **Handoff.** The deliverable is a scorecard with a link to every post and account it cites. Never post, reply, follow or message. A weekly watch of a rival's posts is the host's schedule, through [monitor-competitors](../monitor-competitors/SKILL.md).

## Related skills

- The same audit on other platforms: [audit-linkedin-page](../audit-linkedin-page/SKILL.md), [audit-youtube-channel](../audit-youtube-channel/SKILL.md), [audit-tiktok-account](../audit-tiktok-account/SKILL.md), [audit-instagram-account](../audit-instagram-account/SKILL.md), [audit-facebook-page](../audit-facebook-page/SKILL.md). Run each and set the results side by side, comparing accounts only within one platform.
- What to post next, from real questions: [find-content-ideas](../find-content-ideas/SKILL.md). When to post it: [create-content-calendar](../create-content-calendar/SKILL.md).
- Turning a video, podcast or blog post into a thread: [repurpose-content](../repurpose-content/SKILL.md).
- Everything about a competitor beyond X (search, ads, messaging, pricing): [tear-down-competitor](../tear-down-competitor/SKILL.md).
