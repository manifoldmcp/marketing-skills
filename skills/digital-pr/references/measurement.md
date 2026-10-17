# Measuring digital PR

What to report after coverage runs, how to re-measure it with the tools, and the traps in each number. Checked October 2026.

## What practitioners count

In BuzzStream's practitioner surveys (150+ each, 2025 and 2026) the top measures were quality links (85% to 87%), total mentions (72%) and total links (71% to 73%); qualified leads and sales were reported far less often. About half see results in three to six months, a third in one to three. Set the user's expectations on that clock, not on launch week.

## The numbers to report

| Measure | How to re-measure | Trap |
|---|---|---|
| Pieces of coverage, linked and unlinked | The coverage log the user keeps; `seo_get_backlinks` on the story page; unlinked mentions via [link-building](../../link-building/SKILL.md) or [brand-monitoring](../../brand-monitoring/SKILL.md) | The link index misses unlinked coverage and lags new links by days to weeks |
| Referring domains from publications | `seo_get_referring_domains` on the site (12 credits), filtered to the publication domains, against the baseline | Domain totals move for many reasons; compare publication domains only |
| Links to the story page | `seo_get_backlinks` with the page URL as `target` | Syndicated copies inflate the count: report original publishers apart |
| Link type | `dofollow` and `rel[]` on each backlink row | Nofollow and sponsored links still bring readers and mentions; report them apart, do not drop them |
| Branded demand | `seo_get_keyword_metrics` on the brand name with `ai_volume: true` (about 10 credits): `trend[12]` for Google, `ai_trend[12]` for AI engines | Monthly and modelled; a single story rarely shows. Read it over a quarter |
| AI answers naming the brand | `aeo_run_ai_answers` on a frozen prompt set (27 credits a prompt on five engines); see [ai-search](../../ai-search/SKILL.md) | Answers are noisy: report shares with a margin of error, act on gaps seen in two or more cells |
| Referral traffic and signups | The user's analytics, by referring domain | Most readers of coverage never click; this undercounts the effect |

## Rules

- **Baseline first.** Referring domains from publications, branded demand and AI mention share before the first pitch, or nothing afterwards can be attributed.
- **Count stories, not copies.** The same article on twenty syndication sites is one placement. Practitioners split on this (53% count syndicated links in totals, BuzzStream 2025); say which way the report counts.
- **Count mentions.** Coverage that names the user without a link still feeds AI answers and branded search. The easiest follow-up link is asking the author of an unlinked mention to add one ([link-building](../../link-building/SKILL.md)).
- **Report against the user's own baseline,** never against an industry "average links per campaign": no trustworthy one exists (see [stories](stories.md#benchmarks)).
- **The server keeps no state.** The host keeps the coverage log and each re-measure with its date and `meta.credits_charged`; without it every report starts from zero.
