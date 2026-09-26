---
name: monitoring
description: Recurring marketing monitoring with the manifold tools. Watches new brand mentions on Reddit, TikTok, YouTube, LinkedIn and Instagram, tracks what competitors changed (new ads, new posts, search footprint), tracks Google rankings for a fixed keyword set, tracks how often ChatGPT, Gemini, Claude, Perplexity and Google AI Overviews mention or cite the brand for fixed prompts, and combines them in a weekly report with week-over-week change. Use when the user asks to monitor, track, watch, keep an eye on or be alerted about something daily, weekly or monthly, for social listening, brand monitoring, mention alerts, competitor alerts, ongoing competitive intelligence, rank tracking or a position tracker, AI visibility or share of voice over time, or a weekly marketing report or digest. A one-off check belongs to the source group, a client's monthly report to agency. The server has no scheduler and keeps no state; the host schedules each run, stores the state between runs and sends any alert.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Monitoring

Every job here is a job from another group, run again on a schedule and compared with the last run. The manifold server has no scheduler and keeps no state, so each playbook settles two things with the host before the first run: when it runs, and what it stores for the next one. The playbooks link to their source playbook for the what and add the schedule, the state and the change report.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `reddit_get_new_posts` and `seo_get_position` (hosts often add a prefix, for example `mcp__manifold__seo_get_position`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The playbooks cross tool groups: `reddit_*` and the platform groups for mentions and posts, `ads_*` for ads, `seo_*` for ranks and search footprint, `aeo_*` for AI answers. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run what the rest allow, and mark those sections "not watched" in every report rather than reporting zero.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. A recurring request with no single subject ("keep an eye on things for us") opens the weekly report.

| Job | The user says | Open |
|---|---|---|
| Brand mentions: new posts and videos naming the brand since the last run | "alert me when someone mentions us", "monitor Reddit for our brand", "social listening", "track mentions of Acme every day", "who talked about us this week" | [references/brand-mentions.md](references/brand-mentions.md) |
| Competitor watch: what competitors changed since the last run, in ads, posts and search | "watch our competitors", "alert me when a competitor launches new ads", "weekly competitor updates", "track what Rival posts", "competitive intel every Monday" | [references/competitor-watch.md](references/competitor-watch.md) |
| Rank tracking: Google positions for a fixed keyword set, run on a schedule, with the change | "track our rankings", "rank tracker", "weekly keyword positions", "monitor where we rank for X", "alert me if we drop off page one" | [references/rank-tracking.md](references/rank-tracking.md) |
| AI visibility tracking: how often AI answers mention or cite the brand for fixed prompts, run over run | "track our AI visibility", "monitor ChatGPT mentions over time", "AI share of voice every month", "are we showing up in Perplexity more", "track AI search" | [references/ai-visibility-tracking.md](references/ai-visibility-tracking.md) |
| Weekly report: the other jobs combined, with week-over-week change | "weekly marketing report", "Monday digest", "what changed this week", "one weekly summary of mentions, ranks and competitors", "keep an eye on everything" | [references/weekly-report.md](references/weekly-report.md) |

## Shared rules

### Schedule and state

- The host runs the schedule. If it has a scheduler (a scheduled task, a routine, a cron job), set the playbook up there with the cadence the user chose. If it has none, say so: the user asks again each day or week, and the host reruns the playbook with the stored state.
- The first run is the baseline. Nothing in it is "new"; it sets what later runs compare against. Say so in the first report.
- Each playbook names the state it needs, and the host stores it where the next run can read it (a file, a doc, a sheet, its memory): `next_since` for Reddit, the ids already seen, previous ranks, previous answers, previous ad ids, previous numbers. Without the stored state every run is a first run.
- Store the settings with the state: the terms, communities, keywords, prompts, engines, competitors, `location`, `language` and `device`. Every run uses the same ones. A changed setting starts a new baseline for what it changed; report those rows apart until they have history.
- Store each run's date and the credits it cost (`meta.credits_charged` on every response), so the monthly cost is measured, not guessed.

### Cadence and cost

- Each playbook states its cost per run and per month. A month is 30 daily runs, about 4.3 weekly runs or about 2.2 fortnightly runs. Say both numbers before setting up the schedule, and pass `max_credits` on every call so no run overspends. `dry_run: true` prices any call for free.
- Match the cadence to how fast the thing moves and how fast the user will act: mentions daily or weekly, competitors weekly, ranks weekly, AI answers every one or two weeks. Running more often than the user acts on the result only multiplies the cost.
- Two tools are never cached and are charged in full on every run: `reddit_get_new_posts` and `aeo_run_ai_answers`. The rest are free when this account already paid for the same result inside the cache window (6 hours for platform searches and listings, 24 hours for positions and ad libraries, 7 days for domain overviews), so a rerun inside the window returns the same numbers for nothing and adds nothing new.

### Change, not level

- Lead with what changed since the last run: new rows, moves, drops, rows that disappeared. Levels come second, as context.
- Flag a change only above the noise: a rank move of 3 or more positions, or one that crosses into or out of the top 3 or the top 10; an AI mention share that moves 10 or more points, or smaller moves that hold for three runs; any new mention in a watched community; any new ad id; a competitor post at twice that account's median engagement.
- A quiet run is one line ("no new mentions since Tuesday"). Do not pad it.

### Handoff

- The server never sends alerts, emails or posts. The deliverable is the change table. If the host has an email, chat or notification tool, offer to send the table there; the host sends it, not the server.
- Replying to a mention, answering a competitor or changing a page is the user's call. Each playbook points to the group that acts on a row.

## Other groups

- The one-off version of each job: [seo rank check](../seo/references/rank-check.md), [ai-search visibility check](../ai-search/references/visibility-check.md), [competitors teardown](../competitors/references/teardown.md), [paid-ads competitor ads](../paid-ads/references/competitor-ads.md), and Reddit's [find subreddits](../reddit/references/find-subreddits.md) to choose communities to watch.
- A monthly report for an agency's client: [agency monthly report](../agency/references/monthly-report.md), which can build on the weekly report here.
- The reaction to a launch, measured once a week or so after it: [launch reaction report](../launch/references/reaction-report.md).
- Acting on what a run finds: Reddit threads worth an answer, [reddit threads to reply](../reddit/references/threads-to-reply.md); pages that lost rank, [seo](../seo/SKILL.md); sources AI engines cite for competitors, [ai-search citation building](../ai-search/references/citation-building.md).
- What customers complain about across sources, as research rather than an alert: [customers pain points](../customers/references/pain-points.md).
