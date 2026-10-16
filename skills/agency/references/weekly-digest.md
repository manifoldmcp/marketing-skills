# Weekly digest

One recurring document a week on the user's own marketing (or a client's, between monthly reviews): Search Console clicks, new brand mentions, competitor changes, rank moves and AI visibility, each with the change from last week, led by at most three things worth acting on. Each section's method lives in its topic; this page is how they fit into one weekly read. Checked October 2026.

## Sections and where their method lives

| Section | Method | Weekly by default |
|---|---|---|
| Search Console clicks and impressions | [seo](../../seo/SKILL.md) | Yes, only when connected |
| New brand mentions | [brand-monitoring](../../brand-monitoring/SKILL.md) | Yes (or daily on its own schedule; the digest then reads the stored week) |
| Competitor changes: ads, posts, traffic, top pages | [competitor-intel](../../competitor-intel/SKILL.md) | Yes |
| Google rank moves on the tracked keywords | [seo](../../seo/SKILL.md) | Yes |
| AI visibility on the tracked prompts | [ai-search](../../ai-search/SKILL.md) | Every other week; off weeks show the last measured date |

A section never run before starts with its baseline that week, and says so.

## Cost

At each topic's defaults a run costs about 0 (Search Console) + 26 (mentions) + 41 (competitors) + 120 (20 ranks) = 187 credits, or 367 in the weeks AI visibility runs (+180 for 10 prompts on five engines): about 1,200 credits a month. Daily mentions instead of weekly raise it to about 1,570. Say both numbers before setting the schedule, and pass `max_credits` on every call so no week overspends. Store `meta.credits_charged` from every response, so the monthly cost is measured, not guessed (a month is about 4.3 runs).

Rank tracking and AI visibility are the largest lines. Trim the keyword set to the ones that matter before dropping a section, and keep AI visibility fortnightly: its week-to-week moves are mostly noise.

`reddit_get_new_posts` and `aeo_run_ai_answers` are never cached and are charged in full every run. The rest are free when the account already paid for the same result inside the cache window: 1 hour for Reddit searches and threads, 6 hours for other platform searches and listings, 24 hours for profiles, positions, SERPs and ad lists, 7 days for domain overviews and most search data. Console and Analytics reads are free at any cadence. `dry_run: true` prices any call.

## What counts as a change

Changes first: a digest that restates levels every week stops being read by the third one. Flag only above the noise:

- a rank move of 3 or more positions, or one that crosses into or out of the top 3 or top 10;
- an AI mention share that moves 10 or more points, or a smaller move that holds for three runs;
- Search Console clicks for the last 7 days 20% or more below the weekly average of the 21 days before (a rule of thumb, not a tested threshold);
- any new ad id from a competitor; a competitor post at twice that account's median engagement;
- a mention that is a complaint or question with reach, or by a journalist or creator the user named (the rest go in the digest body);
- any `aeo_get_site_readiness` check with `tier: "blocks_citations"` newly showing `ok: false` (free; a CDN or firewall now turning AI crawlers away goes to the top).

Estimated SEO traffic moves slowly and is a model: report it month over month inside the competitor section, never as a weekly headline. When Google Analytics is connected, add organic sessions and key events for the same windows (`analytics_get_report`, free), so a fall in clicks shows whether it cost conversions.

## Shape

- **Headline table**: metric, this week, last week, change, baseline, for Search Console clicks, new mentions, keywords in the top 10, AI mention share, and the count of competitor moves.
- **Up to three actions**, each tied to a row and to the topic that acts on it: a thread worth an answer ([reddit](../../reddit/SKILL.md)), a competitor push ([competitor-intel](../../competitor-intel/SKILL.md) or [paid-ads](../../paid-ads/SKILL.md)), a page that fell off page one ([seo](../../seo/SKILL.md)), a prompt where a competitor took the user's place ([ai-search](../../ai-search/SKILL.md)).
- **Each section's change table**; a quiet section is one line.
- **The week's credits by section.**

A section whose tool group is switched off or whose call failed says "not measured this week", never zero.

## Schedule and state

- The host runs the schedule. If it has a scheduler (a scheduled task, a routine, cron), set the digest up for the report day (default Monday morning). If not, say so: the user asks each week and the host reruns with the stored state.
- The host stores each section's settings (terms, communities, keywords, prompts, engines, competitors, `location`, `language`, `device`) and each run's numbers where the next run can read them. Every run uses the same settings; a changed setting starts a new baseline for what it changed, shown apart until it has history. Without stored state every run is a first run.
- Acting on a row (replying, answering a competitor, changing a page) is the user's call. The server sends nothing; if the host has an email or chat tool, offer to send the digest there.
