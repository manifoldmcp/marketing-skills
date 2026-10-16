# Tracking rankings and watching Search Console

Where a site ranks for a fixed set of keywords, and whether its own Google clicks are falling, checked once or on the host's schedule. The server keeps no history and sends no alerts: the host stores every run and delivers the change. Checked October 2026.

## A rank check

Pick the cheapest method that answers the question:

| Question | Method | Cost |
|---|---|---|
| One site, up to about 15 keywords | `seo_get_position` per keyword: rank, ranking `url`, `above[]` | 6 credits each |
| Several sites on the same keywords | `seo_get_serp` with `depth` 20 or 30: every site's position on one page | 2 or 3 credits a keyword |
| Every keyword a site ranks for | `seo_get_ranked_keywords`: an index snapshot, not live | 10 per 100 rows |
| The user's own site | `console_get_search_analytics` by query (add `device`): the average Google recorded | free |

- Pass the same `location`, `language` and `device` on every call. A rank is one location on one device; the user's own search is personalised and will differ.
- For a business that serves a town, track the search with the town in it ("dentist columbus ohio"). `location` localises the local pack but not the organic rows of a bare search: "dentist" at the United States, Columbus and Seattle returned no organic page about the searched city, and four of nine were the same in all three (October 2026). The pack and Maps go to [local-seo](../../local-seo/SKILL.md).
- A domain target counts every subdomain; pass the host (`blog.example.com`) to check one.
- Results are cached 24 hours: a second check the same day returns the same answer, free.
- Search Console's average blends every variant, location and device over the window. A 9.6 there and a 6 live can both be right; report it beside the live check, not instead. Since September 2025 its average position reads better than before for most sites with no real change ([Search Console](search-console.md#the-september-2025-break)).
- Read [rank against organic rank](keyword-data.md#reading-rank). The wrong page ranking (a blog post for a product query, an old URL after a migration) is a finding in itself.

## On a schedule

- **Fix the set and the method once:** ten to thirty keywords that matter (money keywords and pages at 4 to 20), the competitors, the market. A keyword added later starts its own baseline and is reported apart.
- **Weekly by default.** Daily costs seven times as much and leads to the same decisions.
- **Cost:** 20 keywords for one site with `seo_get_position` is 120 credits a run, about 520 a month weekly. The same 20 with two competitors through `seo_get_serp` at `depth: 30` is 60 a run, about 260 a month (top 30 only). Daily on 20 keywords is about 3,600 a month. Say both numbers before setting it up, and pass `max_credits` on every call.
- **Store per run:** the date, keyword, `rank`, `organic_rank`, `url`, the domain directly above, and `meta.credits_charged`. The first run is the baseline; nothing in it is a change.
- **Compare** `organic_rank` (features come and go and shift `rank`), crossings into or out of the top 3, top 10 and top 100, a changed ranking `url`, and a new domain directly above.
- **Noise:** a move of one or two places is data centres, personalisation and features. Act on three or more that holds two runs, or any crossing of page one. A null `rank` is "not in the top 100 scanned", not removed from Google.
- For a big move, `seo_get_serp` at default depth (1 credit, +1 with `ai_overview: true`) shows what ranks now and which features appeared. Many keywords falling on one run is a [technical-seo](../../technical-seo/SKILL.md) diagnosis, especially near a [Google update](google-updates.md#confirmed-updates); one page slipping over several runs is a [refresh](refresh.md); the ranking URL flipping between two of the user's pages is [cannibalisation](refresh.md#cannibalisation).
- Once a month, optionally, `seo_get_ranked_keywords` on the target (10 credits): keywords newly ranking and ones that left, against last month's stored list; `is_new` and `previous_rank` give movement since the index's previous update when there is no stored list.

## Watching Search Console

Free, so it can run daily for a site where a lost day costs money; weekly for the rest. It has no estimate to fall back on: without the connection, it cannot run.

- **Baseline (first run):** `dimensions: ["date"]` for 28 days and the same 28 days a year earlier (16 months are kept), plus the top pages and queries. Watch up to 20 money pages and the brand queries on their own lines.
- **Each run:** the last 7 days against the weekly average of the 21 before, and each day against the same weekday a week earlier (B2B sites dip every weekend). **Flag clicks 20% or more below** (a rule of thumb). Lead with what changed; a quiet run is one line.
- **When it flags,** pages then queries for the last 7 days and the 7 before (`limit: 1000`), joined and ranked by clicks lost, and read by what moved with them:
  - `position` fell: a ranking loss, for [technical-seo](../../technical-seo/SKILL.md) to diagnose.
  - `impressions` fell with `position` steady: demand fell (season, news, a trend ending); check the year-earlier baseline. Year over year across September 2025, compare clicks only.
  - `impressions` and `position` steady, `ctr` fell: the results page changed. `seo_get_serp` with `ai_overview: true` (2 credits) on the top query shows whether an AI Overview or another feature now sits above, and whether it cites the user.
- **Pages that vanished:** `console_inspect_url` (free, Google only, 2,000 a day per property shared by everyone on it): a `verdict` other than PASS, a changed `google_canonical` or a `robots_txt_state` block is a developer fix, not content. Weekly, `console_get_sitemaps`: new `errors`, or a `last_read_at` that stopped moving. `urls_indexed` reads 0 from Google today; ignore it.
- Average position moving with flat clicks is usually a mix change (new long-tail queries ranking low), not a loss.
- A rerun before Google's data moves (about two days' lag) adds nothing.

## Schedule and delivery

If the host has a scheduler (a scheduled task, a routine, cron), set the run up there; if not, say so, and the user asks again each week while the host reruns with the stored state. Store settings and history where the next run can read them (a file, a sheet, a doc, its memory), and offer a sheet the user owns, since a lost store loses the trend. If the host has an email, chat or notification tool, offer to send the change table there; the server never sends.

For one weekly report across SEO, AI answers, mentions and competitors, see [agency](../../agency/SKILL.md).
