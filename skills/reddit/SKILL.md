---
name: reddit
description: Reddit marketing with the manifold tools. Finds the subreddits where the user's buyers talk, reads each community's rules on self promotion, finds recent threads where a helpful reply fits, mines Reddit for pain points and for complaints about competitors, finds the Reddit threads that ChatGPT, Perplexity and Google show for the category, and plans a Reddit strategy. Use when the user asks for Reddit marketing, a Reddit strategy, which subreddits to post in, subreddit rules or whether self promotion is allowed, threads to reply to or comment on, Reddit engagement, what Redditors complain about, pain points or customer language from Reddit, complaints about a competitor on Reddit, "alternative to X" or "switched from X" threads, or Reddit threads that AI answers cite. The result is a table for the user, who posts from their own account; posting, replying, voting, messaging and scheduling are out of scope, and watching Reddit over time belongs to monitoring.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Reddit

Reddit rewards people who help and bans people who advertise. Every job here ends in a list the user works through by hand: communities, threads or quotes, each with a link and, where a reply is involved, what the community's rules allow.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_search_posts` and `reddit_get_subreddit` (hosts often add a prefix, for example `mcp__manifold__reddit_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `reddit_*` tools are not, the Reddit group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: every playbook here starts from Reddit.
- AI-cited threads and the strategy also use `aeo_run_ai_answers` and `seo_get_serp`. If the `aeo_*` tools are off, find the threads from Google alone and say AI engines were not checked; if the `seo_*` tools are off, from AI answers alone.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only says "Reddit", open the strategy.

| Job | The user says | Open |
|---|---|---|
| Reddit strategy: which communities, how to take part, and a 90-day plan | "Reddit strategy", "Reddit marketing plan", "how do we grow on Reddit", "should we be on Reddit", "Reddit playbook for our startup" | [references/strategy.md](references/strategy.md) |
| Find subreddits: the communities where the buyers talk, sized and ranked | "which subreddits should we post in", "where does our audience hang out on Reddit", "find subreddits about X", "best subreddits for our product" | [references/find-subreddits.md](references/find-subreddits.md) |
| Subreddit rules: what each community allows before anyone posts | "can I post my product in r/SaaS", "subreddit rules", "is self promotion allowed there", "will I get banned for posting a link" | [references/subreddit-rules.md](references/subreddit-rules.md) |
| Threads to reply: recent threads where a helpful answer fits | "Reddit threads to reply to", "posts asking for a tool like ours", "where should we comment this week", "Reddit conversations to join" | [references/threads-to-reply.md](references/threads-to-reply.md) |
| Pain points: problems people describe in the space, clustered with quotes | "what do people complain about on Reddit", "pain points from Reddit", "customer language from Reddit", "voice of customer on Reddit" | [references/pain-points.md](references/pain-points.md) |
| Competitor complaints: complaints about named competitors and why people switch | "what do people hate about HubSpot on Reddit", "alternative to X threads", "people switching from X", "competitor complaints on Reddit" | [references/competitor-complaints.md](references/competitor-complaints.md) |
| AI-cited threads: the Reddit threads AI engines and Google show for the category | "which Reddit threads does ChatGPT cite", "Reddit threads in AI answers", "Reddit threads Google shows for our keywords", "threads Perplexity uses for our category" | [references/ai-cited-threads.md](references/ai-cited-threads.md) |

## Shared rules

### Search and watching

- **Search is a sample.** `reddit_search_posts` and `reddit_search_comments` return one ranked page per call (1 credit) and are never complete; `meta.cursor` pages the next set. Use them for "what has Reddit said about X". A count from search is a count in the sample: compare like with like (same query, sort and time range) and never report it as a total.
- **Watching is complete.** `reddit_get_new_posts` returns every post in up to 10 subreddits since `since` (at most 7 days), oldest first, with `match` to keep posts whose title or body holds a term. It costs 1 credit per subreddit per page. Read `coverage[]`: `complete: false`, or a `covered_from` later than `since`, means a busy community was cut; raise `pages` (up to 3), shorten `since`, or pass `meta.cursor`. Dedupe rows on `id`.
- **Check that search rows are on topic.** If most titles and bodies do not contain the query's words, the search dropped the query: a search sorted `new` or `top` across all of Reddit has done this. A search with a `time_range` can also come back empty when it should not, comment search most of all. In either case, search again with `sort: "relevance"` and a wider `time_range` (`"all"` if need be), and filter on `created_at` yourself.
- `reddit_search_subreddits` counts the communities behind one page of posts. It answers "where is this discussed", not "every subreddit about X", so run several phrasings and ask the user for communities they know.

### Floors

- **Active community.** Keep a subreddit when people post there daily: `weekly_contributions` above about 100, or at least 7 posts in a 7-day check-in. Subscribers alone mislead: a large community can be dead, and a 20,000-member one that is exactly the ICP beats a general one with millions where a post sinks in an hour.
- **Open thread.** A thread qualifies for a reply only when it is neither `locked` nor `archived` (from `reddit_get_post`) and its body is not "[removed]" or "[deleted]". Fresh beats old: a reply in the first 48 hours is read by the poster and the voters; after 7 days it is read only by search visitors, except threads Google or AI engines show, which stay read for months ([AI-cited threads](references/ai-cited-threads.md)).
- **Rules first.** No playbook suggests a reply or a post before the community's rules are read the way [Subreddit rules](references/subreddit-rules.md) reads them. No rule on self promotion is not permission.

### Evidence

- Every row carries a link: the thread `url`, or the comment's own `url` for a quote. Quote verbatim; do not paraphrase inside quote marks.
- Leave usernames out of research tables. The user needs the thread, not the person, and Reddit users are private individuals.

### Credits

- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Every `reddit_*` call is 1 credit (per subreddit per page for `reddit_get_new_posts`), so most jobs here cost 20 to 60 credits. The expensive call is `aeo_run_ai_answers`: 18 credits per prompt on its five default engines.
- Search, thread and comment results are cached 1 hour and a subreddit's record 7 days; a result this account already paid for is free while cached. `reddit_get_new_posts` is never cached, because it must see what arrived.

### Handoff

- Never post, reply, vote, send messages or schedule. No manifold tool does, and the account at risk is the user's. The deliverable is a table the user works through from their own account.
- Draft a reply or a post only when the user asks. Then: answer the question in the thread first, use a detail from the post, say the user's affiliation plainly ("I work on X"), mention the product only where it answers the question and the rules allow it, and add at most one link.
- The server keeps no state. To check the same communities every day or week, the host schedules `reddit_get_new_posts`, stores `next_since` to send back as `since`, and keeps the ids already seen: that is the `monitoring` group's job.

## Other groups

- Alerts or a daily or weekly watch on Reddit ("alert me when someone mentions us", "every day"): [monitoring brand mentions](../monitoring/references/brand-mentions.md), which reuses these playbooks and adds the schedule.
- Pain points across Reddit, reviews and social comments together: [customers pain points](../customers/references/pain-points.md), which uses this group's pain points.
- Competitors beyond what Reddit says about them (sites, ads, pricing, positioning): the [competitors](../competitors/SKILL.md) group.
- Posting a launch across Reddit, Facebook groups and other communities: [launch communities](../launch/references/communities.md).
- AI visibility beyond Reddit, choosing the prompts, and citation building across every source type: the [ai-search](../ai-search/SKILL.md) group.
- Content ideas and repurposing across channels: the [content](../content/SKILL.md) group.
- Facebook groups: the [facebook](../facebook/SKILL.md) group's group mining.
- No channel chosen yet ("where do I start", "more signups"): the [growth-plan](../growth-plan/SKILL.md) group.
