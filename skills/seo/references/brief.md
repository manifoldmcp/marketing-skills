# Content brief

A brief tells a writer what one new page must cover to rank for one keyword: the intent and format Google rewards, the sections the winners share, the related keywords, and the angle that makes the page better than what ranks. It is built from page one, not from a template. For a page that already exists, use [optimize a page](optimize-page.md) instead.

## Inputs to settle first

- **Keyword**: the one query the page targets. If the user gives a topic, pick the keyword with `seo_search_keywords` first (10 credits) and confirm it.
- **Site**: the user's domain, to check it has no page for this already and to suggest internal links.
- **Angle**: what the user knows or has that the ranking pages do not: data, a product, a template, first-hand experience. Ask; it is the one thing the tools cannot supply.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default brief costs about 6 + 2 + 10 + 10 = 28 credits; `seo_get_page` is free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Check the site has no page for it.** `seo_get_position` with the keyword and the site as `target` (6 credits). If a page of the site already ranks in the top 30, the job is to improve that page: switch to [optimize a page](optimize-page.md). A brief for a second page would compete with the first.
2. **Read page one.** `seo_get_serp` with `ai_overview: true` (2 credits). Read the intent (learn, compare, buy, do), the format of the top three, `features` (`featured_snippet` means one short answer wins the top spot; `video` and forum rows with `type: "discussions_and_forums_element"` mean people want to see it done or hear from peers), and the `ai_overview` text and `references[]`: what Google already answers, and which sources it trusts.
3. **Read the winners.** `seo_get_page` on the top five organic results (free, rate limited). From each: `title`, `h1`, `h2` headings, `word_count` and `schema_types`. Headings three or more of them share are the sections a reader expects. The median `word_count` sets the length range; the schema they share (FAQPage, HowTo, Article) is the markup to add.
4. **Collect the related keywords.** `seo_search_keywords` with the keyword as `seed` and `mode: "related"` (10 credits): the secondary keywords and questions that belong on this page, one or two per section. Then `seo_get_ranked_keywords` on the top result's URL (10 credits, the URL as `target`): the other queries the winning page ranks for, which are the subtopics Google rewards.
5. **Find the angle.** From the headings: what every winner covers the same way, and what none covers (a worked example, current data, a template, a comparison, the user's own numbers). The brief's angle is the user's answer to that gap.
6. **Deliver** the brief: the target keyword with volume, KD and intent; the format; a working title under about 60 characters and a meta description under about 155; the `h1`; the target length range; the schema to add; the angle in two lines; then an outline table: section heading, what it covers, keywords to use, which winners cover it. End with internal links: the site's related pages to link from and to (the host or user knows the site; `seo_get_ranked_keywords` on the site, 10 credits, finds the pages ranking for related keywords if needed).

## Judgment

- Cover what the winners share, then add what they lack. Copying their headings gives a page no reason to rank above them.
- Use the median length, not the longest page. A 6,000-word outlier is not the bar.
- If page one is all tools, videos or forum threads, a written page is the wrong format. Say so in the brief rather than write it anyway.
- Where an `ai_overview` answers the query, put a direct two-sentence answer near the top: it is what gets quoted, and readers who click want more than the overview.
- Put the user's own evidence (numbers, screenshots, customer examples) in the brief as required inputs. It is what makes the page better and citable, and the tools cannot supply it.
- The brief is the deliverable. Write the article only when the user asks, and then from the brief.
