# PR strategy

Digital PR earns links and mentions with stories: data, expert comment and launches that a journalist can use. The strategy finds which stories earned competitors their best coverage, what story the user can own, and turns that into a 90-day plan that the journalists playbook can execute.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: links to specific pages, brand awareness, or authority for the whole domain. Default: authority for the domain, measured in referring domains from publications.
- **Stage**: pre-launch, just launched or established. It decides whether launch news is a story at all.
- **ICP**: who buys, so the plan targets what they read (trade press or national press).
- **Budget**: credits for the research (this playbook costs about 120) and hours a week for pitching. Default: 1,000 credits and 3 hours a week.
- **Team**: who pitches, who can be quoted as an expert, and whether anyone can run a survey or analyse data. Default: the founder, quoted as the expert.
- **Competitors**: two or three. Default: the top three from `seo_get_serp_competitors`.
- **Assets**: data the user holds (usage numbers, a customer survey), a launch, a strong opinion. Default: none, so the plan builds one from public data.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 95 credits. `seo_get_backlink_summary` on the site and each competitor (10 credits each) for referring domains. `seo_get_backlinks` with `limit: 500` on each competitor (18 credits each): keep the rows from publications, and read their `page_title` to name the stories that earned each competitor coverage (a data study, a funding round, a founder quote, a free tool).
3. **Gaps.** About 25 credits more. `seo_get_referring_domains` on the site (12 credits) shows which of those publications already link to the user; the rest are the gap. Then look for a story the user can own: `seo_search_keywords` on the category (10 credits) and read the `trend[12]` of the biggest terms for rising or falling demand, and `reddit_search_posts` with the category and `time_range: "year"` (1 credit a page) for the arguments people are having. A trend or a debate with numbers behind it is a data story.
4. **Tactics.** Choose from this group, and say why each fits:
   - [Journalists](journalists.md) builds the media list for each story. Every story in the plan runs through it.
   - [Best-of lists](best-of-lists.md) for evergreen placements that do not need news.
   - [Lost links](lost-links.md) to recover press links the site already earned and broke.
   - [Backlink targets](backlink-targets.md) for resource pages that link to data once it exists.
   - For press around a launch day, the [launch](../../launch/references/press.md) group's press playbook.
5. **Plan.** A 30-60-90 day plan: by day 30, one reactive pitch (expert comment on a trend from step 3) to a media list from the journalists playbook; by day 60, one data story with its own list; by day 90, a second story and the reclaim of any broken press links. Give each period an owner and a KPI.
   - KPIs the tools can measure again later: referring domains from publications (`seo_get_referring_domains` on the site, filtered to the publication domains), links to the story page (`seo_get_backlinks` on its URL), and branded search demand (`seo_get_keyword_metrics` on the brand name, `trend[12]`).
   - **Deliver** one document: the inputs with defaults marked, the stories that earned each competitor coverage, the publication gap, two or three story ideas with the data behind each, the 30-60-90 table, and three first actions for this week.

## Judgment

- No story, no coverage. If the user has no asset and no opinion, the first 30 days build one from public data before any pitch goes out.
- Data from these tools is public, so say where every number came from. A journalist checks.
- Reactive comment is fastest: a trend in the news plus a quotable founder. A data study takes longer and earns more links.
- Coverage without a link still counts for brand demand and for AI answers. Report mentions, not only links.
- Keep the plan to what the team's hours allow. One story pitched well beats three pitched badly.
