# Lists that rank on Google

A "best X tools" article on Google's first page sends buyers and a link at the same time. Here the lists are chosen by where they rank for the category.

## Find the lists

1. **Find the list keywords.** `seo_search_keywords` with `seed: "best <category>"` (10 credits for 100 rows), and once more with `seed: "<top competitor> alternatives"` (10 credits). Keep keywords that read as a list: best, top, alternatives, vs, tools, software, apps. Sort by volume and keep the top 10.
2. **Pull the first page.** `seo_get_serp` for each of the 10 keywords (1 credit each at the default depth of 10). Keep the results whose title reads as a list and whose domain is a publisher, not a vendor. Reddit threads and YouTube videos on the page belong to [build-ai-citations](../../build-ai-citations/SKILL.md), which covers what Google shows as well; note them.

## Rank

- `seo_get_domain_ratings` with every list domain in one call (4 credits). Apply the domain rank [floor](../../create-link-building-plan/references/outreach.md#floors).
- Rank what is left: first the lists that name two or more competitors and not the user, then by the keyword's volume and the list's position on the page.

## Columns

Keyword, monthly volume, position, domain rank.

## Judgment

- The position on the first page is the proof a list is worth the effort. A list that dropped off page one is not.
- A list the user is already on is not a target, but its position is worth reporting: if it ranks, ask the editor to update the entry rather than add it.
- In a new category with no "best X" lists, use "alternatives to <incumbent>" keywords: those lists exist earlier.
