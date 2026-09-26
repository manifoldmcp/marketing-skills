# Rank tracking

Google positions for a fixed set of keywords, checked on a schedule and compared with the runs before. The check itself is the `seo` group's [rank check](../../seo/references/rank-check.md): its method, its market settings and how to read `rank`, `organic_rank`, `url` and `above[]`. This playbook adds the schedule, the stored history and the change report. The server keeps no history, so the host stores every run.

## Inputs to settle first

- **Target, keywords, competitors and market**: as in [rank check](../../seo/references/rank-check.md), fixed for every run. Ten to thirty keywords that matter: the money keywords and the pages ranking 4 to 20. For a site's whole footprint rather than a chosen list, see step 5.
- **Method**: chosen once in rank check's first step and kept, so runs compare: `seo_get_position` for one target (6 credits a keyword), or `seo_get_serp` at `depth: 30` when competitors are tracked on the same keywords (3 credits a keyword for every site at once, top 30 only).
- **Cadence**: weekly by default. Positions are cached 24 hours, and day-to-day moves are mostly noise.
- **Budget**: 20 keywords for one target, weekly, cost 20 x 6 = 120 credits a run, about 520 a month. The same 20 with two competitors through `seo_get_serp` at `depth: 30` cost 20 x 3 = 60 a run, about 260 a month. Daily on 20 keywords is about 3,600 a month. Look-closer calls in step 4 add 1 or 2 credits a keyword, and the monthly footprint in step 5 adds 10. Say both numbers before setting up the schedule; pass `max_credits` if the user gave a budget.

## Steps

1. **Set up (first run only).** Settle the inputs and store them with the host, as in the [router](../SKILL.md#schedule-and-state), and set the schedule. Run the check as rank check does and store it as the baseline: per keyword, the date, `rank`, `organic_rank`, `url`, and the domain directly above (or every tracked site's position from the SERP rows).
2. **Run the check.** The same method, keywords, `location`, `language` and `device` as the baseline. `seo_get_position` is 6 credits a keyword; `seo_get_serp` is 1 credit per 10 results of `depth`.
3. **Compare.** Per keyword, against the last run and the baseline:
   - the move in `organic_rank`, which is the number to trend: SERP features come and go and shift `rank` when the site has not moved;
   - crossing into or out of the top 3, the top 10 or the top 100 (`rank` null);
   - a change of ranking `url`;
   - a new domain directly above.
4. **Look closer at the big moves.** For each keyword over the router's [thresholds](../SKILL.md#change-not-level), `seo_get_serp` at the default depth (1 credit, +1 with `ai_overview: true`) to see what ranks now and which features appeared. Many keywords falling on the same run points to the `seo` group's [traffic drop](../../seo/references/traffic-drop.md); one page slipping over several runs, to its [content refresh](../../seo/references/content-refresh.md).
5. **Monthly footprint (optional).** Once a month, `seo_get_ranked_keywords` on the target (10 credits for 100 rows): keywords the site newly ranks for and ones that left, against last month's stored list. It is an index snapshot, not a live check, so it complements the tracked set rather than replacing it.
6. **Deliver and store.** A table: keyword, organic rank now, last run, change, best since the baseline, rank (with features), ranking URL (flagged when it changed), who sits directly above, and a note. One summary line: keywords up, down and unchanged, and the count in the top 3 and top 10 against last run. The host appends the run to its history (date, keyword, `rank`, `organic_rank`, `url`, the domain above) with the credits it cost.

## Judgment

- A move of one or two places is noise: data centres, personalisation and SERP features shuffle results daily. Act on a move of three or more that holds for two runs, or on any crossing of page one.
- Weekly is enough for almost every site. Daily costs seven times as much and leads to the same decisions; the 24-hour cache means two checks in one day return the same result anyway.
- `rank` null means not in the top 100 scanned, not removed from Google. Check the ranking `url` of the last run before calling it a loss.
- A ranking URL that flips between two of the user's pages from run to run is cannibalization: two pages competing for one keyword. That is a job for the `seo` group, not a tracking error.
- Keep the set fixed. A keyword added later has its own baseline from its first run; report it apart until it has history.
- The history lives with the host. If the store is lost, so is the trend: offer to keep it in a sheet or a file the user owns.
