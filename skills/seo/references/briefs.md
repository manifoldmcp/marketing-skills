# A brief for one page

What a writer needs to make one new page that can rank for one keyword: the intent and format Google rewards, the sections readers expect, the related questions, and the angle that gives the page a reason to outrank what is there. Built from page one, not a template. Writing the full article happens only when the user asks, and then from the brief. Checked October 2026.

## What current evidence says a brief must contain

- **An asset or a task, named as a required input.** The traits that most separated winners from losers in 2025 to 2026 were offering a product, letting the visitor complete a task, and proprietary assets such as own data, tools, templates or tests ([updates](google-updates.md#what-winners-had)). Google's AI optimisation guide calls generic tips commodity content. The tools cannot supply this; ask the user what they have, and put it in the brief as something the writer must use.
- **The format the query asks for.** A list only where the query asks for options; a tool where the top results are tools; a how-to with steps where people want it done. A page in the wrong format does not rank by adding words.
- **Who wrote it and how.** A byline the reader can check; for reviews and comparisons, how many products, how they were tested, with evidence. Google's helpful content guidance asks for exactly this.
- **A direct answer near the top** where an AI Overview answers the query: two sentences the overview can quote, then the depth the click came for.
- **No word count target.** Google says it has no preferred length. Give the winners' median `word_count` as context and the sections as the requirement; a 6,000-word outlier is not the bar.
- **Schema for rich results, not for AI.** Add the markup the winners share (Article, FAQPage, HowTo, Product) where it earns a rich result; it has shown no lift for AI citations.

## Before writing one

`seo_get_position` with the keyword and the site (6 credits). If a page of the site already ranks in the top 30, improve that page instead ([quick wins](quick-wins.md)): a second page would compete with the first ([cannibalisation](refresh.md#cannibalisation)). If the user gave a topic, not a keyword, pick it with `seo_search_keywords` first (10 credits).

## Reading page one

- `seo_get_serp` with `ai_overview: true` (2 credits): intent (learn, compare, buy, do), the top three's format, `features` (`featured_snippet` means one short answer wins the top; `video` and forum rows mean people want to see it done or hear peers), and the overview's text and `references[]`, which show what Google already answers and whom it trusts.
- `seo_get_page` on the top five results (free): `title`, `h1`, `h2`, `word_count`, `schema_types`. Headings three or more share are the sections a reader expects.
- `seo_search_keywords` with `mode: "related"` (10 credits) and `seo_get_ranked_keywords` on the top result's URL (10 credits): secondary keywords and the subtopics the winning page is rewarded for. One or two per section; never a page each.
- The angle: what every winner covers the same way, what none covers (a worked example, current data, a template, a comparison, the user's own numbers), and the user's answer to that gap. Covering only what the winners share gives the page no reason to rank above them.

About 28 credits a brief.

## The brief

The target keyword with volume, KD and intent; the format; a working title under about 60 characters and a meta description under about 155; the `h1`; the length range as context; the schema; the angle in two lines; the required inputs from the user (data, screenshots, customer examples, the tester's name); an outline table (section, what it covers, keywords, which winners cover it); and internal links to and from the site's related pages.

For a comparison or alternatives page, read [comparison pages](comparison-pages.md) first.
