# A digital PR plan

When the user wants a plan rather than one pitch: the stories that earned competitors coverage, the publications that cover rivals and not the user, a story the user can own, and a 30-60-90 day plan the team's hours can carry.

## Settle first

Ask in one message, and use the default for anything unanswered, saying so.

- **Goal:** links to specific pages, awareness, authority for the domain, or presence in AI answers. Default: authority, counted in referring domains from publications, plus mentions.
- **Stage:** pre-launch, just launched or established. It decides whether launch news is a story at all.
- **Buyers:** what they read decides trade press or national press.
- **Team and hours:** who pitches, who can be quoted, whether anyone can run a survey or analyse data. Default: the founder as spokesperson, three hours a week.
- **Assets:** data the user holds, a launch, a strong view. Default: none, so the first month builds a data story from public data.
- **Competitors:** two or three. Default: the top three from `seo_get_serp_competitors`.
- **Market:** `location` and `language` when not the US and English.

## Baseline and gaps

About 120 credits with Manifold.

1. `seo_get_backlink_summary` on the site and each competitor (10 credits each) for referring domains.
2. `seo_get_backlinks` with `limit: 500` on each competitor (about 18 each): the coverage they earned and what earned it ([media lists](media-lists.md#where-the-reporters-come-from)).
3. `seo_get_referring_domains` on the site (12 credits): which of those publications already link to the user. The rest are the gap.
4. A story to own: `seo_search_keywords` on the category (10 credits) for rising demand in `trend[12]`, and `reddit_search_posts` with `time_range: "year"` for the arguments buyers are having ([stories](stories.md)).
5. Optional: `aeo_run_ai_answers` on five buyer prompts to see which outlets the engines cite ([AI answers](ai-answers.md)).

## Order of work

1. **A story before any pitch.** If the user has no asset and no view, days 1 to 30 build one.
2. **Reactive comment** runs from week one: it is the fastest coverage and needs only a spokesperson and the news.
3. **One data story per quarter**, pitched to its own list of 15 to 25 reporters, then re-pitched as the topic recurs.
4. **Evergreen placements** that need no news (best-of lists, resource pages, broken press links) run alongside; they belong to [link-building](../../link-building/SKILL.md).
5. **Podcasts** when the spokesperson talks well ([podcasts](podcasts.md)).
6. **Launch press** only when there is launch news ([stories](stories.md#launch-news)); its timing across channels is [growth-strategy](../../growth-strategy/SKILL.md).

## The 30-60-90 days

| Period | Typical work | KPI the tools re-measure |
|---|---|---|
| Days 1 to 30 | Baseline; spokesperson bio and topics; reactive pitches on two news moments; one sourcing service, fifteen minutes a day; data story scoped | Pitches sent, replies, first placements; baseline recorded |
| Days 31 to 60 | Data story published with its method page and pitched to its list; five podcast pitches; unlinked mentions from month one chased | Coverage pieces, links to the story page, referring domains from publications |
| Days 61 to 90 | Re-pitch the data to new angles and regions; second reactive cycle; first podcast episodes live; re-measure | Referring domains from publications and branded demand against baseline; AI mention share on the frozen prompts |

Give each period an owner, and three first actions for this week.

## Setting targets honestly

- One story pitched well beats three pitched badly. Keep the plan inside the team's hours.
- Promise inputs (stories, reporters reached, follow-ups), not placements. No one can promise coverage.
- Results usually show in three to six months ([measurement](measurement.md)). A 90-day plan sets the first measurable movement, not the end state.
- Every number in a pitch or a report says where it came from. A journalist checks.
- Never send pitches, answer queries or book shows for the user, and never buy coverage that is not labelled as paid.
